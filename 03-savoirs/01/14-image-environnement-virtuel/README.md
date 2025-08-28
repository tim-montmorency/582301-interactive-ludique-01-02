# Intégration d’images dans l’environnement virtuel


## Principes généraux
- Choisir le bon nœud selon l’usage : rendu 2D, interface, tuiles, textures 3D, ou rendu dynamique.
- Optimiser pour la cible (WebGL/WASM → poids réduit, atlas, formats compressés quand possible).
- Configurer l’import (compression, filtre, repeat, mipmaps) dans l’onglet Import pour chaque image.

## Nœuds images
- Sprite2D — afficher une texture 2D simple (images isolées)  
  https://docs.godotengine.org/en/stable/classes/class_sprite2d.html
- AnimatedSprite2D / SpriteFrames — animations image-par-image (spritesheets ou frames)  
  https://docs.godotengine.org/en/stable/classes/class_animatedsprite2d.html
- TextureRect — intégrer une image dans l’interface (Control) avec options d’échelle et de stretch  
  https://docs.godotengine.org/en/stable/classes/class_texturerect.html
- TextureButton — bouton graphique basé sur textures (UI interactive)  
  https://docs.godotengine.org/en/stable/classes/class_texturebutton.html
- NinePatchRect — images UI redimensionnables (bordures conservées)  
  https://docs.godotengine.org/en/stable/classes/class_ninepatchrect.html
- TileMap / TileSet — création de niveaux 2D à partir de tiles / atlas  
  https://docs.godotengine.org/en/stable/classes/class_tilemap.html | https://docs.godotengine.org/en/stable/classes/class_tileset.html
- ViewportTexture / Viewport — rendre une scène dans une texture (render-to-texture, mini-caméras, effets)  
  https://docs.godotengine.org/en/stable/classes/class_viewport.html | https://docs.godotengine.org/en/stable/classes/class_viewporttexture.html
- MeshInstance3D + StandardMaterial3D — appliquer des textures en 3D (albedo, normal, roughness...)  
  https://docs.godotengine.org/en/stable/classes/class_meshinstance3d.html | https://docs.godotengine.org/en/stable/classes/class_standardmaterial3d.html
- Importer d’images & workflow d’import — réglages disponibles lors de l’import (format, compression, mipmaps)  
  https://docs.godotengine.org/en/stable/tutorials/assets/importing_assets.html

## Exemples rapides (GDScript)
- Charger et assigner une texture à un Sprite2D
```gdscript
# attaché au Sprite2D
func _ready():
    texture = load("res://assets/sprite.png")
    self.texture = texture
```

## Jouer une animation via AnimatedSprite2D (frames préparés en editor)
```gdscript
$AnimatedSprite2D.play("run")
```

## Bonnes pratiques et optimisation
- Atlas / spritesheets : regrouper petites images dans un atlas réduit overhead (surtout Web).
- Formats : PNG pour transparence, JPEG pour photos (sans alpha). Pour le web, réduire la résolution et compresser avant import.
- Taille : privilégier puissances de deux si utilisé avec certaines compressions/mipmaps pour 3D.
- Mipmaps : utiles en 3D et pour textures réduites; à désactiver pour pixel art 2D.
- Pixel art : désactiver filtrage (filter off) et activer pixel snap si nécessaire.
- Assets lourds : compresser, ou stocker hors dépôt (Git LFS pour projets avancés).
- Tester sur la cible (navigateur WebGL/WASM) : rendu et performances diffèrent du desktop.

## Workflow recommandé 
1. Importer l’image dans res://assets/, régler l’import (Importer → Reimport si besoin).  
2. Utiliser Sprite2D / TextureRect pour prototypage rapide.  
3. Passer à TileSet, AnimatedSprite2D ou ViewportTexture selon le besoin.  
4. Mesurer la taille et la latence pour l’export HTML5 ; optimiser avant déploiement.

## Ressources complémentaires

- Godot docs — Textures & Sprites (tutoriels) : https://docs.godotengine.org/en/stable/tutorials/2d/textures_and_sprites.html  
- Importing assets : https://docs.godotengine.org/en/stable/tutorials/assets/importing_assets.html  
- 3D materials & textures : https://docs.godotengine.org/en/stable/tutorials/3d/3d_textures_and_materials.html