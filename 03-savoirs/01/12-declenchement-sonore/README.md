# Déclenchement d’échantillons sonores



## 0) Contexte Web en 30 s

* **Depuis Godot 4.3, l’export Web est *monothread* par défaut** (plus compatible sur iOS/macOS et les portails comme itch.io). Pour éviter les craquements audio en mono-thread, Godot utilise par défaut un **mode de lecture “Sample” via Web Audio**, pensé pour la faible latence — mais **avec des limites** (pas d’AudioEffects, pas de réverb/Doppler, génération procédurale non supportée, spatialisation parfois imparfaite). ([Godot Engine][1], [docs.godotengine.org][2])
* Si vous **activez les threads** pour retrouver la lecture “Stream” (latence plus haute mais effets et pitch complets), vous devrez **isoler le site** (COOP/COEP) ou **activer PWA** pour injecter automatiquement ces en-têtes. ([docs.godotengine.org][3], [Godot Engine][1])
* Côté navigateur : **audio et plein écran doivent être déclenchés suite à un geste utilisateur** (\_input/\_unhandled\_input). De plus, **l’onglet inactif met en pause** `_process()`/`_physics_process()`. Prévoyez-le. ([docs.godotengine.org][3])

---

## 1) Choisir le bon nœud audio (avec docs)

* **Sons non-positionnels** (UI, musique) → `AudioStreamPlayer`. ([docs.godotengine.org][4])
* **Sons 2D spatialisés** → `AudioStreamPlayer2D` (+ `AudioListener2D` si vous voulez déplacer l’“oreille”). ([docs.godotengine.org][5])
* **Sons 3D** → `AudioStreamPlayer3D` (+ `AudioListener3D`). ([docs.godotengine.org][6])
* **Type de ressource** lue par ces lecteurs : `AudioStream` (WAV/OGG/MP3, etc.). ([docs.godotengine.org][7])

---

## 2) Déclencher un son par **événement** (signals) — compatible Web

### Exemple : jouer un SFX quand le joueur entre dans une zone

Scène : `Area2D` + `CollisionShape2D` + `AudioStreamPlayer2D` (SFX assigné).

```gdscript
# Area2D.gd (Godot 4.x)
extends Area2D
@onready var sfx: AudioStreamPlayer2D = $AudioStreamPlayer2D

func _ready() -> void:
    body_entered.connect(_on_body_entered)  # signal idiomatique

func _on_body_entered(_body: Node) -> void:
    sfx.play()
```

`Area2D` émet `body_entered`/`area_entered` ; les **signals** sont la façon idiomatique de réagir aux événements. ([docs.godotengine.org][8])

### Exemple : timing précis avec `AnimationPlayer`

Dans une cinématique/UI, ajoutez une **Call Method Track** qui appelle `play()` au timecode voulu (aucun problème côté Web). ([docs.godotengine.org][9])

---

## 3) “Déverrouiller” l’audio sur navigateur (autoplay)

Sur le Web, l’audio **ne démarre qu’après un geste** (clic/toucher/touche). Montrez un écran “Cliquer pour activer l’audio”, puis jouez un son court pour initialiser le contexte.

```gdscript
# SoundGate.gd (attaché à un Control en overlay)
extends Control
@onready var gate := $Panel    # votre overlay
@onready var ping: AudioStreamPlayer = $Ping  # petit son (ou silence court)

func _ready() -> void:
    if OS.has_feature("web"):
        gate.visible = true

func _unhandled_input(event: InputEvent) -> void:
    if not OS.has_feature("web") or not gate.visible:
        return
    if event is InputEventMouseButton or event is InputEventKey or event is InputEventScreenTouch:
        gate.visible = false
        if ping.stream:
            ping.play()  # “débloque” l’audio, puis votre jeu peut jouer d’autres sons
```

Pourquoi ? **Le plein écran et l’audio doivent découler d’un événement d’entrée**, pas d’un simple `Input.is_action_pressed()` interrogé hors callback. ([docs.godotengine.org][3])

---

## 4) **Playback Type** : *Sample* (par défaut Web) vs *Stream*

* **Par défaut sur le Web** : `AudioStreamPlayer[2D/3D]` utilisent **Sample** → **latence faible**, mais **pas d’AudioEffects** (ni réverb/Doppler) et certains comportements (pitch, spatialisation avancée) peuvent être limités. Réglage global : *Project Settings → Audio > General > Default Playback Type (web)*, ou propriété **`playback_type`** par nœud. ([docs.godotengine.org][2])
* **Besoin d’effets/pitch complets** ? Passez **en “Stream”** (par nœud ou projet) **et** utilisez l’export **avec threads** + **isolation COOP/COEP** (ou **PWA** qui injecte ces en-têtes). Attendez-vous à **plus de latence**. ([docs.godotengine.org][3], [Godot Engine][1])

**Astuce perf** : pré-“enregistrer” vos flux comme samples au chargement pour éviter un coût la première fois :

