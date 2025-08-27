
# Git — introduction rapide

## Concepts clés (en bref)
- Dépôt (repo) : l'espace qui contient votre code et l'historique.
- Commit : un snapshot versionné (message + auteur + changements).
- Branche (branch) : variante de développement parallèle (ex. `main`, `dev`, `feature-x`).
- Remote : dépôt distant (GitHub) pour sauvegarde et collaboration.

## Préparation rapide
- Installer Git et Git LFS.


	macOS (exemples) :

	```bash
	brew install git
	brew install git-lfs
	git lfs install
	```

	Windows :

	- Méthode GUI (recommandée pour débutant·e·s) :
		- Git : https://git-scm.com/download/win (installez Git Bash)
		- Git LFS : https://git-lfs.github.com/ (exécutez l'installateur)
		- Après installation, ouvrez Git Bash ou PowerShell et lancez :

			```bash
			git lfs install
			```

		- Option gestionnaire de paquets (si configuré) :


			winget (Windows 10/11) :

			```powershell
			winget install --id Git.Git -e --source winget
			winget install --id Git.GitLFS -e --source winget
			git lfs install
			```


##  Configurer votre identité :

```bash
git config --global user.name "Prénom Nom"
git config --global user.email "etu@exemple.edu"
```

## Flux de travail recommandé (VS Code + GitHub)
1. Créer un dépôt GitHub (ou cloner un dépôt existant).
2. Ouvrir le dossier du projet dans VS Code (extension Git intégrée).
3. Travailler, puis utiliser l'onglet Source Control pour stage/commit/push, ou la ligne de commande :

	```bash
	git clone git@github.com:utilisateur/mon-projet.git
	cd mon-projet
	git checkout -b ma-branche
	# modifier des fichiers
	git add .
	git commit -m "Message clair et bref"
	git push -u origin ma-branche
	```

4. Créer une Pull Request sur GitHub pour fusionner les changements vers `main`.

Conseils VS Code : utilisez l'icône Source Control pour visualiser diffs, et l'extension GitHub Pull Requests pour gérer PRs sans quitter l'éditeur.

## Gérer les médias — Git LFS (usage recommandé)
- Git LFS est pratique pour les projets contenant de nombreux fichiers volumineux.
- Pour les débutant·e·s qui ont des assets légers (quelques images/sons), LFS n'est pas nécessaire : ajouter les fichiers au dépôt suffit.

Avantages de Git LFS pour les grands projets :
- Réduit la taille du repository Git (seuls des pointeurs sont versionnés),
- Accélère les clones/pulls pour les contributeur·rice·s,
- Gère proprement les binaires (images, audio, .pck, .wasm) sans polluer l'historique Git,
- Intégration avec GitHub (stockage LFS géré côté serveur).

Exemple d'activation et d'usage :

```bash
git lfs track "*.png"
git lfs track "*.wav"
git add .gitattributes
git add <fichiers-lourds>
git commit -m "Add large media via LFS"
git push
```


## Fichiers utiles
- `.gitignore` : ignorer les dossiers temporaires et les imports Godot (`.import/`, `export_presets.cfg`, `.godot/` selon version).
- `.gitattributes` : contient les règles Git LFS.

Exemple minimal `.gitignore` pour Godot :

```text
# Godot
.import/
imports/
*.import
/.godot/
/.export/
/.cache/

# OS
.DS_Store
```

## Bonnes pratiques (rapide)
- Commits petits et fréquents, messages explicites.
- Branches par fonctionnalité / TP.
- Garder le dépôt principal (`main`) stable ; fusion via PR.
- Pousser les exports Web dans `docs/` ou `gh-pages` uniquement (éviter de versionner des builds volumineux inutiles dans `main` si possible).

## Ressources
- Documentation Git : https://git-scm.com/docs
- GitHub Pages : https://docs.github.com/pages
- Git LFS : https://git-lfs.github.com/
