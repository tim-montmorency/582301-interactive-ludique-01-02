# FFmpeg

## Présentation

FFmpeg est une boîte à outils en ligne de commande puissante pour manipuler l'audio et la
vidéo : conversion de formats, encodage/décodage, extraction, filtrage, création d'images
à partir de séquences et bien plus. C'est un composant essentiel pour les intégrateurs
multimédia qui doivent préparer des assets pour le Web, les jeux ou les expériences interactives.

Licence : principalement LGPL/GPL selon la configuration et les codecs inclus. Site officiel : https://ffmpeg.org/

## Pourquoi FFmpeg pour un intégrateur multimédia / jeux

- Universel : supporte quasiment tous les formats audio/vidéo existants (MP4, WebM, OGG, WAV, PNG, JPEG, etc.).
- Scriptable : s'utilise en batch dans des pipelines d'export, CI ou scripts d'automatisation.
- Léger et portable : disponible sur macOS, Linux et Windows; peut être intégré dans des conteneurs.
- Puissant : filtres vidéo (scale, pad, overlay, tile...), outils d'analyse (`ffprobe`), encodages matériels (NVENC, VAAPI).

Pour les jeux, FFmpeg facilite :
- convertir et compresser audio pour réduire la taille des téléchargements (WAV → OGG/Opus/AAC),
- produire textures animées (vidéo → image sequence → sprite sheet),
- générer vignettes et miniatures automatisées,
- transcoder vidéos pour web (H.264/MP4 ou VP9/WebM selon la plateforme).

## Installation rapide

- macOS (Homebrew) :

```bash
brew install ffmpeg
```

- Ubuntu/Debian (paquet officiel ou PPA pour build complet) :

```bash
sudo apt update
sudo apt install ffmpeg
```

- Windows : télécharger la build depuis https://ffmpeg.org/download.html (ou utiliser Chocolatey).

Pour disposer des codecs et options avancées (NVENC, libvpx, libx264, libopus...), installer une build complète.


## Liens utiles

- Documentation officielle FFmpeg : https://ffmpeg.org/documentation.html
- Guide des options : https://trac.ffmpeg.org/wiki/Encode/H.264
- ffprobe (analyse) : https://ffmpeg.org/ffprobe.html

