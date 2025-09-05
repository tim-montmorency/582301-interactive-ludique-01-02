# Introduction à GDScript (pour Godot 4.4.1)

Ce guide présente l'essentiel de GDScript (version utilisée en Godot 4.4.1) pour des étudiant·e·s qui connaissent déjà JavaScript. L'approche est pédagogique : montrer les similarités et différences utiles pour démarrer rapidement.

## Survol rapide
- Entrée : code GDScript dans des scripts attachés à des scènes Godot.
- Sortie : comportements (mouvements, logique, interactions) exécutés par l'éditeur/compileur Godot.
- Erreurs communes : noms de nœuds incorrects, types non initialisés, oublis de la méthode `_process` vs `_physics_process`.

## Syntaxe et types — points de comparaison avec JavaScript
- GDScript ressemble à Python dans sa syntaxe (indentation significative) mais certains comportements rappellent JS.
- Déclaration de variables :

```gdscript
var x = 5        # type inféré
var y: int = 10  # type explicit
const PI = 3.1416
```

- Types courants : `int`, `float`, `bool`, `String`, `Array`, `Dictionary`, `Node`, `Node2D`, etc.
- GDScript est typé dynamiquement mais autorise des annotations statiques (type hints) pour meilleure lisibilité et vérification.

Comparaison JS :
- Pas de `let/const` vs `var` précis ; `var` est la déclaration principale (use `const` pour constantes).
- Pas de `null` vs `undefined` distinction stricte ; on utilise `null` pour l'absence.

## Fonctions

```gdscript
func greet(name: String) -> String:
    return "Bonjour %s" % name

func _ready():
    print(greet("Monde"))
```

- `-> Type` indique le type de retour (optionnel).
- Les fonctions sont déclarées avec `func` (vs `function`/arrow en JS).

## Classes et scripts
- Un fichier `.gd` attaché à un nœud agit comme une classe. Exemple simple :

```gdscript
extends Node2D

var speed: float = 200.0

func _physics_process(delta: float) -> void:
    position.x += speed * delta
```

- `extends` définit la classe parente (similaire à `class X extends Y` en JS).
- `self` est implicite (`self` existe mais rarement nécessaire).

## Signals (événements)
- Signals = mécanisme d'événements (similaire aux events JS mais intégré au moteur).

```gdscript
signal hit(damage)

func _on_somebody_hits(d):
    emit_signal("hit", d)
```

- On se connecte avec `node.connect("signal_name", target, "method_name")` ou via l'éditeur.

## Scènes et nœuds (architecture Godot)
- Godot n'est pas seulement le langage : la structure scène/nœud est fondamentale.
- Un projet est composé de scènes (.tscn) contenant des nœuds hiérarchiques.
- Pour accéder à un nœud :

```gdscript
var player = $Player    # si le nœud s'appelle "Player"
player.position.x += 10
```

- Utiliser `get_node("path")` pour chemins relatifs/absolus.

## Asynchronie et timers
- Il n'y a pas de `async/await` au sens JS, mais Godot propose :
  - `_process(delta)` et `_physics_process(delta)` pour updates réguliers,
  - `yield()` (deprecated en 4?) / `await` (GDScript 2 introduit `await` sur Signals) — vérifier la version.

Exemple d'attente sur un signal :

```gdscript
await some_timer.timeout
print("Timer done")
```

## Collections et itérations

```gdscript
var a = [1, 2, 3]
for x in a:
    print(x)

var d = {"name": "Alice", "score": 10}
for key in d.keys():
    print(key, d[key])
```

## Gestion des ressources et chemins
- `load("res://path/to/resource")` charge une ressource au runtime.
- `preload("res://...")` charge à la compilation (utile pour performances).

## Bonnes pratiques pédagogiques
- Utiliser GDScript pour tous les TP initiaux (simplicité + intégration).
- Commenter brièvement les scripts, garder les fonctions courtes.
- Préférer les signals pour la communication entre nœuds plutôt que liaisons directes fortes.
- Annoter les types quand utile pour l'autocomplétion et la documentation.

## Exemples rapides
- Déplacement basique (Node2D) :

```gdscript
extends Node2D

var speed := 300

func _process(delta: float) -> void:
    var dir := Vector2.ZERO
    if Input.is_action_pressed("ui_right"):
        dir.x += 1
    if Input.is_action_pressed("ui_left"):
        dir.x -= 1
    position += dir.normalized() * speed * delta
```

- Collision simple via `area_entered` signal :

```gdscript
func _on_Area2D_body_entered(body):
    if body.is_in_group("enemies"):
        take_damage(10)
```

## Ressources
- Documentation GDScript : https://docs.godotengine.org
- Tutoriels officiels et exemples fournis avec Godot.
- https://gdscript.com/tutorials/


