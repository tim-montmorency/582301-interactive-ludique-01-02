# Niveaux et progression

## Comment passer d'un niveau à l'autre
Cette fiche explique des patterns simples et robustes pour implémenter la progression de niveau (level-to-level) dans un jeu 2D — en s'appuyant sur l'exemple Godot fourni dans le dépôt `exemple--multi-niveaux` (liens en bas).


Objectif : montrer comment organiser le passage de scènes (niveaux), gérer l'état du joueur et préserver la cohérence du gameplay.

Voir aussi : `03-savoirs/03/05-transition-scene/README.md` — pour l'explication des Autoloads et la gestion des nœuds persistants (HUD, audio, etc.).

---

## 1. Exemple concret (référence)
Les extraits suivants proviennent du projet d'exemple :

- autoload `Main` (fichier project.godot indique `Main="*res://scripts/main.gd"`)
- `scripts/main.gd` — fonction utilitaire pour changer de scène :

```gdscript
extends Node

func changer_scene(nouvelle_scene:PackedScene):
	print("changer scene")
	get_tree().call_deferred("change_scene_to_packed", nouvelle_scene)
```

- `scripts/porte_niveau.gd` — script d'un `Area2D` (porte) qui change la scène quand le joueur entre :

```gdscript
# Dans un noeud Porte basé sur un Area2D
extends Area2D

@export var nouvelle_scene:PackedScene

func _on_body_entered(body: Node2D) -> void:
	if body is Joueur:
		Main.changer_scene(nouvelle_scene)
```

