# Configuration de la caméra virtuelle 2D

Une caméra dans un moteur de jeu est une fenêtre pour montrer au joueur l'état du monde du jeu. Elle peut avoir plusieurs styles, comme, par exemple, les caméras sidescroller, top-down, ou en première personne (en 3D). L’objet « caméra » définit quelle partie du monde du jeu est visible par le joueur, et on peut ajouter plusieurs comportements à cet objet pour avoir des fonctionnalités, comme des caméras fixes ou auto-scrolling ou contrôlables.

Dans Godot et en 2D, on utilise le nœud `Camera2D` pour représenter notre caméra. Elle a plusieurs comportements de base déjà fournis, comme des limites horizontales et verticales, du déplacement et de la rotation adoucis, contrôles par glissage, et un système de zoom. 

Pour que la caméra suive un objet, on peut placer le nœud `Camera2D` comme fils de cet objet. Remarque l'arborescence de scène dans l'exemple suivant.

![Exemple arborescence de caméra.](<Recording 2025-09-22 152355.gif>)

Essayez de modifier le comportement de la caméra (limites, zoom) dans [l'exemple de jeu de plateforme](https://egl-edu.github.io/exemple--plateforme/) (téléchargez et modifiez les propriétés du nœud).

## Ressources supplémentaires

* [Documentation officielle sur les caméras 2D](https://docs.godotengine.org/fr/4.x/classes/class_camera2d.html).