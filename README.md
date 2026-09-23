# AKA Toolkit — release combinée

Ce dépôt **ne contient pas de code** : il assemble, en une seule Release GitHub, ce qu'il faut pour
développer sur la console AKA, à partir des dépôts qui font le vrai travail :

- **AKA-IDE** ([Jicehel-Aka/AKA-IDE-Linux-new](https://github.com/Jicehel-Aka/AKA-IDE-Linux-new)) — l'éditeur
  pour votre machine (Windows et Linux), en un seul exécutable autonome.
- **AKA-Love** ([Jicehel-Aka/akalove](https://github.com/Jicehel-Aka/akalove)) — le firmware Lua/Love2D.
- **MicroPython-AKA** ([Jicehel-Aka/MycroPython](https://github.com/Jicehel-Aka/MycroPython)) — le firmware MicroPython.

Résultat de chaque exécution : deux fichiers dans les Releases de **ce** dépôt —
`aka-ide-windows-vX.Y.0.zip` / `aka-ide-linux-vX.Y.0.zip` (l'IDE), et `SD_files.zip` (tout ce qu'il y a
à copier sur la carte SD, chemins déjà corrects — voir « Contenu de SD_files » ci-dessous).

## Avant le premier lancement

Les trois noms de dépôt sont déjà renseignés dans `.github/workflows/release.yml`. Il ne reste qu'une
condition : **chacun des trois dépôts doit avoir publié au moins une Release** (son propre `release.yml`, déjà en
place dans chacun) avant que ce workflow puisse fonctionner — il télécharge la dernière Release de
chaque dépôt, il ne compile rien lui-même.

## Contenu de `SD_files.zip`

```
SD_files/
├── AKA/
│   ├── languages.csv     dossier de scripts par langage (voir plus bas) — PARTAGÉ entre les firmwares
│   └── lang/              *.json — textes de l'interface (fr/en/de/it/es), utilisés par le menu commun
├── AKA_Love/               firmware.bin, meta.json, Picture.png, screen.bmp, game.txt, games/
├── micropython/            firmware.bin, meta.json, Picture.png, screen.bmp, docs/
└── py/                     main.py (lanceur) + les jeux d'exemple MicroPython, À PLAT (racine de la carte)
```

Copiez ce dossier `SD_files` tel quel à la racine de votre carte SD.

## `AKA/languages.csv` : le dossier de chaque langage, modifiable

Une ligne par langage de programmation : `lang_id,dossier_sur_la_carte_sd,nom_affiché`. C'est CE
fichier qui dit à AKA-Love (et bientôt à MicroPython) où ranger un fichier reçu par le protocole de
transfert USB (voir `docs/PROTOCOLE_TRANSFERT.md` du dépôt AKA-Love) — modifiez-le directement, ou
depuis AKA-IDE (il gère sa propre copie, éditable, et régénère celle-ci à l'export). Un nouveau
langage n'a besoin QUE d'une ligne de plus ici, pas d'une recompilation d'un firmware existant.

La copie de référence de ce dépôt (`AKA/languages.csv`, à la racine) prime sur celles embarquées dans
les zips d'AKA-Love et de MicroPython-AKA au moment de l'assemblage — pratique pour préparer le chemin
d'un langage AVANT même que son firmware sache le lire.

## Déclenchement

- **Manuel** : onglet Actions → « Build and release the AKA Toolkit » → Run workflow.
- **Planifié** : tous les lundis, pour rattraper les releases publiées entre-temps.
- **Automatique** (optionnel) : chacun des trois dépôts source peut prévenir celui-ci dès qu'il publie
  une release (déjà câblé dans leurs `release.yml`, étape « Notify AKA Toolkit », désactivée par
  défaut). Pour l'activer :
  1. Créez un [Personal Access Token](https://github.com/settings/tokens) avec le droit `repo` (accès
     à ce dépôt).
  2. Ajoutez-le comme secret **`TOOLKIT_DISPATCH_TOKEN`** dans chacun des trois dépôts source
     (Settings → Secrets and variables → Actions).

## Statut

Aucune partie de ce dépôt n'a tourné réellement (ni AKA-IDE, ni AKA-Love, ni MicroPython-AKA n'ont
encore de release publiée à ma connaissance) : `actionlint` valide la syntaxe, l'assemblage a été
rejoué à la main avec des fichiers factices (mêmes noms, mêmes dossiers), mais le tout premier vrai
run — une fois les deux noms de dépôt complétés et au moins une release publiée partout — reste à faire.
