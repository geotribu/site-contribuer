---
title: Ajouter une nouvelle année
categories:
    - Geotribu
date: 2024-01-15
description:
icon : material/firework
image:
tags:
    - coulisses
    - GéoCapot
---

#

le site étant statique et les contenus organisés par années

## Procédure

1. Créer un sous-dossier avec l'année dans le dossier `content/articles`
1. Y copier un fichier `.pages` depuis l'année précédente et ajouter l'année  Exemple pour 2024 :
1. Y copier un fichier `.meta.yml` depuis l'année précédente et incrémenter le boost de `1`. En fait, la valeur du boost doit correspondre aux 2 derniers chiffres de l'année. Exemple pour 2024 :

    ```yml title="content/articles/2024/.meta.yml"
    search:
        boost: 24
    ```

1. Ajouter l'année en haut de la liste `nav`. du fichier `.pages` à la racine du dossier `content/articles` :

    ```yml title="content/articles/.pages"
    title: "&#128214; Articles"

    nav:
        - "2024"
        - "2023"
        - "2022"
        - "2021"
        - "2020"
        - "2015"
        - "2014"
        - "2013"
        - "2012"
        - "2011"
        - "2010"
        - "2009"
        - "2008"

    ```

1. Reproduire pour le dossier `rdp`
1. créer une Pull Request
