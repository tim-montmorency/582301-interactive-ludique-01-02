# Progression en fonction de la réussite d’une action

Cette section présente plusieurs méthodes courantes pour verrouiller la progression d'un joueur derrière une action ou un objectif réussi : vaincre un boss, éliminer un certain nombre d'ennemis, collecter tous les objets ramassables, résoudre un puzzle, etc. 

## Principes communs

- Signaler clairement la condition requise (discoverability).
- Donner un feedback immédiat et redondant (visuel + audio) lors de l'accomplissement.
- Prévoir de l'accessibilité (indices alternatifs, messages textuels).

## Vaincre un boss
Description : la porte / la sortie ne s'ouvre que quand le boss est vaincu.

Pattern : le boss émet un signal `defeated` quand sa vie atteint zéro ; un gestionnaire (Main, GameState ou la porte elle-même) écoute ce signal et déverrouille la progression.

Exemple (boss.gd) :

```gdscript
extends CharacterBody2D

signal defeated

var health = 100

func apply_damage(dmg:int) -> void:
    health -= dmg
    if health <= 0:
        emit_signal("defeated")
        queue_free()
```

Gestionnaire (par ex. porte qui écoute) :

```gdscript
func _ready():
    var boss = get_node("../Boss")
    if boss and boss.has_signal("defeated"):
        boss.connect("defeated", Callable(self, "_on_boss_defeated"))

func _on_boss_defeated():
    $CollisionShape2D.disabled = true
    $AnimationPlayer.play("open")
```

Conseils : ajouter un mini-cinematic/son pour célébrer la défaite et prévenir le joueur que la progression est débloquée.

## Compteur d'ennemis éliminés (kill count)
Description : débloquer la sortie après avoir tué N créatures.

Pattern : chaque ennemi émet un signal `enemy_killed` ou appelle un gestionnaire central pour incrémenter un compteur ; la porte vérifie le compteur.

Exemple (GameState ou Main) :

```gdscript
var enemies_killed := 0
var enemies_needed := 10

func on_enemy_killed():
    enemies_killed += 1
    if enemies_killed >= enemies_needed:
        unlock_gate()

func unlock_gate():
    var gate = get_node("/root/World/Gate")
    gate.open()
```

Dans l'ennemi (on death) :

```gdscript
get_node("/root/Main").on_enemy_killed()
```

Conseils : afficher la progression ("5 / 10") dans le HUD pour maintenir la motivation.

## Collecter tous les objets ramassables (collect-all)
Description : la progression est conditionnée à la collecte de tous les items d'un ensemble (ex : clefs, fragments).

Pattern : le niveau connaît `total_items` ; chaque pickup incrémente `collected_items`. Quand égalité, déverrouiller.

Exemple :

```gdscript
var total_items := 5
var collected_items := 0

func on_item_collected():
    collected_items += 1
    if collected_items >= total_items:
        $Gate.open()
```

Pickup (item.gd) :

```gdscript
func _on_body_entered(body):
    if body.is_in_group("player"):
        get_node("/root/Main").on_item_collected()
        queue_free()
```

Conseils : fournir un suivi (mini-map, HUD counter) et prévoir des mécanismes de récupération si l'objet est perdu.

## Résolution d'un puzzle ou activation d'un mécanisme
Description : des éléments du niveau doivent être manipulés dans un certain ordre ou état pour débloquer la progression.

Pattern : chaque élément du puzzle émet un signal lors du changement d'état ; un vérificateur central valide la configuration et déverrouille.

Exemple (vérification simple) :

```gdscript
func check_puzzle(states:Dictionary) -> bool:
    # states : {"lever1":true, "lever2":false, ...}
    return states["lever1"] and not states["lever2"] and states["lever3"]

func on_puzzle_changed(states:Dictionary):
    if check_puzzle(states):
        open_secret()
```

## Accumulation de ressources / score
Description : la progression peut dépendre d'un score (argent, points d'expérience) atteint.

Pattern : mettre à jour un compteur global et vérifier le seuil.

## Autres patterns
- Temps (survivre X secondes)
- Reputation / alliances (avoir aidé un NPC)
- Exploration (découvrir tous les lieux)
- Coopération (tous les joueurs doivent activer un interrupteur)

Questions de design et pièges à éviter
- Discoverability : le joueur doit comprendre l'objectif ou avoir des indices clairs.
- Feedback : indiquez clairement quand une condition est remplie.
- Frustration : évitez les «collectathons» excessifs sans points de repère.
- Sauvegarde : pour les objectifs longue durée, sauvegardez l'état intermédiaire.
- Raccourcis : prévoir des mécaniques pour éviter de coincer un joueur (retour en arrière, items réapparaissant si nécessaire).

Tests et checklist pour TP
- Vérifier que la condition n'est pas triviale à activer par accident.
- Vérifier feedback (son + visuel) quand l'objectif est atteint.
- Tester sauvegarde/reprise après redémarrage.
- S'assurer qu'il n'y a pas de blocage (deadlock) si un objet nécessaire n'est plus accessible.

Exemples d'utilisation pédagogique
- Niveau tutoriel : kill count simple pour apprendre le combat.
- Niveau puzzle : progression par résolution pour enseigner observation et logique.
- Niveau exploration : collect-all pour encourager l'exploration.

Voir aussi : `03-savoirs/03/03-clef-porte/README.md` pour un exemple clé → porte et `03-savoirs/03/07-hud/README.md` pour l'affichage d'un compteur dans le HUD.

---

Si vous voulez, je peux générer des scripts exemples et une petite scène de démonstration pour l'un des patterns ci‑dessus (quel choix : boss, kill-count, collect-all ?) .
# Progression en fonction de la réussite d’une action

