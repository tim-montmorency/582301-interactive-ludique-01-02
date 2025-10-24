# Indication visuelle et sonore des réussites et échecs d’interaction

Cette fiche explique comment concevoir des retours visuels et sonores clairs et utiles pour indiquer la réussite ou l'échec d'une interaction en jeu (ex. ouverture d'une porte, collecte d'un objet, tentative d'action invalide). L'objectif est d'aider les étudiants à produire des retours qui améliorent l'apprentissage, l'immersion et l'accessibilité.

## Principes clés

- Rapidité : le feedback doit arriver immédiatement (≤ 200–300 ms) pour être perçu comme lié à l'action.
- Clarté : distinguer visuellement et auditivement réussite vs échec par des signifiants contrastés (couleurs, formes, timbres sonores).
- Redondance sensorielle : combiner visuel + audio (et vibration/haptique si disponible) pour renforcer la compréhension et l'accessibilité.
- Cohérence : utiliser un vocabulaire visuel/sonore constant dans tout le jeu (mêmes couleurs/icônes pour les mêmes états).
- Non-intrusivité : informer sans interrompre le flow ; privilégier des retours légers pour les interactions fréquentes.

## Design visuel — bonnes pratiques

- Couleurs et contrastes :
  - Succès : verts, teintes chaudes mais non agressives ; icône check, glow doux.
  - Échec : rouges/oranges, shake léger, bordure pulsante.
  - Attention : utiliser des palettes accessibles — testez avec simulations de daltonisme.
- Animation :
  - Micro-animations courtes (0.15–0.35 s) pour attirer l'œil sans distraire.
  - Utiliser easing (ease-out pour l'apparition, spring pour un rebond) pour rendre le feedback naturel.
- Placement :
  - HUD / écran : pour infos globales (points, inventaire).
  - In-world : signaux positionnés au niveau de l'objet (ex. clé qui brille) quand l'information est liée à un objet du monde.
  - Contextuel : messages qui apparaissent près du joueur/objet et disparaissent si non pertinents.
- Hiérarchie : afficher d'abord l'information critique (danger, échec bloquant), puis les informations secondaires (points, progression).

## Design sonore — bonnes pratiques
- Intégrité sonore : choisir des sons courts (≤ 400 ms) distincts pour succès/échec.
- Timbre et hauteur :
  - Succès : timbres brillants, montée en pitch court.
  - Échec : timbres sourds, descente en pitch, consonances mineures.
- Volume adaptatif : ajuster en fonction du mix global pour éviter la saturation.
- Latence : jouer le son immédiatement sur l'événement ; précharger les assets audio pour éviter delays.

## Mapping visuel ↔ sonore
- Les combinaisons visuel+son doivent être consistantes : par ex. un glow vert + un chime pour succès, un flash rouge + un bruit sourd pour échec.
- Tester la redondance : s'assurer que si une modalité est indisponible (son coupé), l'autre suffit à transmettre l'information.

## Feedbacks contextuels — exemples concrets
- Interaction réussie (ex. ouverture porte après clé) :
  - Visuel : animation d'ouverture + particules lumineuses + HUD icon pulse.
  - Son : chime bref + bruit d'arc/tonalité ascendante.
- Interaction échouée (ex. porte verrouillée) :
  - Visuel : shake de la porte, bordure rouge éphémère, petit message "Il vous manque une clé".
  - Son : son sourd court + léger bruit de rebond.
- Collecte d'objet : icône HUD apparait et pulse, petit jingle.
- Échec non-bloquant (ex. action impossible ici) : micro-feedback très court, éviter messages persistants.

## Accessibilité
- Sous-titres / texte : toujours fournir un texte bref pour les retours importants.
- Couleurs : ne pas dépendre uniquement de la couleur — ajouter icônes/formes.
- Volume et options : permettre réglage du volume des indices et la désactivation des effets visuels (pour épileptiques).

## Techniques d'implémentation (Godot) — recommandations rapides
- Précharger les sons avec `AudioStreamPlayer` ou `AudioStreamPlayer2D` pour minimiser la latence.
- Utiliser `AnimationPlayer` ou `Tween` (ou `AnimationTree`) pour micro-animations (scale, modulate, offset).
- Signaux : centraliser les événements (ex. `Main` ou `GameState`) et émettre `interaction_success`, `interaction_fail` que le HUD/Audio écoute.

### Mini-exemple (pseudocode GDScript)

```gdscript
# Dans Main.gd (autoload)
signal interaction_success(text:String)
signal interaction_fail(text:String)

func notify_success(text:String = ""):
    emit_signal("interaction_success", text)

func notify_fail(text:String = ""):
    emit_signal("interaction_fail", text)

# Dans HUD.gd (CanvasLayer)
func _ready():
    Main.connect("interaction_success", Callable(self, "_on_success"))
    Main.connect("interaction_fail", Callable(self, "_on_fail"))

func _on_success(text:String) -> void:
    $SuccessIcon.show()
    $SuccessIcon.play("pulse")
    $AudioSuccess.play()
    $MessageLabel.text = text
    $MessageLabel.show()

func _on_fail(text:String) -> void:
    $FailIcon.show()
    $FailIcon.play("shake")
    $AudioFail.play()
    $MessageLabel.text = text
    $MessageLabel.show()
```

## Tests et protocole de validation
- Latence : mesurer que le son et l'animation démarrent quasi-instantanément après l'action (≤ 300 ms).
- Clarté : en user-test, demander au joueur d'expliquer l'état après feedback ; viser ≥ 90 % de compréhension rapide.
- Accessibilité : vérifier mode sans son (sous-titres/messages visibles) et mode haute-contraste.
- Non-intrusivité : s'assurer que les feedbacks répétés ne deviennent pas irritants (limiter fréquence ou ajouter dampening).

## Checklist rapide pour TP
- Y a-t-il un son distinct pour succès vs échec ?
- Le visuel (couleur/icone/animation) est-il cohérent et lisible à distance normale de jeu ?
- Le feedback apparaît-il en < 300 ms ?
- Le message est-il compréhensible en moins de 2 secondes ?
- Les options d'accessibilité sont-elles présentes (sous-titres, volume, contraste) ?

