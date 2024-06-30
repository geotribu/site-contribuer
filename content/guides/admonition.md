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

Par "encart" on fait référence à une balise qui ressemble par exemple à cela :

!!! warning "attention il y a un éléphant rose dans le ciel :elephant::purple_heart: !!"
    Nan nan, c'est pas vrai

Utiles pour ajouter des infos supplémentaires, des avertissement ou pour plein d'autres raisons permettant d'alléger et marquer des pauses dans la rédaction, ces encarts peuvent être de plusieurs nature et se déclarent comme ceci :

<!-- markdownlint-disable MD046 -->
=== "Rendu"

    !!! tip "ceci est un encart de type 'tip' hihi"
        texte de l'encart

=== "Markdown"

    ```markdown
    !!! tip "ceci est un encart de type 'tip' hihi"
        texte de l'encart
    ```
<!-- markdownlint-enable MD046 -->

Après les trois points d'exclamation vient le type de l'encart, dont [les valeurs possibles sont listés ici](https://squidfunk.github.io/mkdocs-material/reference/admonitions/#supported-types) et parmi lesquelles :

- warning
- note
- abstract
- tip
- quote
- question
...etc

## Personnaliser le titre de l'encart

Il est possible de mettre son propre texte entre guillemets pour donner un titre à l'encart. Un double guillemet vide donnera un encart sans titre :

!!! question ""
    Ceci est un encart de type question et sans titre (donc sans question)... Bah je sais pas moi... 12 ?

## Rendre l'encart dépliable

Pour un encart dépliable, c'est trois points d'interrogation qu'on utilisera, avec un `+` pour dire si l'encart est déplié par défaut :

<!-- markdownlint-disable MD046 -->
=== "Rendu"

    ??? question "Quelle est la différence entre un lapin :rabbit: et un pneu ?"
        Ils sont tous les deux en caoutchouc, sauf le lapin !

=== "Markdown"

    ```markdown
    ??? question "Quelle est la différence entre un lapin :rabbit: et un pneu ?"
        Ils sont tous les deux en caoutchouc, sauf le lapin !
    ```
<!-- markdownlint-enable MD046 -->
