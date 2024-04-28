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

Cette page décrit comment on peut utiliser Gource pour générer des vidéos rétrospectives sur les contributions au site (voir [un exemple sur la rétrospective 2022]({{ config.extra.geotribu_main_site }}articles/2023/2023-01-30_voeux-geotribu-2023/#retrospective-2022)). Ou celle portant sur l'année 2023 :

<iframe width="100%" height="400" src="https://www.youtube.com/embed/cHQzkNkLeW8?si=Xwck99pK8GVk2Vy3" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

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
git shortlog -nse --since="01 Jan 2023" --before="31 Dec 2023"
```

Ce qui donne par exemple :

```sh
764  Julien Moura <dev@ingeoveritas.com>
233  Florian Boret <florian.boret@data-wax.com>
122  Geotribot <geotribu+bot@gmail.com>
50  Guilhem Allaman <dev@guilhemallaman.net>
45  Nicolas David <nicolas.david@ign.fr>
36  Florent Fougères <florent.fougeres@gmail.com>
28  Delphine Montagne <delphine.montagne@univ-pau.fr>
27  Quy Thy Truong <quythy.truong@oslandia.com>
20  Aurélien Chaumet <aurelienchaumet17@gmail.com>
14  Mathilde Ferrey <45847618+mferrey@users.noreply.github.com>
6  Gabriel Poujol <gpoujol@openig.org>
5  Arnaud Vandecasteele <arnaud.sig@gmail.com>
5  Maël Reboux <m.reboux@rennesmetropole.fr>
3  Jérémy Garniaux <jeremy@mapper.fr>
2  Jérémie Hanke <70693383+sigps@users.noreply.github.com>
2  Loïc Bartoletti <loic.bartoletti@oslandia.com>
2  Michaël Galien <michael.galien@gard.fr>
2  Yann Chambon <126060687+yannchambon@users.noreply.github.com>
1  Benoît Blanc <benoitblanc@live.com>
1  Christian Quest <github@cquest.org>
1  Jérémie Prud'homme <p.jeremie@gmail.com>
1  Olivia Guyot <olivia.guyot@camptocamp.com>
1  Pierre-François Blin <pierrefrancois.blin@gmail.com>
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
cp tooling-gource/background_sombre_dalle ~/Git/Geotribu/website/
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
gource --load-config gource.ini -o - | ffmpeg -y -r 60 -f image2pipe -vcodec ppm -i - -vcodec libx264 -preset fast -pix_fmt yuv420p -crf 1 -threads 0 -bf 0 ~/Videos/geotribu_history.mp4
```

Décortiquons :

1. `--load-config gource.ini`: on charge les paramètres de configuration.
1. `-o -`: cela indique à Gource de rediriger la sortie vers la sortie standard (_stdout_) au lieu d'écrire directement dans un fichier. La sortie sera ensuite utilisée comme entrée pour la commande suivante, ici `ffmepg`.
1. `|`: C'est le symbole de tube (_pipe_) qui prend la sortie de la première commande (`gource`) et la transmet à la deuxième commande (`ffmpeg`).
1. `ffmpeg`: c'est la bibliothèque de traitement multimédia qu'on utilise pour encoder la vidéo dans un fichier
1. `-y`: écraser automatiquement les fichiers de sortie existants sans poser de questions.
1. `-r 60`: débit d'images de sortie (60 images par seconde dans ce cas).
1. `-f image2pipe`: on indique le format d'entrée, qui est une série d'images en flux (_image2pipe_).
1. `-vcodec ppm`: codec vidéo utilisé pour le flux d'entrée, qui est PPM (_Portable Pixel Map_)
1. `-i -`: on indique à `ffmpeg` de prendre l'entrée depuis la sortie standard (_stdout_) de la commande précédente (`gource`).
1. `-vcodec libx264`: codec vidéo de sortie, qui est libx264, un codec vidéo largement utilisé pour la compression vidéo.
1. `-preset fast`: préréglage de vitesse de la compression vidéo, en choisissant "ultrafast" pour maximiser la vitesse au détriment de la taille du fichier.
1. `-pix_fmt yuv420p`: format de pixel de sortie pour la vidéo.
1. `-crf 1`: facteur de qualité Constant Rate Factor (CRF) pour la vidéo. Une valeur faible (comme 1) entraînera une meilleure qualité mais un fichier plus volumineux.
1. `-threads 0`: on indique à `ffmpeg` d'utiliser autant de threads que possible pour l'encodage.
1. `-bf 0`: on désactive l'utilisation de cadres B (B-frames) pour la compression vidéo.
1. `geotribu_history.mp4`: fichier de sortie qui contiendra la vidéo finale.

----

## Ressources

- le [projet GitHub de Gource](https://github.com/acaudwell/Gource)
- le [projet Github  avec les fichiers de configuration](https://github.com/geotribu/tooling-gource) pour Geotribu
- la [page Gource sur la doc francophone d'Ubuntu](https://doc.ubuntu-fr.org/gource)