```gdscript
# Pendant le chargement d'un niveau (Web + Sample)
for p in get_tree().get_nodes_in_group("preload_sfx"):
    if p is AudioStreamPlayer and p.stream:
        AudioServer.register_stream_as_sample(p.stream)
```

Cette API et le principe “Sample playback” sont détaillés dans le billet 4.3. ([Godot Engine][1])

---

## 5) Optimiser **assets** et **mix** pour le Web

1. **Formats & taux d’échantillonnage**

* **SFX courts** : utilisez des WAV (PCM), mono si possible ; **musique/boucles** : OGG (Vorbis). Gardez **un seul sample rate** (44.1 kHz ou 48 kHz) pour éviter les conversions. ([docs.godotengine.org][7])

2. **Polyphonie & voix simultanées**

* Limitez `max_polyphony` sur vos lecteurs et groupez vos SFX par bus (UI/SFX/Music) pour gérer les volumes globalement (même si les **effets** de bus sont **désactivés en Sample**). ([docs.godotengine.org][4])

3. **2D/3D**

* En mode Sample, la **spatialisation** peut être moins fidèle ; si nécessaire, préférez `AudioStreamPlayer` (non-positionnel) + panoramique simple plutôt que des chaînes complexes d’effets (non disponibles). ([docs.godotengine.org][2])

4. **Mobile & onglets**

* Le **matériel mobile** a moins de marge : gardez vos sons courts, évitez de déclencher trop de voix à la fois, et souvenez-vous que **l’onglet inactif met le jeu en pause** (pas de timers/tweens/audio nouveaux pendant ce temps). ([docs.godotengine.org][3])

---

## 6) Déclencheurs courants (patterns prêts à l’emploi)

* **Collision/zone** → `Area2D.body_entered` → `sfx.play()`. ([docs.godotengine.org][8])
* **UI** → Bouton `pressed` → `click_sfx.play()` (fonctionne bien puisque c’est un geste utilisateur). ([docs.godotengine.org][4])
* **Cutscene** → `AnimationPlayer` + *Call Method Track* → appelle `play()` exactement au timecode. ([docs.godotengine.org][9])
* **3D “au point d’explosion”** → placez un `AudioStreamPlayer3D` à la position puis `play()`. ([docs.godotengine.org][6])

---

## 7) Check-list d’export **HTML5** orientée audio

* **Single-thread (par défaut)** : simple et compatible → **Playback = Sample** (faible latence, pas d’effets). ([Godot Engine][1])
* **Multi-thread** : si vous avez absolument besoin d’effets/pitch → activez **Thread Support** et assurez **COOP/COEP** côté serveur **ou** cochez **PWA → Ensure Cross Origin Isolation Headers**. ([docs.godotengine.org][3], [Godot Engine][1])
* **Autoplay** : affichez un **overlay “Cliquer pour activer l’audio”** et démarrez un son dans `_input/_unhandled_input`. ([docs.godotengine.org][3])

---

### Liens rapides (docs officielles des nœuds)

`AudioStreamPlayer` • `AudioStreamPlayer2D` • `AudioStreamPlayer3D` • `AudioListener2D` • `AudioListener3D` • `AudioStream` • `AnimationPlayer / Call Method Track`. ([docs.godotengine.org][4])

Si tu veux, je peux te fournir un *starter* Godot minimal (scènes 2D/3D + overlay d’activation audio + réglages projet Web) prêt à exporter.

[1]: https://godotengine.org/article/progress-report-web-export-in-4-3/ "Web Export in 4.3 – Godot Engine"
[2]: https://docs.godotengine.org/en/latest/tutorials/export/exporting_for_web.html?utm_source=chatgpt.com "Exporting for the Web - Godot Docs"
[3]: https://docs.godotengine.org/en/4.4/tutorials/export/exporting_for_web.html?utm_source=chatgpt.com "Exporting for the Web — Godot Engine (4.4) documentation in ..."
[4]: https://docs.godotengine.org/en/4.4/classes/class_audiostreamplayer.html "AudioStreamPlayer — Godot Engine (4.4) documentation in English"
[5]: https://docs.godotengine.org/en/4.4/classes/class_audiostreamplayer2d.html "AudioStreamPlayer2D — Godot Engine (4.4) documentation in English"
[6]: https://docs.godotengine.org/en/4.4/classes/class_audiostreamplayer3d.html "AudioStreamPlayer3D — Godot Engine (4.4) documentation in English"
[7]: https://docs.godotengine.org/en/4.4/classes/class_audiostream.html?utm_source=chatgpt.com "AudioStream — Godot Engine (4.4) documentation in English"
[8]: https://docs.godotengine.org/en/4.4/classes/class_area2d.html?utm_source=chatgpt.com "Area2D — Godot Engine (4.4) documentation in English"
[9]: https://docs.godotengine.org/en/stable/tutorials/animation/animation_track_types.html "Animation Track types — Godot Engine (stable) documentation in English"


Voir l'[exemple](../30-projet-simple/) 

