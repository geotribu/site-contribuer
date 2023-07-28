---
title: "Gource : l'historique des contributions en vidéo"
categories:
    - article
    - Geotribu
date: 2023-07-29 20:20
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

![logo Gource](https://cdn.geotribu.fr/img/logos-icones/logiciels_librairies/gource.webp){: .img-rdp-news-thumb }

Le(s) site(s) Geotribu étant basés sur Git, on profite ainsi de l'outillage de l'écosystème ; GitHub bien sûr mais aussi d'autres outils moins connus.

Cette page décrit comment on peut utiliser Gource pour générer des vidéos rétrospectives sur les contributions au site (voir [un exemple sur la rétrospective 2022]({{ config.extra.geotribu_main_site }}articles/2023/2023-01-30_voeux-geotribu-2023/#retrospective-2022)).

<iframe width="100%" height="400" src="https://www.youtube-nocookie.com/embed/mbDAz9aAVW8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

C'est un outil qui est connu et largement utilisé dans différents projets (voir [ici l'exemple de MapServer]({{ config.extra.geotribu_main_site }}rdp/2020/rdp_2020-05-15/?h=gource#mapserver-760)). Dans la communauté géomatique, on a régulièrement la vidéo retraçant les contributions à OpenStreetMap sur une fenêtre spatio-temporelle. A titre d'expérience personnelle, je l'avais utilisé pour retracer le tavail de développement sur le plugin QGIS d'Isogeo : <https://www.youtube.com/watch?v=URoH0osLY_4>.

## Prérequis

- [ ] avoir un ordinateur qui dispose d'une bonne carte graphique
- [ ] avoir cloné la dernière version du dépôt du site web
- [ ] vérifier que tous les contributeurs ont bien un seul nom (via le fichier `.mailmap`)
- [ ] avoir du temps devant soi (ou un double écran)

----

## Installation

!!! note
    L'installation sur Windows est plutôt facile de mémoire (enfin celle de ffmpeg semble un peu exotique), donc je n'en parle pas ici.

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

### Fichier `gource.ini`

```ini
[display]
fullscreen=true
output-framerate=60
output-ppm-stream=./geotribu_website_git_history.ppm
viewport=1280x720

[gource]
auto-skip-seconds=10
date-format=%d %B %Y
disable-progress=true
frameless=true
hide=mouse
highlight-dirs=true
highlight-users=true
key=true
# logo="https://cdn.geotribu.fr/img/internal/charte/geotribu_logo_64x64.png"
path=.
seconds-per-day=0.2
stop-at-end=true
title=Contributions au contenu de Geotribu
```

### Options de compression vidéo de `ffmpeg`

----

## Show time !

![icône globe video](https://cdn.geotribu.fr/img/internal/icons-rdp-news/animation_video.png "icône globe video"){: .img-rdp-news-thumb }

Allez, on a tout ce qu'il faut, c'est parti pour produire la vidéo ! Concrètement, on lance la commande qui va lancer l'animation en plein écran, filmer l'écran et encoder/compresser le tout dans un fichier mp4.

```sh
gource --camera-mode track --logo ../site-contribuer/content/theme/assets/images/geotribu/geotribu_logo_tipi_seul_carre.png --title "Site Geotribu - 2022" --date-format "%e %B" --seconds-per-day 1 --auto-skip-seconds 1 --key --start-date 2022-01-01 --stop-date 2022-12-31 --hide mouse -f -1280x720 -o - | ffmpeg -y -r 60 -f image2pipe -vcodec ppm -i - -vcodec libx264 -preset ultrafast -crf 1 -threads 0 -bf 0 gource.mp4
```

```sh
gource --load-config gource.ini --start-date '2020-06-01' -1280x720 -o - | ffmpeg -y -r 60 -f image2pipe -vcodec ppm -i - -vcodec libx264 -preset ultrafast -pix_fmt yuv420p -crf 1 -threads 0 -bf 0 geotribu_history.mp4
```

----

## Ressources

- le [dépôt GitHub de Gource](https://github.com/acaudwell/Gource)
- la [page Gource sur la doc francophone d'Ubuntu](https://doc.ubuntu-fr.org/gource)
