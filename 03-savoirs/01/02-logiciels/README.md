# Choix du logiciel

## Court comparatif des technologies d'intégration multimédia

- Web (HTML5 / JavaScript — WebAudio, Canvas, WebGL)
  - Avantages : très portable (navigateur), déploiement simple, large audience, bon pour prototypage et distribution web.
  - Limites : variations entre navigateurs, latence audio parfois supérieure, accès matériel restreint.

- Moteurs de jeu (Unity, Unreal, Godot)
  - Avantages : puissance 2D/3D, accélération GPU, outils d'édition, export multi-plateforme, écosystèmes riches.
  - Limites : taille des builds, complexité, licences (Unity/Unreal) ou maturité (Godot) à considérer.

- Frameworks multimédia temps réel (Max/MSP, Pure Data, SuperCollider)
  - Avantages : excellent pour prototypage son interactif et installations, flux de données visuels, faible courbe pour audio.
  - Limites : export/distribution moins standardisée pour le web/mobile, intégration visuelle limitée sans ponts.

- Bibliothèques natives (C++ : JUCE, Qt, SDL, frameworks audio natifs)
  - Avantages : contrôle fin sur la performance et la latence, accès matériel, builds natifs optimisés.
  - Limites : travail multi-plateforme plus chargé, packaging et mises à jour plus lourds.

- Environnements de creative coding (Processing, p5.js)
  - Avantages : prototypage rapide, pédagogie, bonne intégration web (p5.js).
  - Limites : pas toujours adaptés aux projets à haute performance ou exigences temps réel strictes.


## Enjeux de portabilité et performance

- Portabilité
  - Vérifier plateformes cibles (desktop, web, iOS, Android, installations embed). Les choix web facilitent la portabilité, mais les différences de navigateur et de matériel impactent le comportement.
  - Considérer dépendances natives (plugins, drivers) qui peuvent bloquer certains environnements.

- Performance
  - Mesurer : framerate, latence audio (ms), consommation CPU/GPU, mémoire, consommation batterie sur mobile.
  - Le passage au natif offre souvent une meilleure performance et un accès bas niveau (threads, SIMD, drivers), mais augmente la charge d'intégration.

- Trade-off
  - Portabilité élevée ↔ contrôle/performance bas niveau. Choisir selon contraintes réelles (exigences audio faible latence, rendu 3D intensif, distribution web simple).


## Système d'élimination (processus de décision rapide)

1. Besoin de latence audio < 10 ms ou contrôle matériel bas niveau ? → privilégier native (JUCE, frameworks audio natifs) ou moteurs optimisés.
2. Besoin d’un rendu 3D avancé / physiques complexes ? → Unity ou Unreal.
3. Priorité déploiement web / partage immédiat ? → HTML5 (WebAudio, WebGL, p5.js).
4. Prototype sonore/interaction rapide sans contrainte de packaging ? → Max/MSP, Pure Data, SuperCollider.
5. Installation interactive avec capteurs/IO spécifiques ? → C++ / Qt / JUCE ou moteur avec plugins (accès hardware fiable).

Si plusieurs réponses « oui », évaluer le compromis via la grille ci-dessous (portabilité vs performance vs coût/complexité).


## Composantes cruciales à évaluer

- Portabilité : plateformes supportées, compatibilité navigateurs, facilité d'installation (app store, package). 
- Performance : fps stable, latence audio, consommation CPU/GPU, mesures sur cibles représentatives.
- Fonctionnalités : codecs supportés, APIs (WebUSB, WebMIDI, OpenGL/Metal/Vulkan), plugins/extensions.
- Workflow : temps de développement, outils d'édition, hot-reload, debugging, CI/CD.
- Taille et packaging : poids des builds, contraintes réseau pour le déploiement.
- Licence & coût : modèle commercial, restrictions pédagogiques ou commerciales.
- Communauté & support : documentation, exemples, plugins.
- Accessibilité & internationalisation : facilité d'ajouter aides, tailles cibles, textes alternatifs.
- Sécurité et vie privée : accès capteurs, permissions utilisateur, collecte de données.


## Recommandation pratique

- Commencez par un prototype sur la plateforme la plus simple (web ou Max/p5.js) pour valider concepts et interactions.
- Mesurez tôt la performance cible sur appareils représentatifs.
- Si un seul critère critique (par ex. latence strictement basse), passez tôt au choix technologique adapté (native ou moteur optimisé) pour éviter retravail massif.


## Fiches logiciel

<!-- start-replace-subnav -->
* [Outils de programmation compilé](/03-savoirs/01/02-logiciels/00-compile/)
    * [Bibliothèques natives (JUCE, Qt, SDL)](/03-savoirs/01/02-logiciels/00-compile/juce/)
    * [openFrameworks](/03-savoirs/01/02-logiciels/00-compile/openframeworks/)
    * [SDL (Simple DirectMedia Layer)](/03-savoirs/01/02-logiciels/00-compile/sdl/)
* [Outils de programmation interprétée ](/03-savoirs/01/02-logiciels/01-interprete/)
    * [Max (Max/MSP)](/03-savoirs/01/02-logiciels/01-interprete/max/)
    * [p5.js](/03-savoirs/01/02-logiciels/01-interprete/p5js/)
    * [Processing / p5.js (creative coding)](/03-savoirs/01/02-logiciels/01-interprete/processing/)
    * [Pure Data (Pd)](/03-savoirs/01/02-logiciels/01-interprete/pure-data/)
    * [SuperCollider](/03-savoirs/01/02-logiciels/01-interprete/supercollider/)
    * [TouchDesigner](/03-savoirs/01/02-logiciels/01-interprete/touchdesigner/)
* [Engin de jeu](/03-savoirs/01/02-logiciels/02-engin-de-jeux/)
    * [Godot](/03-savoirs/01/02-logiciels/02-engin-de-jeux/godot/)
    * [Unity](/03-savoirs/01/02-logiciels/02-engin-de-jeux/unity/)
    * [Unreal Engine](/03-savoirs/01/02-logiciels/02-engin-de-jeux/unreal/)
* [10-creation-visuelle](/03-savoirs/01/02-logiciels/10-creation-visuelle/)
* [20-creation-sonore](/03-savoirs/01/02-logiciels/20-creation-sonore/)
* [Comparatif rapide des frameworks](/03-savoirs/01/02-logiciels/framework_comparison/)
<!-- end-replace-subnav -->



