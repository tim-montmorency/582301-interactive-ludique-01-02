# examples de commandes ffmpeg

Cette page contient des exemples pratiques pour préparer des médias (vidéo et audio)
destinés à être intégrés dans un projet Godot (export de textures animées, vidéos OGV,
audio OGG/WAV compatibles, etc.). Les commandes sont réutilisables dans des scripts
ou pipelines d'assets.

Rappels : Godot fonctionne bien avec des fichiers OGV (Theora+Vorbis) pour les
VideoPlayer, et avec des WAV/OGG pour l'audio. Adapter les formats selon la
version de Godot utilisée et la plateforme cible.

## Vidéo → OGV (Theora + Vorbis)

Conversion d'une vidéo (mp4/mov) en OGV (Theora vidéo + Vorbis audio) — bon pour compatibilité
avec les VideoPlayer classiques :

```bash
ffmpeg -i input.mp4 -c:v libtheora -qscale:v 7 -c:a libvorbis -qscale:a 5 output.ogv
```

Notes :
- Ajuster `qscale:v` (0..10) pour la qualité Theora (plus élevé = meilleure qualité, plus grosse taille).
- `qscale:a` contrôle la qualité Vorbis pour l'audio.
- Redimensionner et limiter le fps si nécessaire :

```bash
ffmpeg -i input.mp4 -vf "scale=640:-2,fps=30" -c:v libtheora -qscale:v 7 -c:a libvorbis -qscale:a 5 output_640.ogv
```

## Extraire l'audio d'une vidéo et encoder en OGG (Vorbis)

```bash
ffmpeg -i input.mp4 -vn -c:a libvorbis -qscale:a 5 extracted.ogg
```

## Audio → OGG Vorbis (pour effets/music) — qualité/vitesse

```bash
ffmpeg -i sound.wav -c:a libvorbis -qscale:a 5 sound.ogg
```

## Audio → WAV compatible Godot (PCM 16-bit LE)

Godot attend souvent des WAV PCM classiques (signed 16-bit little-endian). Pour convertir :

```bash
ffmpeg -i input.mp3 -c:a pcm_s16le -ar 44100 -ac 2 output_wav_44100_16bit.wav
```

Paramètres courants :
- `-ar 44100` : fréquence d'échantillonnage (44100 ou 48000).
- `-ac 2` : canaux (1 = mono, 2 = stéréo).

Pour produire un WAV mono 16-bit 48 kHz :

```bash
ffmpeg -i input.wav -c:a pcm_s16le -ar 48000 -ac 1 output_mono_48k.wav
```

## Normalisation / préparation de samples (éviter pops lors des loops)

Appliquer une très courte fondu d'entrée/sortie pour éviter les clics sur boucles courtes :

```bash
ffmpeg -i loop_source.wav -af "afade=t=in:st=0:d=0.01,afade=t=out:st=4.99:d=0.01" -c:a pcm_s16le -ar 44100 loop_ready.wav
```

Ici, on suppose une durée de ~5s ; adapter `st` et `d` en fonction de la longueur du sample.

## Découper / extraire un segment (sans réencodage quand possible)

Extraire un segment rapidement (copy) :

```bash
ffmpeg -ss 00:00:10 -to 00:00:20 -i input.mp4 -c copy segment.mp4
```

Si vous devez réencoder (p.ex. convertir audio) :

```bash
ffmpeg -ss 00:00:10 -to 00:00:20 -i input.mp4 -c:v libtheora -c:a libvorbis out_segment.ogv
```

## Sprite sheet / séquence d'images depuis une vidéo (pour textures animées)

Extraire une séquence d'images :

```bash
ffmpeg -i anim.mp4 -vf fps=12 -q:v 2 frame_%04d.png
```

Créer une sprite sheet (grille) :

```bash
ffmpeg -i anim.mp4 -vf "fps=12,scale=64:64,tile=5x5" -frames:v 1 spritesheet.png
```

## Batch : convertir tous les WAV en OGG (dossier d'assets)

```bash
for f in *.wav; do ffmpeg -i "$f" -c:a libvorbis -qscale:a 5 "${f%.wav}.ogg"; done
```

## Recommandations pratiques

- Préserver des masters non-compressés (WAV) pour l'édition, puis générer des versions compressées
	pour l'exécution (OGG Vorbis) afin de réduire la taille du jeu.
- Tester les fichiers dans Godot : importer et jouer pour valider latence et qualité.
- Pour des vidéos animées longues, préférez des formats compressés et optimisés; redimensionnez
	en amont pour réduire mémoire et temps de chargement.
- Documenter le pipeline d'assets (scripts + paramètres) pour reproductibilité et CI.

---

Si vous voulez, je peux :
- ajouter un petit script `make_assets.sh` prêt à l'emploi (convert + resize + checks), ou
- créer un job GitHub Actions qui installe ffmpeg et exécute ce script automatiquement.