# git command

Ce document présente une arborescence synthétique des commandes Git de base et leur fonction.

## Arborescence des commandes Git (basique)

- Configuration
  - `git config --global user.name "Nom"` — définir le nom d'utilisateur pour les commits
  - `git config --global user.email "email@example.com"` — définir l'email pour les commits

- Création / récupération
  - `git init` — initialiser un dépôt Git local
  - `git clone <url>` — cloner un dépôt distant en local

- Travailler l'index (staging)
  - `git add <fichier>` — ajouter un fichier à l'index (préparer pour commit)
  - `git add .` — ajouter toutes les modifications suivies au staging
  - `git reset <fichier>` — retirer un fichier de l'index (sans toucher au fichier)

- Validation (commits)
  - `git commit -m "message"` — créer un commit à partir de l'index
  - `git commit --amend` — modifier le dernier commit (message ou contenu)

- État et historique
  - `git status` — afficher l'état du dépôt (modifs, staging, branche)
  - `git log` — voir l'historique des commits
  - `git show <commit>` — afficher les détails d'un commit
  - `git diff` — montrer les différences non indexées

- Branches
  - `git branch` — lister les branches locales
  - `git branch <nom>` — créer une nouvelle branche
  - `git checkout <branche>` — se positionner sur une branche (ancienne forme)
  - `git switch <branche>` — se positionner sur ou créer/aller sur une branche (recommandé)
  - `git merge <branche>` — fusionner une branche dans la branche courante
  - `git rebase <branche>` — réappliquer des commits sur une autre base (avancé)

- Travail avec des dépôts distants
  - `git remote add origin <url>` — lier un dépôt distant nommé "origin"
  - `git fetch` — récupérer les références distantes sans fusionner
  - `git pull` — récupérer puis fusionner (fetch + merge) depuis le distant
  - `git push` — envoyer les commits locaux vers le dépôt distant

- Sauvegarde temporaire
  - `git stash` — sauvegarder temporairement des modifications non committées
  - `git stash pop` — réappliquer et supprimer la dernière stash

- Annuler / Réécrire
  - `git reset --soft <commit>` — reculer HEAD sans toucher à l'index et au working tree
  - `git reset --mixed <commit>` — (défaut) reculer HEAD et réinitialiser l'index
  - `git reset --hard <commit>` — reculer HEAD et réinitialiser index + working tree (détruit les changements)
  - `git revert <commit>` — créer un commit qui annule un commit précédent (sécurisé pour branches partagées)

- Tags
  - `git tag <v1.0>` — créer une étiquette nommée pour marquer un commit
  - `git push --tags` — envoyer les tags vers le distant

- Utilitaires courants
  - `git rm <fichier>` — supprimer un fichier du dépôt et de l'index
  - `git mv <src> <dst>` — renommer/déplacer un fichier suivi par Git
  - `git blame <fichier>` — montrer quelle révision/quel auteur a modifié chaque ligne
  - `git stash list` — lister les stashs disponibles

## Notes rapides
- Pour voir une aide rapide : `git help <commande>` ou `git <commande> --help`.
- Préférer `git switch` et `git restore` pour certaines opérations sur les branches et fichiers (nouvelle ergonomie).
- Attention aux `reset --hard` et aux opérations qui réécrivent l'historique sur des branches partagées (risque de perte de données).

