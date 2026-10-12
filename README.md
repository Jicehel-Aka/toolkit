# AKA Toolkit — release combinée

Ce dépôt **ne contient pas de code** : il assemble, en une seule Release GitHub, ce qu'il faut pour
développer sur la console AKA, à partir des dépôts qui font le vrai travail :

- **AKA-IDE** ([Jicehel-Aka/AKA-IDE-Linux-](https://github.com/Jicehel-Aka/AKA-IDE-Linux-)) — l'éditeur
  pour votre machine (Windows et Linux), en un seul exécutable autonome. Il **embarque déjà** les
  simulateurs PC (`aka-py-sim` pour MicroPython, `akalove-play` pour Love2D) : « Tester (PC) »
  fonctionne sans rien installer d'autre.
- **AKA-Love** ([Jicehel-Aka/akalove](https://github.com/Jicehel-Aka/akalove)) — le firmware Lua/Love2D.
- **MicroPython-AKA** ([Jicehel-Aka/MycroPython](https://github.com/Jicehel-Aka/MycroPython)) — le firmware MicroPython.

Résultat de chaque exécution : trois fichiers dans les Releases de **ce** dépôt —
`aka-ide-windows.zip` et `aka-ide-linux.tar.gz` (l'IDE pour votre machine), et `SD_files.zip` (tout ce
qu'il y a à copier sur la carte SD, chemins déjà corrects — voir « Contenu de SD_files » ci-dessous).

**Pour l'utilisateur final** : prends le fichier IDE de ton OS (Windows **ou** Linux), et copie le
contenu de `SD_files.zip` à la racine de ta carte SD. C'est tout — docs et exemples inclus.

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

Les trois dépôts sources ont désormais des releases publiées : **AKA-IDE v2.0.0**, **AKA-Love v0.12.0**,
**MicroPython-AKA v0.5.0**. Le workflow a été aligné sur les noms d'assets réels publiés par chacun
(`GamebuinoAKA-IDE-*`, `aka-love-sdcard-*.zip`, `micropython-aka-sdcard-*.zip`). Lance « Run workflow »
pour produire la première release combinée.
