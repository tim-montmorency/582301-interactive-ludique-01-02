# Intégration d’une interface graphique HUD («Head-Up Display»)

L'interface HUD est une abréviation originaire des équipments aéronautiques (*heads-up display*). L’idée de base c’est que ces éléments d’interface sont visibles à la majorité du temps, parce qu’ils communiquent des informations relevantes pour le déroulement du système à tous temps. Dans les jeux vidéo, les éléments de HUD sont souvent placés aux coins de l’écran et utilisent différentes conventions et techniques d’interface pour présenter des informations.

## Rôle du HUD et tendance vers le minimalisme

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

## Assemblage avec Godot

Pour afficher une interface relative à l’écran (indépendamment de la position de la **Camera2D**), on utilise un nœud **CanvasLayer** avec des nœuds d’interface (hérités de **Control**) dedans. On peut utiliser des conteneurs, comme les **HBoxContainer**, pour les organiser horizontalement.


La connexion entre les nœuds d’interface et les variables du jeu (niveau courant, nombre de monnaies, nombre de vies, etc.) peut être réalisée avec des **signaux**. Voici un exemple pour le nombre de monnaies.

```gdscript
# Dans main.gd, le script central du jeu

@export var monnaies = 0
signal monnaie_collectee

func augmenter_monnaies():
    monnaies += 1
    monnaie_collectee.emit(monnaies)
```

![Arborescene de la scène Monnaie](image.png)

```gdscript
# monnaie.gd
extends Area2D

func _ready() -> void:
    body_entered.connect(monnaie_touchee)

# Fonction quand un corps touche la monnaie
func monnaie_touchee(body):
    if body is Joueur:
        Main.augmenter_monnaies()
        call_deferred("queue_free")
```

![Arborescence de la scène HUD](image.png)

```gdscript
# Dans hud.gd
extends Control

# Connecte le signal pour activer la rétroaction et
# actualiser le HUD
func _ready() -> void:
    %MonnaiesLabel.text = str(Main.monnaies)
    Main.monnaie_collectee.connect(retroaction_monnaie)	

func retroaction_monnaie(valeur_monnaies):
    %MonnaiesLabel.text = str(valeur_monnaies)
    %MonnaiesSFX.play()
```
