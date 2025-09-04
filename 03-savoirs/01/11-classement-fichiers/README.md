# Classement des fichiers et des médias

## Structure de base de projet

Convention a utiliser pour des travaux pratiques ou le projet intégrateur.

```bash
# formule pour le nom du dossier racine: [NomDeFamille]-[Prénom]_[IdentifiantTravail]
# voici un exemple: 

- $nom_$prenom-tp1/ # nom du dépôt sur GitHub
    - docs/ # dossier avec l'exportation pour la web
    - projet/ # racine du projet Godot
        - project.godot # fichier avec infos du projet Godot, auto-crée
        - assets # Dossier contenant les ressources du projets
            - audio # Sous-dossier pour classer les élements audio
            - images # sous-dossier pour classer des élément graphique
        - ...
        - ... # d'autres sous-dossier et fichiers indexés par Godot
    - README.md # fichier Markdown avec la documentation et présentation du projet
    - ... 
    - ... # d'autres fichiers et dossiers qui ne seront pas indexés dans le projet Godot
```
