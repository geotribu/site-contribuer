---
title: Intégrer un post Mastodon
categories:
    - article
    - contribution
    - tutoriel
comments: true
date: 2023-10-03 10:20
description: "Guide de contribution à Geotribu : comment intégrer un post Mastodon dans un contenu en Markdown."
icon: material/mastodon
tags:
    - guide
    - intégration
    - Markdown
    - Mastodon
---

# Intégrer un post Mastodon

Contrairement à [l'intégration d'un tweet](./twitter.md), celle d'un post Mastodon est basique (iframe) et simple (clic clic).

## Pas à pas

Prenons ce post Mastodon pour exemple : <https://mapstodon.space/@jeremy/109378742289307030>

1. En bas à droite du post, cliquer sur les `...` et sélectionner `Intégrer` :

    ![Menu Intégrer le post](https://cdn.geotribu.fr/img/internal/contribution/mastodon/mastodon_embed_post_menu.webp){: .img-center loading=lazy }

1. Un petit widget s'ouvre et propose tout simplement de copier le code d'intégration :

    ![Widget intégration de post Mastodon](https://cdn.geotribu.fr/img/internal/contribution/mastodon/mastodon_embed_post_widget.webp){: .img-center loading=lazy }

1. Coller le code dans votre contenu en Markdown
1. Supprimer la partie qui référence le script d'intégration JavaScript (exemple : `<script src="https://mapstodon.space/embed.js" async="async"></script>`) car le site l'intègre déjà.
1. Optionnellement, il est possible de jouer sur l'attribut `width` si vous pensez que le contenu du post en vaut le coup. Ainsi pour notre exemple, j'indique la largeur 600 pixels :

    <!-- markdownlint-disable MD046 -->
    === "Markdown"

        ```markdown
        <iframe src="https://mapstodon.space/@jeremy/109378742289307030/embed" class="mastodon-embed" style="max-width: 100%; border: 0" width="600" allowfullscreen="allowfullscreen"></iframe>
        {: align=middle }
        ```

    === "Rendu"

        <iframe src="https://mapstodon.space/@jeremy/109378742289307030/embed" class="mastodon-embed" style="max-width: 100%; border: 0" width="600" allowfullscreen="allowfullscreen"></iframe>
        {: align=middle }
    <!-- markdownlint-enable MD046 -->

:bellhop_bell: Voilà, c'est prêt ! :tada:
