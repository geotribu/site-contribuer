---
title: Définir l'icône d'un article
subtitle: Rendre son article iconique
authors:
    - Julien Moura
categories:
    - article
    - contribution
    - tutoriel
date: 2024-01-17
description: "Guide de contribution à Geotribu : comment définir l'icône d'un article pour le menu de navigation et la 'social card'."
icon: fontawesome/solid/icons
image:
tags:
    - icône
    - Markdown
    - tutoriel
---

# Ajouter une icône à son article

Comble du _fancy_, il est possible d'ajouter une icône à son article pour personnaliser la façon dont il apparaît dans la barre de navigation de gauche :

![Icônes des articles dans la nvigation de gauche de Geotribu](https://cdn.geotribu.fr/img/internal/contribution/icone_pages/icones_pages_navigation_gauche.webp){: .img-center loading=lazy }

Cette icône est également utilisée pour agrémenter l'image de mise en avant de l'article quand elle est générée automatiquement. Par exemple sur [l'article DuckDB de fin 2023]({{ config.extra.geotribu_main_site }}articles/2023/2023-12-19_duckdb-donnees-spatiales/) :

![]({{ config.extra.geotribu_main_site }}assets/images/social/articles/2023/2023-12-19_duckdb-donnees-spatiales.png){: .img-center loading=lazy }

## Procédure

1. Se rendre sur la doc du thème pour y rechercher l'icône de son coeur (cliquer l'icône `(+)` pour ouvrir le moteur de recherche ) :

    [:material-form-dropdown: Moteur de recherche des icônes sur la doc du thème Material](https://squidfunk.github.io/mkdocs-material/reference/#setting-the-page-icon){: .md-button}

    ![Material for Mkdocs - Moteur de recherche des icônes](https://cdn.geotribu.fr/img/internal/contribution/icone_pages/chercher_icone_pages.webp){: .img-center loading=lazy }

1. Cliquer dessus, ça copie le code de l'icône
1. Coller dans l'en-tête du fichier :

    ```yaml
    ---
    [...]
    icon: simple/qgis
    [...]
    ---

    # Titre de la page

    ...
    ```
