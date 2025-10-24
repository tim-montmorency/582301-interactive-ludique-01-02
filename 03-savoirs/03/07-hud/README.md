# Intégration d’une interface graphique HUD («Head-Up Display»)

## Rôle principal du HUD

- Fournir au joueur les informations nécessaires pour prendre des décisions (vie, énergie, munitions, objectifs, feedback immédiat).
- Réduire l'incertitude — rendre visibles les variables de gameplay importantes sans interrompre l'immersion.

## Le HUD comme couche informative

- Traditionnellement, le HUD est une couche fixe (barres, compteurs, mini-carte) qui accompagne le joueur en permanence.
- Il sert aussi de point d'interface pour des actions rapides (touches rapides, boutons contextuels).

## Tendance vers le minimalisme et vers l'absence

- De nombreux jeux modernes tendent à minimiser, masquer ou supprimer le HUD visible pour préserver l'immersion.
- Les informations sont de plus en plus intégrées directement dans l'environnement (communication environnementale) :
	- indicateurs visuels sur les objets (lumières, particules, état changeant),
	- retours audio directs (effets sonores de blessure, dialogues),
	- changements de caméra ou d'éclairage pour signaler danger/progression,
	- éléments contextuels qui apparaissent uniquement quand nécessaires (HUD contextuel).

## Avantages du minimalisme

- Renforce l'immersion et la lisibilité du monde.
- Encourage l'observation et l'exploration comme mécanismes d'apprentissage.
- Réduit le bruit perceptuel et la surcharge cognitive.

## Risques et contreparties

- Perte d'accessibilité si des informations critiques ne sont pas disponibles par d'autres canaux.
- Peut rendre le jeu moins clair pour des joueurs novices.

## Bonnes pratiques de conception

- Prioriser : afficher uniquement l'information essentielle en permanence.
- Redondance sensorielle : fournir la même information via deux canaux (visuel + audio) pour accessibilité.
- Contextualité : faire apparaître des éléments HUD uniquement quand le joueur en a besoin (par ex. indicateurs de cible en visée).
- Signifiants clairs : utiliser couleurs, icônes et transitions familières pour réduire l'effort d'interprétation.
- Tests de lecture rapide : vérifier en playtest que les joueurs comprennent et trouvent l'info en < 2 s.

## Exemples et références rapides

- Minimalisme visuel : Journey (2012) — interfaces très discrètes favorisant l'exploration.
- Communication environnementale : Portal (2007) — éléments du level design et sons qui guident le joueur.

#