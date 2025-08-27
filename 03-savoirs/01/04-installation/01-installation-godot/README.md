# Installer Godot 4.4.1 (sans .NET)

Pour ce cours nous utilisons Godot 4.4.1; la version « standard » (sans .NET/C#) suffit et est recommandée pour éviter d'installer le runtime .NET.



## Pourquoi sans .NET ?
- Simplicité d'installation pour les étudiant·e·s.
- Exportation vers le web ne fonctionne pas actuellement avec .NET
- Moins de dépendances et d'incompatibilités sur différentes plateformes.
- GDScript fournit tout le nécessaire pour les exercices du cours.

## Téléchargement
- Page officielle : https://godotengine.org/download
- Choisir la version 4.4.1 — télécharger le binaire standard (Standard version) pour votre OS :
  - Windows : archive ZIP ou installateur (Standard, x64)
  - macOS : dmg ou zip (standard)
  - Linux : AppImage / tar.xz (standard)

## Installation (résumé)
- macOS : ouvrir le `.dmg` ou dézipper l'archive, déplacer l'app Godot dans `/Applications` ou dossier choisi.
- Windows : dézipper l'archive et lancer `Godot_v4.4.1-stable_win64.exe` (aucune installation obligatoire si archive).
- Linux : rendre l'AppImage exécutable (`chmod +x Godot_v4.4.1.AppImage`) et lancer.

## Vérifier l'installation
- Lancer Godot : la fenêtre d'accueil affiche la version en haut.
- Créer un nouveau projet test pour vérifier que l'éditeur démarre et que l'export HTML5 fonctionne (voir export ci‑dessous).

## Export HTML5 / WebAssembly
- Pour exporter en HTML5, configurer le preset d'export HTML5 dans Godot (menu Export).
- Godot 4 génère des fichiers `.html`, `.wasm` et `.pck` prêts à être déployés sur GitHub Pages. (`/docs`)
- Tester localement via un petit serveur (ex. `Live Server dans VScode`) pour éviter les blocages CORS en ouvrant directement le fichier.

