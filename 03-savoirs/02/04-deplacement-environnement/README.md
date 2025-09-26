# Création d’un environnement virtuel navigable

Un environnement virtuel navigable implique un espace avec des obstacles solides, zones de passage et élements de décor. Il a des proportions adéquates pour que les personnages et objets interactifs puissent le traverser de façon intéressante.

Les objets solides dans un monde de jeu ont des **zones de collision** (ou *colliders*). Ces composants définent la forme et volume que ces objets occupent à l'espace. En Godot, la forme des collisions sont définis par des noeuds `CollisionShape2D` ou `CollisionPolygon2D`, qui sont placés comme fils des nooeuds qui vont définir de comportements spécifiques et qui héritent de `PhysicsBody2D`.

![Exemple de niveau dans l'éditeur Godot et ses zones de collision.](image.png)

## Objets solides

Le `StaticBody2D` crée des objets solides qui ne se déplacent pas et ne sont pas impactés par d'autres objets (ex. un mur). Les `AnimatableBody2D` peuvent être animés et vont impacter d'autres objets, mais ne sont pas impactés par contre (ex. une plateforme mobile). Les `RigidBody2D` sont des corps physique 2D qui sont déplacés par une simulation physique (ex. une boule dynamique). 

Vous trouverez plus d'info sur les [objets de collision à la documentation officielle](https://docs.godotengine.org/fr/4.x/tutorials/physics/physics_introduction.html#collision-objects).

## Aire de détection (Area2D)

Le noeud [`Area2D`](https://docs.godotengine.org/fr/4.x/tutorials/physics/physics_introduction.html#area2d) utilise les zones de collision pour détecter quand un autre objet entre ou sort de son `CollisionShape2D`.  Ils peuvent détecter quand les objets se chevauchent et émettre des signaux quand les corps entrent ou sortent. On peut les utiliser pour déclencher des événements et comportements. 

## Cartes editables (avec **TileMapLayer**)

Les cartes éditables (appellées tilemaps en anglais) nous permettent de créer des espaces avec des tuiles réutilisables. On prends une texture de spritesheet comme base et on peut définir difféerentes proprietes pour chaque tuile, comme des collisions ou connexions entre tuiles. Avec cettes informations, on peut "peindre"
notre environnement avec les tuiles. Ça facilite le processus de création et raffinement d'environnement et donne une présentation plus cohérente et thématique.

Le point de départ pour les tilemaps sur Godot est le noeud `TileMapLayer`. 

![TileMapLayer avec l'éditeur de TileMap activé.](<Capture d’écran 2025-09-18 105034.png>)

Quand un noeud TileMapLayer est selectionné, il montre la grille à la scène. On doit ajouter des ressources `TileSet` pour définir la palette de tuiles et ses propriétes. Après, dans l'éditeur de TileMap à droite, c'est possible de choisir quelles tuiles ou terrains à utiliser pour ajouter à l'environnement.

## Tutoriel vidéo

* Tilemap via Enric https://www.youtube.com/watch?v=B_y0oJMRjSs


## Examples

* [Example d'un tile set](https://raw.githubusercontent.com/egl-edu/exemple--plateforme/refs/heads/main/projet_godot/assets/spritesheets/paysage_tile_set.tres)

* https://tim-montmorency.com/godot-doc/#/godot-demo-projects/2d/dynamic_tilemap_layers/

