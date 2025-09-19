# Grille d'évaluation — TP2 : Personnage et environnement


## Mode d'emploi
Chaque critère est noté de 1 à 4 :

- 4 — Excellent
- 3 — Satisfaisant
- 2 — Partiellement
- 1 — Insuffisant

Total maximum : 40 points (10 critères × 4). La note finale pour le cours
sera calculée à partir de ce total et convertie sur 20% (voir section Calcul).

## Livrables obligatoires
- Scene composé d'un personnage interactif, d'animation de déplacement (mouvement, saut),  objets interactif, menu.
- Export Web fonctionnel (lien déployé — ex. GitHub Pages). 
- Dépôt Git en ligne (URL) contenant le code source et l'historique. 
- Documentation de l'arborescence via `README.md` , intention et instructions d'opération,



## Grille (10 critères — notation 1–4)

| Critère | 4 — Excellent | 3 — Satisfaisant | 2 — Partiellement | 1 — Insuffisant |
|---|---|---:|---:|---:|
| Notions d’interaction : interacteur, affordance | Affordances claires et cohérentes ; interactions significatives et documentées. | Affordances visibles ; interactions fonctionnelles mais peu documentées. | Affordances partielles, interactions limitées ou ambiguës. | Aucune affordance identifiable ou interactions non fonctionnelles. |
| Détection de l’état du personnage (collision, au sol, en l'air) | Etats détectés de façon fiable ; transitions stables et testées. | Détection correcte dans la majorité des cas ; quelques limites. | Détection imparfaite ; transitions parfois incohérentes. | Etats non détectés ou erratiques. |
| Indication visuelle et animation de l’état du personnage | Animations/feedback clairs pour chaque état (idle/run/jump/hurt) ; bonne qualité visuelle. | Indications présentes et généralement lisibles. | Indications limitées ou peu distinctes entre états. | Pas d'indication visuelle des états. |
| Création d’un environnement virtuel navigable | Niveau cohérent, repères visuels, parcours testable et jouable. | Environnement jouable mais simplifié ou peu soigné. | Navigation possible mais confuse ; éléments manquants. | Environnement non navigable ou incohérent. |
| Déplacement dans l’environnement virtuel | Contrôles réactifs et fluides ; mécanique (saut, inertia) stable. | Déplacement fonctionnel ; quelques ajustements à prévoir. | Déplacement saccadé ou imprécis ; ressenti à améliorer. | Déplacement non fonctionnel ou impraticable. |
| Configuration de la caméra virtuelle 2D | Caméra suit correctement, cadre bien géré, pas de sauts brusques. | Caméra globalement correcte, gênes occasionnelles. | Caméra parfois mal cadrée ou gênante. | Caméra non configurée ou problématique. |
| Détection de collisions pour déclenchement d'événements | Collisions fiables et utilisées pour déclencher événements attendus. | Evénements déclenchés correctement dans la plupart des cas. | Déclenchements inconsistants ; certains événements manquent. | Collisions non fiables ou événements non déclenchés. |
| Intégration de médias sonores | Sons pertinents, bien synchronisés ; réglages audio gérés (volume, loop). | Sons présents et utiles ; quelques problèmes mineurs. | Sons limités, mal synchronisés ou manquants. | Pas de son ou son inapproprié. |
| Fonctionnement d’une interface virtuelle (menu) | Menu complet (start/pause/instructions) ergonomique et fonctionnel. | Menu présent et fonctionnel mais minimal. | Menu basique ou navigation peu claire. | Pas d'interface/menu fonctionnel. |
| Objet interactif qui change comportement du niveau/personnage | Objet pertinent, effet observable, documenté et testé. | Objet fonctionne et produit un effet visible. | Objet présent mais effet limité ou mal documenté. | Objet absent ou non fonctionnel. |