- `scripts/joueur.gd` — `CharacterBody2D` qui expose un `etat_courant` (machine d'état simple) ; le joueur est identifié par la classe `Joueur`.

```gdscript
class_name Joueur
extends CharacterBody2D

@export var etat_courant = Etat.REPOS
enum Etat { REPOS, PROMENER, SAUT, GLISSE_MUR }
```

Les scènes incluent un `PorteNiveau` (Area2D) avec un `CollisionShape2D` et la propriété `nouvelle_scene` pointant sur la scène suivante.

---

## 2. Pattern minimal pour la progression par porte (level gate)
Étapes :

1. Placez une `Area2D` (porte/trigger) avec un `CollisionShape2D` à l'endroit où on doit changer de niveau.
2. Exposez la `PackedScene` cible (`nouvelle_scene`) en export sur le script du trigger.
3. Lors du signal `body_entered`, vérifiez que l'objet est bien le joueur (type, groupe ou propriété).
4. Appeler un gestionnaire central (autoload) pour effectuer le changement de scène de façon sûre (déféré si nécessaire).

Pourquoi utiliser un autoload (Main) ?
- Centralise la logique de changement de scène.
- Permet de faire des préparations (sauvegarde d'état, fade out, statistiques) avant le changement.
- `get_tree().change_scene_to_packed()` doit idéalement être appelé de façon différée depuis un signal pour éviter les conflits de pile d'exécution — d'où `call_deferred` dans `Main.changer_scene`.

Code minimal du trigger (bonnes pratiques) :

```gdscript
func _on_body_entered(body: Node) -> void:
	# Vérifier soit par type, soit par groupe
	if body is Joueur or body.is_in_group("player"):
		# Option : désactiver le trigger pour éviter retriggers
		set_monitoring(false)
		# Option : jouer animation/son puis changer
		Main.changer_scene(nouvelle_scene)
```

---

## 3. Variantes utiles pour la progression

- Gating (verrouillage par condition) : ne changer de niveau que si le joueur possède un item ou a rempli une condition.

```gdscript
if body.has_method("has_key") and body.has_key("gold_key"):
	Main.changer_scene(nouvelle_scene)
```

- Téléportation avec transition : animer la disparition du joueur, sauvegarder l'état, puis changer la scène.

- Checkpoints et retour : stocker la position/restart point dans un autoload ou un fichier pour reprendre en cas de mort.

- Branches de niveaux : `nouvelle_scene` peut être choisi dynamiquement selon l'état du jeu (choix du joueur, score, etc.).

## 4. Gestion de l'état et persistance

Pour des jeux multi-niveaux plus complexe, il peut être nécessaire de conserver des informations entre les scènes ex:

- progression (niveau atteint)
- objets collectés
- variables globales (vie, points)

Une approche est d'utiliser un autoload `GameState`/`Main` qui contient un dictionnaire d'état, et l'écrire sur disque pour la persistance.

Exemple simple :

```gdscript
# Autoload GameState.gd
extends Node

var state = {
	"niveau_actuel": 0,
	"items": [],
}

func save(path: String = "user://save.json"):
	var f = File.new()
	f.open(path, File.WRITE)
	f.store_line(to_json(state))
	f.close()

func load(path: String = "user://save.json"):
	var f = File.new()
	if f.file_exists(path):
		f.open(path, File.READ)
		state = parse_json(f.get_line())
		f.close()
```

Appeler `GameState.state["niveau_actuel"] = X` avant de changer de scène permet de retrouver la progression.

---

## 5. Intégration avec la machine d'état du joueur

Le `Joueur` de l'exemple possède un `etat_courant` (REPOS, PROMENER, SAUT...). Utilisez ces états pour contrôler :

- animations (changer animation selon l'état)
- capacités temporaires (verrouiller le saut lors d'une cutscene)
- interactions (ne pas autoriser certains triggers si le joueur est en état particulier)

Astuce : dans le trigger, vous pouvez interroger `body.etat_courant` pour décider si le passage est autorisé (ex. ne laisser passer que si le joueur est au sol).

```gdscript
if body is Joueur and body.etat_courant != body.Etat.SAUT:
	Main.changer_scene(nouvelle_scene)
```

---

## 6. Scénarios avancés (design de niveaux)

- Niveaux modulaires : charger des scènes petites (modules) et composer la carte à la volée.
- Transition douce : cross-fade visuel + audio pour améliorer l'expérience.
- Niveaux dépendants d'objets : débloquer portes si un ensemble d'objectifs est accompli.
- Multiplay / persistance réseau : synchroniser l'état de progression côté serveur.

---

## 7. Tests et protocole de validation

- Test 1 — Trigger fiable : spawn player et entrer volontairement dans la porte 10 fois ; vérifier qu'il n'y a pas de crash ni de double-changement.
- Test 2 — Gating : essayer de passer sans clef, puis avec la clef ; vérifier comportement attendu.
- Test 3 — Sauvegarde/restaure : sauvegarder l'état, relancer le jeu et vérifier que `niveau_actuel` est correctement restauré.

---

## 8. Liens et ressources

- Exemple utilisé (project file) : https://raw.githubusercontent.com/egl-edu/exemple--multi-niveaux/refs/heads/main/projet_godot/project.godot
- `main.gd` : https://raw.githubusercontent.com/egl-edu/exemple--multi-niveaux/refs/heads/main/projet_godot/scripts/main.gd
- `porte_niveau.gd` : https://raw.githubusercontent.com/egl-edu/exemple--multi-niveaux/refs/heads/main/projet_godot/scripts/porte_niveau.gd
- `joueur.gd` : https://raw.githubusercontent.com/egl-edu/exemple--multi-niveaux/refs/heads/main/projet_godot/scripts/joueur.gd
- Scènes exemples : `niveau_00_initiale.tscn`, `niveau_01_wall_jump.tscn` dans le même repo (voir `scenes/`)

---

Cette page vise à fournir un guide pratique et directement exploitable pour intégrer la progression de niveau dans vos projets Godot. Si vous voulez, je peux :

- ajouter un exemple de transition visuelle (fade) à intégrer dans `Main.changer_scene`,
- ajouter un autoload `GameState.gd` pleinement implémenté et relié aux scènes, ou
- transformer les tests en checklist imprimable pour les séances de TP.

```

