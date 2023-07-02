---
title: "GOurce : mettre l'historique de contributions en vidéo"
categories:
    - article
    - Geotribu
date: 2023-07-03 20:20
description:
icon : simple/interactiondesignfoundation
image:
tags:
    - coulisses
    - GéoCapot
    - Gource
    - historique
    - valorisation
---


# Créer une data-visualisation des contributions à Geotribu avec Gource

## La check-list de sécurité

- [ ] vérifier que tous les contributeurs ont bien un seul nom (via le fichier `.mailmap`)

## Show time!

![icône globe video](https://cdn.geotribu.fr/img/internal/icons-rdp-news/animation_video.png "icône globe video"){: .img-rdp-news-thumb }

```sh
gource --camera-mode track --logo ../site-contribuer/content/theme/assets/images/geotribu/geotribu_logo_tipi_seul_carre.png --title "Site Geotribu - 2022" --date-format "%e %B" --seconds-per-day 1 --auto-skip-seconds 1 --key --start-date 2022-01-01 --stop-date 2022-12-31 --hide mouse -f -1280x720 -o - | ffmpeg -y -r 60 -f image2pipe -vcodec ppm -i - -vcodec libx264 -preset ultrafast -crf 1 -threads 0 -bf 0 gource.mp4
```

----

## Ressources

- le [dépôt GitHub de Gource](https://github.com/acaudwell/Gource)
