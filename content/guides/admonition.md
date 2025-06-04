---
title: Intégrer un encart
subtitle : On s'encarte !
categories:
    - article
    - contribution
    - tutoriel
comments: true
date: 2024-06-29
description: "Guide de contribution à Geotribu : comment intégrer un encart pour info, ou avertissement, ou autre"
icon: octicons/info-16
license: default
tags:
    - admonition
    - info
    - intégration
    - markdown
    - tutoriel
    - warning
---

# Intégrer un encart

Cette page détaille comment intégrer et customiser un encart (["admonition"](https://squidfunk.github.io/mkdocs-material/reference/admonitions/) dans la langue de Beyoncé) : syntaxes, icônes, titres ...etchétera etchétera

!!! info
    À noter que [GitHub](https://docs.github.com/fr/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#alerts) et [HedgeDoc](https://docs.hedgedoc.dev/references/hfm/#structural-elements) nomment cela des "alertes", nous parlerons ici d'"encarts".

Par "encart" on fait référence à une balise qui ressemble par exemple à cela :

!!! warning "attention il y a un éléphant rose dans le ciel :elephant::purple_heart: !!"
    Nan nan, c'est pas vrai

Utiles pour ajouter des infos supplémentaires, des avertissement ou pour plein d'autres raisons permettant d'alléger et marquer des pauses dans la rédaction, ces encarts peuvent être de plusieurs nature et se déclarent comme ceci :

<!-- markdownlint-disable MD046 -->
=== "Markdown"

    ```markdown
    !!! tip "ceci est un encart de type 'tip' hihi"
        texte de l'encart
    ```

=== "Rendu"

    !!! tip "ceci est un encart de type 'tip' hihi"
        texte de l'encart
<!-- markdownlint-enable MD046 -->

Après les trois points d'exclamation vient le type de l'encart. Les valeurs possibles [sont listées ici](https://squidfunk.github.io/mkdocs-material/reference/admonitions/#supported-types).

## Personnaliser le titre de l'encart

Il est possible de mettre son propre texte entre guillemets pour donner un titre à l'encart. Un double guillemet vide donnera un encart sans titre :

!!! question ""
    Ceci est un encart de type question et sans titre (donc sans question)... Bah je sais pas moi... 12 ?

## Rendre l'encart dépliable

Pour un encart dépliable, c'est trois points d'interrogation qu'on utilisera, avec un `+` pour dire si l'encart est déplié par défaut :

<!-- markdownlint-disable MD046 -->
=== "Markdown"

    ```markdown
    ??? question "Quelle est la différence entre un lapin :rabbit: et un pneu ?"
        Ils sont tous les deux en caoutchouc, sauf le lapin !
    ```

=== "Rendu"

    ??? question "Quelle est la différence entre un lapin :rabbit: et un pneu ?"
        Ils sont tous les deux en caoutchouc, sauf le lapin !
<!-- markdownlint-enable MD046 -->

## Alignement de l'encart

Pour aligner un encart à droite, on utilise les mots-clés `inline end`, comme ceci :

<!-- markdownlint-disable MD046 -->
=== "Markdown"

    ```markdown
    !!! info inline end "Lorem ipsum"
        Lorem ipsum dolor sit amet, et patati et patata
    ```

=== "Rendu"

    !!! info inline end "Lorem ipsum"
        Lorem ipsum dolor sit amet, et patati et patata
<!-- markdownlint-enable MD046 -->

Pour aligner l'encart à gauche, ce sera seulement le mot-clé `inline`.

!!! info "placement de l'encart"
    Un encart aligné à droite ou à gauche doit être placé avant le bloc avec lequel il s'aligne.

## Gérer les encarts multilignes et indentés comme un pro

La syntaxe des encarts peut entrer en conflit avec le [vérificateur du Markdown qui génère alors des faux positifs](../internal/markdown_linter.md#gérer-les-faux-positifs-du-linter "Gérer les faux positifs du linter Markdown"), notamment dans 2 cas de figure assez communs :

- les encarts contenant un texte à plusieurs lignes ;
- les encarts indentés par exemple dans [les onglets](https://squidfunk.github.io/mkdocs-material/reference/content-tabs/).

Il faut alors encadrer l'encart (j'étais obligé de faire cette formulation, c'est contractuel) avec les balises désactivant la règle 46 du linter. Voilà ce que cela donne :

<!-- markdownlint-disable MD046 -->
=== "Markdown"

    ```markdown
    <!-- markdownlint-disable MD046 -->
    !!! example "Je, je suis multiligne"
        Je suis une catin
        Je, je suis si fragile
        Qu'on me tienne la main

        Fendre la lune, baisers d'épine et de plume
        Bercée par un petit vent, je déambule
        La vie est triste comme un verre de grenadine
        Aimer c'est pleurer quand on s'incline
    <!-- markdownlint-enable MD046 -->
    ```

=== "Rendu"
<!-- markdownlint-disable MD046 -->
    !!! example "Je, je suis multiligne"
        Je suis une catin
        Je, je suis si fragile
        Qu'on me tienne la main

        Fendre la lune, baisers d'épine et de plume
        Bercée par un petit vent, je déambule
        La vie est triste comme un verre de grenadine
        Aimer c'est pleurer quand on s'incline
<!-- markdownlint-enable MD046 -->
