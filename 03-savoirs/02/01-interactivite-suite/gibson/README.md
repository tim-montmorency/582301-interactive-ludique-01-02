# Gibson — L'affordance appliquée au jeu vidéo

https://cs.brown.edu/courses/cs137/2017/readings/Gibson-AFF.pdf


Cette page propose une lecture ciblée du chapitre 8 — La théorie des
affordances (The Theory of Affordances) de James J. Gibson, traduite en
recommandations pratiques pour la conception de jeux vidéo et pour
l'évaluation de prototypes.

## Résumé rapide

- L'affordance est une relation entre l'environnement et l'animal : ce que l'environnement "offre" à l'acteur. Ce n'est ni purement physique ni purement mental, mais les deux en même temps.

- Les affordances sont relatives à l'organisme : un objet offre différentes possibilités à un enfant et à un adulte selon leur taille et capacités.

- Gibson avance que l'information contenue dans la stimulation ambiante (lumière,texture optique, sons) présente des invariants — des propriétés stables — qui permettent de percevoir directement ce que l'environnement offre (les affordances), sans avoir besoin d'inférences conscientes longues.

- Risque d'information trompeuse : quand les indices ambiants sont ambigus ou contradictoires la perception peut être erronée et conduire à des actions inadaptées.

## Ce que cela implique pour la conception de jeux (game design)

1. Concevoir pour le corps-jeu

Définir les affordances en fonction des dimensions et mécaniques du personnage (hauteur de saut, vitesse,portée de main). 

2. Rendre l'affordance visible (découvrabilité — discoverability) 

Utiliser couleurs, animations, micro-interactions et sons pour indiquer comment agir. Moins de texte = meilleure prise en main.

3. Multimodalité 

Combiner indices visuels et sonores (voire retours haptiques) pour renforcer la spécification d'une affordance et diminuer les erreurs.

4. Tester la robustesse 

Les affordances doivent tenir dans les cas limites (edge-cases) (collisions rapides, sauts en bord, changements d'état). On doit tester systématiquement ces scénarios.

5. Éviter l'information trompeuse (misinformation) : 

Ajouter des indices supplémentaires pour corriger les illusions (ex. reflets / joints pour rendre le verre visible, sons pour distinguer eau/sol, particules pour indiquer surface instable).

6. Encourager la polyvalence des objets  

Enrichit l'exploration et la créativité du joueur.

7. Penser l'expérience, 

Créer des espaces avec des affordances résonantes

## Exemples concrets

### Jeu de plateformes (platformer) 

* Plateformes sautables 
    * contraste de couleur 
    * impression de surface

si la plateforme est fragile, ajouter des particules de poussière et un son d'effritement.

* Objet ramassable (collectable) 
    * une lueur, 
    * un son aigu à la collecte 

- Porte/levier 
    * animation de pré-activation (clignotement) + son de métal
    * pour indiquer activabilité ; retour (feedback) immédiat à l'activation.


## Tester

### Découvrabilité

(discoverability test / démarrage à froid — cold-start)

Procédure : présenter la scène à un joueur n'ayant reçu aucune instruction.
Demander à l'observateur d'essayer d'accomplir l'objectif principal. Mesurer :

- Temps pour accomplir l'action-clé (seuil recommandé : < 30 s).
- Nombre d'indices nécessaires (aide demandée, indications visuelles/sonores).

Critères :
- Succès clair : l'action est réalisée en < 30 s sans aide.
- Partiel : réalisation avec aide ou > 30 s.
- Échec : l'action n'est pas réalisée.


### Consistance (consistency test)

Procédure : répéter la même action 10 fois dans différents contextes (positions,
vitesses, états). Noter les échecs et comportements aberrants.

Critère de réussite : tolérance d'échec < 10 % pour un niveau "satisfaisant".

Preuve : log des essais (10 entrées) ou vidéo regroupée avec timestamps.

### Calculer la latence (latency check)

Procédure : mesurer le délai entre l'entrée du joueur (touche / clic) et le
retour visible ou sonore fourni par le jeu. Répéter plusieurs fois et prendre
la moyenne.

Seuils recommandés :
- Excellent : < 100 ms
- Satisfaisant : < 200 ms
- À améliorer : > 200 ms

Preuve : capture d'écran timestampée ou log indiquant les temps mesurés.

### Tester les limites (edge-case stress)

Procédure : forcer les cas limites pertinents pour votre prototype :

- Collisions rapides (pusher / enchaînements d'obstacles)
- Sauts en bord de plateforme
- Transitions d'état rapides (par ex. passage de l'état au sol à l'état en l'air)
- Scénarios de surcharge d'entrées (appuis répétés, combinaisons simultanées)

Critère : aucun comportement critique (plantage, perte d'état, téléportation
imprévue). Documenter les anomalies avec timestamps.

Preuve : courtes vidéos ou logs montrant l'anomalie et la séquence reproductible.



