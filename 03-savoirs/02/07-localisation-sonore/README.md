# Intégration de médias sonores dans l'environnement virtuel

## Exemple de référence

Dans l'exemple [gllmar.github.io/gd-plateforme-exemple](https://gllmar.github.io/gd-plateforme-exemple), trois types de sons sont intégrés :

* Dépot : https://github.com/gllmar/gd-plateforme-exemple/


### Son d'action du personnage 

Le lecteur audio (`AudioStreamPlayer2D`) est placé dans la scène du personnage. Le son est centré, non localisé, et suit le joueur.

### Son d'un événement (collecte de cœur/powerup)

Le son déclenché lors de l'interaction avec un objet (Detection de colision). Utilise également un `AudioStreamPlayer2D` pour un effet immédiat et centré. 

### Son localisé (ex. feu de camp)

Utilise `AudioStreamPlayer2D` en complément avec un `AudioListener2d` pour créer un effet de proximité et de panoramique. Le volume et la direction changent selon la position du joueur. 

#### Implémentation technique

https://docs.godotengine.org/en/stable/classes/class_audiostreamplayer2d.html

https://docs.godotengine.org/en/stable/classes/class_audiolistener2d.html


##### Côté joueur

Ajouter un nœud `AudioListener2D` sous ton joueur.

Dans l’Inspecteur, cocher `Current = On` (c’est l’oreille).

(Option) Le listener peut rester enfant du joueur : il suivra le joueur automatiquement ses déplacements.

##### Côté feu de camp (la source)

Dans la scène du feu de camp, avoir les éléments suivants 

* `AnimatedSprite2D` (visuel)
* `AudioStreamPlayer2D` (sonore)
    * Stream : ton loop .ogg/.wav (crépitement)
    * Autoplay : On (le son démarre tout seul)
    * Max Distance : ~200 (px) pour la portée
    * Attenuation : 1.5 (chute naturelle)
    * Panning Strength : 1.0 (pan L/R selon position)

Quand le joueur (listener) s’approche, le feu (player) devient audible et pané correctement. Quand il s’éloigne, le volume chute.


## Outils pour générer des sons 

* https://sfxr.me/
* https://socalabs.com/

