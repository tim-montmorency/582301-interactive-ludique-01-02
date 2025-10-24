# Notions d’interaction : interacteur, affordance, engagement et agentivité

## 1. Concepts

* **Affordance** *(James J. Gibson, 1979)*
  → Les **possibilités d’action offertes par un objet à un agent**, selon les capacités de celui-ci.

  * *Affordance réelle* : ce que l’objet permet effectivement de faire.
  * *Affordance perçue* : ce que l’utilisateur croit pouvoir faire (cruciale pour le design d’interfaces).
    📚 [Wikipedia – Affordance](https://fr.wikipedia.org/wiki/Affordance)

* **Agentivité (agency)**
  → Sentiment du joueur d’avoir un **pouvoir d’action significatif** et d’influence sur le monde du jeu.
  Elle repose sur la cohérence des règles, le feedback, et la perception de conséquences tangibles.

* ⭐ **Engagement**
  → Degré d’attention, d’émotion et d’investissement du joueur. Il dépend du rythme, du feedback sensoriel, de la narration et de la progression.
  (Référence utile : *Flow theory*, Mihály Csíkszentmihályi, 1990)

---

## 2. Éléments qui rendent un jeu intuitif et affordant 

Chaque point inclut un **indicateur mesurable** et un **exemple vidéoludique**.
Ces critères peuvent servir à **évaluer un prototype** lors de playtests.

| Élément                              | Indicateur                                                | Techniques de design                                            | Exemple                                                                              |
| ------------------------------------ | --------------------------------------------------------- | --------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| **Découvrabilité**                   | % de joueurs qui interagissent correctement sans tutoriel | Silhouettes distinctes, animations d’idle, icônes contextuelles | *Super Mario Bros* (1985) – les blocs « ? » attirent le regard et incitent à sauter. |
| **Signifiants visuels** | Temps moyen avant première interaction                    | Éclairage, couleur, contraste, micro-animations                 | *Portal* (2007) – surfaces brillantes indiquant où créer des portails.               |
| **Mapping et contraintes**           | Erreurs d’interaction / minute                            | Correspondance naturelle entre action et effet, limites claires | *Angry Birds* (2009) – flèches et pentes guident le tir intuitivement.               |
| **Feedback immédiat et différencié** | Latence entre action et retour (ms)                       | Son, particules, vibration, changement d’UI                     | *Zelda* (1986) – son distinctif + ajout d’objet visible dans l’inventaire.           |
| **Consistance systémique**           | Taux de réussite sur actions similaires                   | Règles visuelles cohérentes, réutilisation des motifs           | *Hollow Knight* (2017) – logique uniforme dans les mécaniques et environnements.     |
| **Progression (scaffolding)**        | Temps pour maîtriser une mécanique                        | Tutoriels implicites, difficulté graduée, power-ups             | *Metroid* (1986) – chaque objet débloque une nouvelle zone d’exploration.            |
| **Choix significatifs**              | Diversité d’états accessibles après décision              | Conséquences visibles et persistantes, bifurcations             | *Gone Home* (2013), *Edith Finch* (2017) – narration par exploration et découverte.  |
| **Engagement émotionnel**            | Taux de rétention, auto-évaluation                        | Design sonore, rythme visuel, mise en scène                     | *Journey* (2012) – émotion par épure visuelle et sonore.                             |

⭐ *Conseil :* Lors des séance de test, mesurez trois critères simples :
**(1)** taux de découverte spontanée, **(2)** latence du feedback, **(3)** retour volontaire à la mécanique après 10 min.

---

## 3. Comment le joueur reconnaît qu’il peut interagir (modes de détection)

L’interacteur perçoit sa **capacité d’action** à travers des signaux contextuels.
Ces modes peuvent aussi se traduire directement en termes **techniques (moteur de jeu)** :

| Type de détection          | Usage courant                                       | Exemple technique / concept               |
| -------------------------- | --------------------------------------------------- | ----------------------------------------- |
| **Proximale**              | Power-ups, triggers, zones d’activation             | `Area2D` ou `CollisionShape2D` dans Godot |
| **Directionnelle**         | Objets activables seulement dans le champ de vision | *Stealth games* avec cônes de vision      |
| **Contextuelle**           | Dépend d’un état du joueur (ex. clé possédée)       | Portes, objets de quête                   |
| **Événementielle (input)** | Geste, appui long, combinaison                      | Appui prolongé pour charger une attaque   |
| **Sociale / collective**   | Interaction multi-joueurs synchronisée              | Leviers coopératifs, puzzles à deux       |

⭐ *Astuce de design :* toujours accompagner la zone d’interaction d’un **feedback anticipatoire** (halo, son, vibration légère).

---

---



## 4. Recommandations pratiques pour intégrateurs multimédias

* Fournir **au moins un signal visuel ou sonore** avant toute action attendue.
* Documenter les **règles d’affordance** dans le *level design document*.
* Prototyper les mécaniques avec des **formes simples** avant d’ajouter les textures ou animations.
* **Tester tôt, tester souvent** : même un prototype gris peut révéler beaucoup sur la clarté d’une interaction.
* Mesurer trois choses : **découvrabilité**, **latence**, **rétention**.

⭐ *Rappel conceptuel final :*

> **Affordance → Signifiant → Action → Feedback → Agentivité → Engagement**

Ce cycle forme le cœur de toute expérience interactive réussie.

---

### Sources et lectures recommandées

* **Gibson, J. J.** — *The Ecological Approach to Visual Perception* (1979)
* **Norman, D. A.** — *The Design of Everyday Things* (1988)
* **Csíkszentmihályi, M.** — *Flow: The Psychology of Optimal Experience* (1990)
* [Wikipedia – Affordance](https://fr.wikipedia.org/wiki/Affordance)



