# Notions d’interaction : interacteur et affordance 

## Qu'est-ce que l'interactivité ?

Capacité d'un système à répondre aux actions d'un joueur de façon claire et utile.

### Qualité de l'interactivité (critères rapides)

- Réactivité (latence) — délai entrée → retour (idéal < 100 ms).
- Précision — mêmes actions donnent les mêmes résultats.
- Lisibilité du feedback — signaux visuels/sonores clairs.
- Robustesse — comportement correct aux cas limites.
- Découvrabilité (affordance) — l'utilisateur devine l'action possible.
- Satisfaction — ressenti de contrôle.

## Affordance (Gibson) — essentiel

Affordance = ce que l'environnement offre à un acteur (possibilités d'action).
En jeu : rendre ces possibilités visibles (couleur, animation, son) pour éviter les explications textuelles longues.

### Mise en pratique (actions simples)

1. Marquer visuellement les éléments interactifs.
2. Donner un retour immédiat (son, particules, animation) à chaque action.
3. Tester : cold-start (novice), répétitions (consistance), edge-cases.
4. Documenter : courts clips ou captures montrant tests et anomalies.

### Tester l'affordance

- Intuitif : novice réussit l'action en <30 s ? (oui/non)
- Consistant : Répéter une action résulte le même comportement (oui/non)
- Latence adéquate <100 ms ? (oui/non)
- Échecs aux cas limites <10 % ? (oui/non)
- Feedback multimodal présent ? (oui/non)
- Objets réutilisables ? (oui/non)

---

Conserver l'essentiel permet de tester et d'itérer rapidement un prototype.
 
