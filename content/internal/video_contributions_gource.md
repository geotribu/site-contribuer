---
title: "Gource : l'historique des contributions en vidéo"
subtitle: Entrez dans la GéoHistoire
categories:
    - article
    - Geotribu
comments: true
date: 2023-07-29
description: Tutoriel de prise en main de Gource, le logiciel qui permet de générer une datavisualisation animée en vidéo à partir de l'historique des contributions Git.
icon : simple/interactiondesignfoundation
tags:
    - coulisses
    - GéoCapot
    - Git
    - Gource
    - historique
    - valorisation
---

# Créer une data-visualisation des contributions à Geotribu avec Gource

![logo Gource](https://cdn.geotribu.fr/img/logos-icones/logiciels_librairies/gource.webp){: .img-thumbnail-left }

Le(s) site(s) Geotribu étant basés sur Git, on profite ainsi de l'outillage de l'écosystème ; GitHub bien sûr mais aussi d'autres outils moins connus.

Cette page décrit comment on peut utiliser Gource pour générer des vidéos rétrospectives sur les contributions au site (voir [un exemple sur la rétrospective 2022]({{ config.extra.geotribu_main_site }}articles/2023/2023-01-30_voeux-geotribu-2023/#retrospective-2022)). Ou celle portant sur l'année 2024 :

<iframe title="Rétrospective 2024 des contributions au site principal de Geotribu" width="100%" height="315" src="https://video.osgeo.org/videos/embed/39101d03-465e-491c-8894-c5d1254e9b1d?warningTitle=0" frameborder="0" allowfullscreen="" sandbox="allow-same-origin allow-scripts allow-popups allow-forms"></iframe>

C'est un outil qui est connu et largement utilisé dans différents projets (voir [ici l'exemple de MapServer]({{ config.extra.geotribu_main_site }}rdp/2020/rdp_2020-05-15/?h=gource#mapserver-760)). Dans la communauté géomatique, on a régulièrement la vidéo retraçant les contributions à OpenStreetMap sur une fenêtre spatio-temporelle. A titre d'expérience personnelle, je l'avais utilisé pour retracer le tavail de développement sur le plugin QGIS d'Isogeo : <https://www.youtube.com/watch?v=URoH0osLY_4>.

## Prérequis

- [ ] avoir un ordinateur qui dispose d'une bonne carte graphique
- [ ] avoir cloné [la dernière version du dépôt du site web](../edit/local_edition_setup.md)
- [ ] vérifier que tous les contributeurs ont bien un seul nom (en exécutant `git shortlog -nse` et en complétant le fichier `.mailmap` en conséquence)
- [ ] avoir du temps devant soi (ou un double écran)

----

## Préparation : un petit coup d'oeil à l'historique Git

Gource se basant sur l'historique Git, c'est l'occasion de regarder que l'historique est cohérent et le cas échéant de l'ajuster en éditant le fichier `.mailmap`. Pour regarder l'historique sur une année, on peut utiliser la commande `shortlog` :

```sh
git shortlog -nse --since="01 Jan 2025" --before="31 Dec 2025"
```

Ce qui donne par exemple (les adresses mails ont été remplacées par des fausses ici) :

```sh
   382  Guilhem Allaman <dev@users.noreply.github.com>
   285  Julien Moura <1596222+Guts@users.noreply.github.com>
    94  Michaël Galien <64089998+michael-cd30@users.noreply.github.com>>
    85  Geotribot <49699333+dependabot[bot]@users.noreply.github.com>
    76  Thomas Szczurek-Gayant <121474664+thomas-szczurek@users.noreply.github.com>
    66  Karl Tayou <49986468+TANK2003@users.noreply.github.com>
    35  Nicolas Rochard <Doctor-Who@users.noreply.github.com>
    32  Florian Boret <figeofr@users.noreply.github.com>
    31  Paul Blottiere <blottiere.paul@users.noreply.github.com>
    28  Camille Monchicourt <camille.monchicourt@users.noreply.github.com>
    25  Marc Ducobu <marc.ducobu@users.noreply.github.com>
    21  Satya Minguez <97035327+Satya-cd30@users.noreply.github.com>
     8  Jean-Baptiste DESBAS <jean-baptiste.desbas@hautsdefrance.fr>
     4  Delphine Montagne <17273438+KazeNoOni@users.noreply.github.com>
     3  Arnaud Vandecasteele <arnaud.sig@users.noreply.github.com>
     3  Loïc Bartoletti <lbartoletti@users.noreply.github.com>
     3  Thomas Bouché <53937041+ThomasBouche@users.noreply.github.com>
     2  Even Rouault <even.rouault@users.noreply.github.com>
     2  Gabriel Poujol <gpoujol@users.noreply.github.com>
     2  Jérémie Prud'homme <p.jeremie@users.noreply.github.com>
     2  Jérémy Garniaux <jeremy@users.noreply.github.com>
     2  Xavier Thauvin <xavier.thauvin@users.noreply.github.com>
     1  Célestin Huet <celestin.huet@users.noreply.github.com>
     1  Harrissou Sant-anna <delazj@users.noreply.github.com>
     1  Loïc Ecault <loic.ecault@users.noreply.github.com>
     1  Loïc Moisan <loic.moisan.perso@users.noreply.github.com>
     1  Maël Reboux <m.reboux@users.noreply.github.com>
     1  Romain Tourte <rtourte@users.noreply.github.com>
     1  Stéphane Mével-Viannay <mviewer@users.noreply.github.com>
     1  Stéphane Rolle <47242457+stephanerolle@users.noreply.github.com>
```

----

## Installation

!!! note
    L'installation sur Windows est plutôt facile de mémoire (enfin [celle de ffmpeg](https://fr.wikihow.com/installer-FFmpeg-sur-Windows) semble un peu exotique), donc je n'en parle pas ici.

> Sur une distribution basée sur Debian (Ubuntu 22.04 à date) :

Il se trouve que la version disponible dans les dépôts officiels est (à date) la `0.51`. Il est préférable d'utiliser la dernière version (`0.54` à date) depuis les sources.

### Dépendances

On installe d'abord les dépendances requises pour la compilation :

```sh
sudo apt install ffmpeg
sudo apt install libsdl2-dev libsdl2-image-dev libpcre2-dev libfreetype6-dev libglew-dev libglm-dev libboost-filesystem-dev libpng-dev libtinyxml-dev
```

### Téléchargement

On définit la version que l'on souhaite :

```sh
GOURCE_VERSION=0.54
```

On télécharge et extrait :

```sh
wget https://github.com/acaudwell/Gource/releases/download/gource-${GOURCE_VERSION}/gource-${GOURCE_VERSION}.tar.gz -O /tmp/gource.tar.gz
tar xvfz /tmp/gource.tar.gz --directory /tmp
```

### Compilation

On lance la configuration préalable puis la compilation :

```sh
cd /tmp/gource-${GOURCE_VERSION}
./configure
make
sudo make install
```

On vérifie que l'installation s'est bien déroulée :

```sh
gource --help
```

On nettoie l'environnement de compilation :

```sh
sudo apt remove libpcre2-dev libfreetype6-dev libglm-dev libboost-filesystem-dev libpng-dev libtinyxml-dev
```

----

## Configuration

A partir de maintenant, les commandes suivantes sont à exécuter dans le dossier du dépôt Git local du site Geotribu.

### Récupérer les ressources

Cloner le projet avec les fichiers de configuration dédiés à Gource :

```sh
git clone https://github.com/geotribu/tooling-gource.git
# ou git clone git@github.com:geotribu/tooling-gource.git
```

Copier les fichiers liés à Gource dans le dépôt Git du site principal (adapter les chemins à votre environnemtn local) :

```sh
cp tooling-gource/background_sombre_dalle.webp ~/Git/Geotribu/website/
cp tooling-gource/gource.ini ~/Git/Geotribu/website/
cp -R tooling-gource/avatars ~/Git/Geotribu/website/
```

### Fichier `gource.ini`

On repart du fichier `gource.ini` pour avoir une commande plus lisible et pouvoir jouer sur les paramètres plus facilement, notamment la fourchette de dates pour filtrer l'historique.

```ini linenums="1" title="Fichier de configuration gource.ini"
--8<-- "https://raw.githubusercontent.com/geotribu/tooling-gource/main/gource.ini"
```

----

## Show time !

![icône globe video](https://cdn.geotribu.fr/img/internal/icons-rdp-news/animation_video.png "icône globe video"){: .img-thumbnail-left }

Allez, on a tout ce qu'il faut, c'est parti pour produire la vidéo !
On se place dans le dossier du dépôt Git local du site (adapter le chemin à votre propre environnement) où on a copié les fichiers liés à Gource :

```sh
cd ~/Git/Geotribu/website
```

Concrètement, on lance la commande qui va lancer l'animation en plein écran, filmer l'écran et encoder/compresser le tout dans un fichier mp4 :

```sh
gource --load-config gource.ini -o - | ffmpeg -y -r 25 -f image2pipe -vcodec ppm -i - -vcodec libx265 -preset fast -pix_fmt yuv420p -crf 26 -threads 0 -bf 0 ./geotribu_history.mp4
```

Décortiquons :

1. `--load-config gource.ini`: on charge les paramètres de configuration.
1. `-o -`: cela indique à Gource de rediriger la sortie vers la sortie standard (_stdout_) au lieu d'écrire directement dans un fichier. La sortie sera ensuite utilisée comme entrée pour la commande suivante, ici `ffmepg`.
1. `|`: C'est le symbole de tube (_pipe_) qui prend la sortie de la première commande (`gource`) et la transmet à la deuxième commande (`ffmpeg`).
1. `ffmpeg`: c'est la bibliothèque de traitement multimédia qu'on utilise pour encoder la vidéo dans un fichier
1. `-y`: écraser automatiquement les fichiers de sortie existants sans poser de questions.
1. `-r 25`: débit d'images de sortie (25 images par seconde dans ce cas).
1. `-f image2pipe`: on indique le format d'entrée, qui est une série d'images en flux (_image2pipe_).
1. `-vcodec ppm`: codec vidéo utilisé pour le flux d'entrée, qui est PPM (_Portable Pixel Map_)
1. `-i -`: on indique à `ffmpeg` de prendre l'entrée depuis la sortie standard (_stdout_) de la commande précédente (`gource`).
1. `-vcodec libx265`: codec vidéo de sortie, qui est libx265, un codec vidéo largement utilisé pour la compression vidéo.
1. `-preset fast`: préréglage de vitesse de la compression vidéo, en choisissant "ultrafast" pour maximiser la vitesse au détriment de la taille du fichier.
1. `-pix_fmt yuv420p`: format de pixel de sortie pour la vidéo.
1. `-crf 26`: facteur de qualité Constant Rate Factor (CRF) pour la vidéo. Plus la valeur est faible (comme 1), meilleure est la qualité mais plus volumineux sera le fichier de sortie. Concernant le codec x265, il est conseillé un facteur entre 25 et 30 (ffmpeg utilise 28 par défaut).
1. `-threads 0`: on indique à `ffmpeg` d'utiliser autant de threads que possible pour l'encodage.
1. `-bf 0`: on désactive l'utilisation de cadres B (B-frames) pour la compression vidéo.
1. `geotribu_history.mp4`: fichier de sortie qui contiendra la vidéo finale.

## Combiner plusieurs dépôts

`Gource` permet également de créer une vidéo qui montre les contributions sur plusieurs dépôts !

Pour cela, il est nécessaire de d'abord créer des logs sur les différents dépôts, qui doivent être clonés en local :

```sh
mkdir -p gource_2025

gource --output-custom-log gource_2025/english-blog.txt english-blog
gource --output-custom-log gource_2025/infra.txt infra
gource --output-custom-log gource_2025/qchat.txt qchat
gource --output-custom-log gource_2025/qtribu.txt qtribu
gource --output-custom-log gource_2025/site-contribuer.txt site-contribuer
gource --output-custom-log gource_2025/slides.txt slides
gource --output-custom-log gource_2025/website.txt website
```

Puis créer un fichier de log qui les incorpore tous :

```sh
cd gource_2025
cat english-blog.txt infra.txt qchat.txt qtribu.txt site-contribuer.txt slides.txt website.txt | sort -n > 2025_combined.txt
```

Enfin même commande pour lancer la génération gource !

```sh
gource --load-config gource.ini -o - gource_2025/2025_combined.txt | ffmpeg -y -r 25 -f image2pipe -vcodec ppm -i - -vcodec libx265 -preset fast -pix_fmt yuv420p -crf 26 -threads 0 -bf 0 ./geotribu_history.mp4
```

----

## Ressources

- le [projet GitHub de Gource](https://github.com/acaudwell/Gource)
- le [projet Github  avec les fichiers de configuration](https://github.com/geotribu/tooling-gource) pour Geotribu
- la [page Gource sur la doc francophone d'Ubuntu](https://doc.ubuntu-fr.org/gource)
