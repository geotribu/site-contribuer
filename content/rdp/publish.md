---
title: "Publier et diffuser une revue de presse"
subtitle: "Merger > attendre > poster sur les réseaux sociaux"
authors:
    - Geotribu
categories:
    - contribution
comments: true
date: 2021-09-30
description: "Publication et diffusion d'une revue de presse de Geotribu (GeoRDP)."
image: "https://cdn.geotribu.fr/img/articles-blog-rdp/collaboration_world.png"
license: default
tags:
    - guide
    - GeoRDP
    - workflow
---

# Publication et diffusion d'une revue de presse

![icône porte-voix](https://cdn.geotribu.fr/img/internal/icons-rdp-news/journalisme.png "icône porte-voix"){: .img-thumbnail-left }

Une fois la Pull Request validée par un membre de l'équipe, la branche de la revue de presse est fusionnée (*merged*) dans la branche principale, déclenchant la génération et le déploiement du site web.

A noter que la branche ayant servi à la revue de presse est supprimée, ainsi que le site de prévisualisation généré.

## Diffusion automatique

Une fois la revue de presse en ligne, elle est automatiquement :

- intégrée au [flux RSS]({{ config.extra.geotribu_main_site }}feed_rss_created.xml)
- ajoutée à [la newsletter]({{ config.extra.geotribu_main_site }}newsletter/signup/).
- mise en avant sur la page d'accueil du site
- référencée dans les moteurs de recherche (via les *sitemaps* notamment)

## Diffusion sur les réseaux sociaux

Il est alors temps de lancer la diffusion sur les réseaux sociaux pour relayer la publication :

- sur LinkedIn, via [la page officielle de Geotribu](https://www.linkedin.com/company/geotribu/), avec [les hashtags `#Geotribu`](https://www.linkedin.com/feed/hashtag/?keywords=geotribu) et [`#GeoRDP`](https://www.linkedin.com/feed/hashtag/?keywords=geordp)
- Sur Mastodon, via [le compte officiel de Geotribu](https://mapstodon.space/@geotribu/)
- sur Twitter, via [le compte officiel de Geotribu](https://twitter.com/geotribu/)

### Exemple de structure de message

```txt
🗞 La #GeoRDP est en ligne :


👥 Contributeur/ices : @XXXX


🌍 #Geotribu #veille #géomatique #YYYY @ZZZZ
```
