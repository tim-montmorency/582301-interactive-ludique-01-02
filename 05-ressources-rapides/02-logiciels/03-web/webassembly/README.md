# WebAssembly

## WebAssembly (Wasm)

WebAssembly est un format binaire et une cible d'exécution portable conçue pour le Web. Il permet
d'exécuter du code compilé (C/C++, Rust, etc.) dans le navigateur à des performances proches du
natif, tout en restant sécurisé (sandbox). Wasm se charge très rapidement et offre une API
interopérable avec JavaScript pour appeler des fonctions et échanger des données.

Points clés :
- Format compact et binaire optimisé pour le chargement.
- Exécution dans une sandbox côté client (sécurité et isolation).
- Interopérabilité avec JavaScript (appel de fonctions, accès mémoire via ArrayBuffer).
- Idéal pour des modules CPU-intensifs : moteurs physiques, traitement audio, rendu, etc.

## Emscripten

Emscripten est un compilateur qui transforme du code C/C++ (et parfois d'autres langages) en
WebAssembly (ou en asm.js). Il fournit une chaîne d'outils, des bibliothèques d'émulation POSIX
et des liaisons pour faciliter l'exécution de code natif dans le navigateur.

Fonctionnalités et usage :
- Convertit les binaires LLVM en Wasm/JS et génère le glue JavaScript nécessaire.
- Fournit des wrappers pour l'I/O, les sockets (via Emscripten sockets), le système de fichiers
	virtuel (MEMFS) et l'accès aux APIs Web.
- Permet d'embarquer des bibliothèques (SDL, OpenGL via WebGL, libsndfile, etc.).
- Options de compilation pour optimiser la taille (-O2, -Os) ou la vitesse (-O3), et pour
	contrôler l'interface JS/Wasm (embind, ccall/cwrap).

Cas d'usage typiques : portage de moteurs C/C++, bibliothèques multimédia, code scientifique.

## Export Web de Godot

Godot propose un export Web qui compile un projet pour être exécuté dans un navigateur. Selon
la version et la configuration, l'export utilise WebAssembly (Wasm) pour le moteur et JavaScript
comme glue pour charger le runtime et les ressources.

Remarques pratiques :
- L'export produit un ensemble de fichiers (HTML, .wasm, .js, assets) à déployer sur un serveur
	statique ou via GitHub Pages.
- Les performances sont bonnes pour des jeux 2D/3D légers à moyens, mais il faut optimiser les
	ressources (textures, sons) et la mémoire allouée à Wasm.
- Certaines fonctionnalités (plugins natifs, accès bas niveau) peuvent ne pas être supportées
	directement et nécessitent des ajustements.

## Liens utiles
- Specification WebAssembly : https://webassembly.org/
- Emscripten : https://emscripten.org/
- Godot Engine — Web export docs : https://docs.godotengine.org/fr/4.x/tutorials/export/exporting_for_web.html

