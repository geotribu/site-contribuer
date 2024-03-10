---
title: "Rechercher des images"
subtitle: "Trouver la bonne image ou le bon logo"
authors:
    - Julien MOURA
categories:
    - article
    - meta
comments: true
date: 2022-11-26 14:20
description: "Guide pour rechercher dans l'entrepôt d'images de Geotribu (https://cdn.geotribu.fr/), via l'interface graphique de TinyFileManager ou bien via le CLI Geotribu"
icon: material/image-search
image: https://cdn.geotribu.fr/img/internal/contribution/embed_image/geotribu_cdn_tinyfilemanager_search.png
robots: index, follow
tags:
    - cdn
    - CLI
    - images
    - script
    - Python
---

# Rechercher des images dans le CDN de Geotribu

## Parcourir le CDN via l'interface web

L'accès en lecture à [notre entrepôt d'images](./cdn-images-hebergement.md) accumulées depuis toutes ces années est ouvert :gift_heart: et c'est même un passage recommandé pour tout contributeur/rice :

- adresse : <https://cdn.geotribu.fr>
- identifiant : `invité`
- mot de passe : `geotribu_bemyguest2020`

En plus de permettre un petit voyage dans le temps, autant que toutes ces ressources servent en plus de notre site :smiley:. Merci de ne pas en abuser en respectant le _fair-use_. Pensez également à créditer les auteur/es.

### Chercher une image

#### Filtrer le dossier courant

La barre de recherche en haut permet de filtrer sur le nom du fichier parmi ceux du répertoire courant. A noter qu'il faut attendre que l'ensemble des fichiers du répertoire soient listés pour que ce la fonctionne.

![Filtrer le dossier courant dans TinyFileManager](https://cdn.geotribu.fr/img/internal/contribution/embed_image/geotribu_cdn_tinyfilemanager_filter.png){: .img-center loading=lazy }

#### Recherche avancée

Il est également possible d'effectuer une recherche dans l'arborescence en cliquant sur le menu descendant à droite de la barre de filtre et de sélectionner "Recherche avancée" :

![Recherche avancée dans TinyFileManager](https://cdn.geotribu.fr/img/internal/contribution/embed_image/geotribu_cdn_tinyfilemanager_search.png){: .img-center loading=lazy }

----

## Faire une recherche avec le CLI Geotribu

![logo Python](https://cdn.geotribu.fr/img/logos-icones/programmation/python.png "logo Python"){: .img-thumbnail-left }

Si l'interface graphique vous semble trop longue ou la recherche assez fine ou que vous n'avez pas les identifiants sous la main, il est possible d'interroger [l'index des images stockées](https://cdn.geotribu.fr/img/search-index.json) généré avec [lunr.py] toutes les heures et qui est accessible librement.

En quelques mots :

```sh
pip install geotribu
geotribu img search satellite
[...]
geotribu img search --filter logo "name:qgis"
```

Pour aller plus loin, consulter :

- [l'article d'introduction]({{ config.extra.geotribu_main_site }}articles/2023/2023-08-25_geotribu-cli-en-ligne-de-commande/)
- la [documentation du CLI](https://cli.geotribu.fr/), en particulier la [page des exemples consacrés à la recherche d'images](https://cli.geotribu.fr/usage/examples.html#rechercher-une-image)
