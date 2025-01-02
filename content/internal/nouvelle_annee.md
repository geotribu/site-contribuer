---
title: Routine de nouvelle année
subtitle: "Prérequis : gueule de bois et cotillons"
categories:
    - Geotribu
date: 2024-12-31
description: Documentation de la routine de nouvelle année pour les contenus Geotribu.
icon : material/firework
image:
tags:
    - coulisses
    - GéoCapot
---

# Entamer une nouvelle année de contenus sur Geotribu

Le site étant statique et les contenus organisés par années, le premier article ou la première revue de presse de l'année nécessite quelques opérations. Rien de bien compliqué mais vu qu'on ne le fait qu'une fois par an, ça vaut le coup de documenter la procédure.

## Procédure

1. Créer un sous-dossier avec l'année dans [le dossier `content/articles`](https://github.com/geotribu/website/tree/master/content/articles)
1. Y copier un fichier `.pages` depuis l'année précédente et ajouter la nouvelle année. Exemple pour 2025 :

    ```yaml title="content/articles/2025/.pages"
    order: desc
    ```

1. Y copier un fichier `.meta.yml` depuis l'année précédente et incrémenter le boost de `1`. En fait, la valeur du boost doit correspondre aux 2 derniers chiffres de l'année. Exemple pour 2025 :

    ```yaml title="content/articles/2025/.meta.yml"
    search:
        boost: 25
    ```

1. Ajouter l'année en haut de la liste `nav`. du fichier `.pages` à la racine du dossier `content/articles` :

    ```yaml title="content/articles/.pages"
    title: "&#128214; Articles"

    nav:
        - "2025"
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

1. Reproduire pour [le dossier `rdp`](https://github.com/geotribu/website/tree/master/content/rdp)
1. Créer une Pull Request dédiée à ces changements
