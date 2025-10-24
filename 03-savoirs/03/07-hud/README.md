# Intégration d’une interface graphique HUD («Head-Up Display»)

## Fiche : rôle du HUD et tendance vers le minimalisme

Rôle principal du HUD
- Fournir au joueur les informations nécessaires pour prendre des décisions (vie, énergie, munitions, objectifs, feedback immédiat).
- Réduire l'incertitude — rendre visibles les variables de gameplay importantes sans interrompre l'immersion.

Le HUD comme couche informative
- Traditionnellement, le HUD est une couche fixe (barres, compteurs, mini-carte) qui accompagne le joueur en permanence.
- Il sert aussi de point d'interface pour des actions rapides (touches rapides, boutons contextuels).

Tendance vers le minimalisme et vers l'absence
- De nombreux jeux modernes tendent à minimiser, masquer ou supprimer le HUD visible pour préserver l'immersion.
- Les informations sont de plus en plus intégrées directement dans l'environnement (communication environnementale) :
	- indicateurs visuels sur les objets (lumières, particules, état changeant),
	- retours audio directs (effets sonores de blessure, dialogues),
	- changements de caméra ou d'éclairage pour signaler danger/progression,
	- éléments contextuels qui apparaissent uniquement quand nécessaires (HUD contextuel).

Avantages du minimalisme
- Renforce l'immersion et la lisibilité du monde.
- Encourage l'observation et l'exploration comme mécanismes d'apprentissage.
- Réduit le bruit perceptuel et la surcharge cognitive.

Risques et contreparties
- Perte d'accessibilité si des informations critiques ne sont pas disponibles par d'autres canaux.
- Peut rendre le jeu moins clair pour des joueurs novices.

Bonnes pratiques de conception
- Prioriser : afficher uniquement l'information essentielle en permanence.
- Redondance sensorielle : fournir la même information via deux canaux (visuel + audio) pour accessibilité.
- Contextualité : faire apparaître des éléments HUD uniquement quand le joueur en a besoin (par ex. indicateurs de cible en visée).
- Signifiants clairs : utiliser couleurs, icônes et transitions familières pour réduire l'effort d'interprétation.
- Tests de lecture rapide : vérifier en playtest que les joueurs comprennent et trouvent l'info en < 2 s.

Exemples et références rapides
- Minimalisme visuel : Journey (2012) — interfaces très discrètes favorisant l'exploration.
- Communication environnementale : Portal (2007) — éléments du level design et sons qui guident le joueur.

Conclusion courte
Le HUD reste un outil puissant pour la communication de gameplay. La tendance contemporaine favorise une intégration discrète ou contextuelle de l'information dans l'environnement du jeu, mais cela requiert une attention particulière à l'accessibilité et à la redondance sensorielle.

Si vous voulez, je peux :
- ajouter des exemples GDScript pour un HUD contextuel (afficher/supprimer des éléments selon distance ou état),
- proposer une checklist de tests de lisibilité HUD pour les séances de TP.

## Exemple explicatif (Godot) — HUD cohérent avec la caméra : clé et porte

Objectif : montrer un pattern simple où le joueur collecte une clé, le HUD affiche une icône de clé (anchored au screen via `CanvasLayer`) et la porte n'autorise le passage que si le joueur possède la clé. Le HUD est cohérent avec la caméra (utiliser `CanvasLayer`) et réagit aux événements du joueur.

Structure de scène recommandée (extrait) :

- Main scene (Node2D)
  - World (Node2D) — contient le joueur, la clé (Area2D), la porte (Area2D)
  - CanvasLayer (niveau HUD)
	- HUD (Control)
	  - KeyIcon (TextureRect)  # visible = false par défaut

Notes : `CanvasLayer` place le HUD en screen-space indépendamment de la caméra. Le `HUD` peut écouter des signaux du joueur ou d'un autoload `GameState`.

1) Script : `key.gd` (Area2D) — pickup

```gdscript
extends Area2D

@export var pickup_sound: AudioStream

func _on_body_entered(body: Node) -> void:
	if body.is_in_group("player"):
		# S'assurer que le joueur stocke l'information
		if body.has_method("collect_key"):
			body.collect_key("gold_key")
		# Jouer son et supprimer la clé
		if pickup_sound:
			var s = AudioStreamPlayer2D.new()
			s.stream = pickup_sound
			add_child(s)
			s.play()
		queue_free()

# Connect signal in editor: body_entered -> _on_body_entered
```

2) Script : `player.gd` (CharacterBody2D)

```gdscript
extends CharacterBody2D

signal key_collected(key_name)

var inventory = {}

func collect_key(key_name: String) -> void:
	inventory[key_name] = true
	emit_signal("key_collected", key_name)

func has_key(key_name: String) -> bool:
	return inventory.has(key_name) and inventory[key_name]
```

3) Script : `hud.gd` (Control, child of CanvasLayer)

```gdscript
extends Control

onready var key_icon = $KeyIcon

func _ready():
	key_icon.visible = false
	# Option A : trouver le joueur et connecter le signal
	var player = get_tree().get_root().find_node("Player", true, false)
	if player and player.has_signal("key_collected"):
		player.connect("key_collected", Callable(self, "_on_key_collected"))

func _on_key_collected(key_name: String) -> void:
	# afficher l'icône — adaptation possible selon key_name
	key_icon.visible = true
	# Option : jouer une petite animation ou pulse

func hide_key_icon():
	key_icon.visible = false
```

4) Script : `door.gd` (Area2D / StaticBody2D selon design)

```gdscript
extends Area2D

@export var required_key: String = "gold_key"

func _on_body_entered(body: Node) -> void:
	# Si le joueur a la clé, autoriser le passage (ou changer la scène)
	if body.is_in_group("player") and body.has_method("has_key") and body.has_key(required_key):
		# ouvrir la porte (animation / désactiver collision / changer scène)
		$CollisionShape2D.disabled = true
		$AnimationPlayer.play("open")
	else:
		# feedback : son ou message (le HUD pourrait montrer "il vous manque la clé")
		pass

# Connect body_entered -> _on_body_entered
```

Conseils d'intégration
- Utilisez `CanvasLayer` pour garantir que le HUD reste fixe par rapport à l'écran, même si la caméra bouge.
- Préférez la communication par signaux (`key_collected`) entre le joueur et le HUD plutôt que des checks fréquents en process(). C'est plus performant et plus clair.
- Pour l'accessibilité, fournissez un retour redondant (audio + icône + court message textuel) lorsque la clé est collectée.
- Le HUD peut aussi afficher un message contextuel près de la porte (via une `Label` qui s'affiche quand le joueur est à portée) : utile pour indiquer "Appuyez sur E pour ouvrir" seulement si la clé est possédée.

Souhaitez-vous que j'ajoute les fichiers exemples (scripts complets) dans le dépôt ou que je crée une petite scène d'exemple `scenes/hud_key_demo.tscn` ?

