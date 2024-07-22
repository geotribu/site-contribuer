---
title: Référencement et monitoring qualité
subtitle: Premiers sur Lycos
categories:
    - article
    - Geotribu
comments: true
date: 2024-07-22
description: "Sous le GéoCapot : outils utilisés pour monitorer le référencement et la qualité des sites de Geotribu."
icon : fontawesome/brands/searchengin
tags:
    - Bing Webmasters
    - coulisses
    - GéoCapot
    - Google Search Console
    - monitoring
    - référencement
    - SEO
---

# Gestion du référencement et outils de validation du site

## Google Search Console

![logo Google](https://cdn.geotribu.fr/img/logos-icones/entreprises_association/google/google.webp){: .img-thumbnail-left }

Le moteur de recherche le plus utilisé en Europe et en France propose une console web pour référencer et suivre le référencement de sites webs dans le moteur. Les sites Geotribu sont ajoutés via le nom de domaine (tout ce qui est sous geotribu.fr) et liés au compte Google Geotribu.

[:simple-googlesearchconsole: Accéder à la Google Search Console](https://search.google.com/search-console/){: .md-button }
{: align=middle }

Parmi les différents menus et outils, _Insights_ est celui qui est le plus mis en avant et développé par Google. Son but est de fournir en une seule page quelques métriques de base sur le référencement d'un ou plusieurs sites :

![Google Search Console - Insight 1](https://cdn.geotribu.fr/img/internal/contribution/seo_monitoring/search_console_insight_00.webp){: loading=lazy width=45% }
![Google Search Console - Insight 2](https://cdn.geotribu.fr/img/internal/contribution/seo_monitoring/search_console_insight_01.webp){: loading=lazy width=45% }
{: align=middle }

C'est aussi dans la Search Console que Google nous avertit s'il y a une erreur +/- bloquante de structure dans les contenus publiés. L'une des informations les plus importantes est le relevé des codes 404 :

![Google Search Console - Toutes les erreurs 404](https://cdn.geotribu.fr/img/internal/contribution/seo_monitoring/search_console_404.webp){: loading=lazy width=30% }
![Google Search Console - Filtrer les pages 404 par sitemap](https://cdn.geotribu.fr/img/internal/contribution/seo_monitoring/search_console_404_filtre.webp){: loading=lazy width=30% }
![Google Search Console - Pages 404 de https://geotribu.fr](https://cdn.geotribu.fr/img/internal/contribution/seo_monitoring/search_console_404_pages-sitemap.webp){: loading=lazy width=30% }
{: align=middle }

Pour gérer les erreurs 404, souvent liées à des pages renommées/déplacées/supprimées, on utilise [le plugin mkdocs-redirects](https://github.com/mkdocs/mkdocs-redirects) via [un fichier de configuration](https://github.com/geotribu/website/blob/master/config/plugins_redirections.yml), principalement alimenté pendant [le travail de nettoyage et de correspondance des anciens contenus](https://github.com/geotribu/website/issues/443).

On y trouve aussi les informations liées aux [extraits enrichis, métadonnées structurées](seo_extraits_enrichis.md) et aux [cartes de partage](social_cards.md) :

![Google Search Console - Erreurs dans les données structurées](https://cdn.geotribu.fr/img/internal/contribution/seo_monitoring/search_console_rich-snippets_erreurs.png){: .img-center loading=lazy }

Il est également possible d' "inspecter" manuellement une page en entrant son URL dans la barre de recherche en haut pour vérifier l'état de son indexation et obtenir ses caractéristiques au regard de l'index de Google :

![Google Search Console - Inspection d'une page](https://cdn.geotribu.fr/img/internal/contribution/seo_monitoring/search_console_inspection_page.png){: .img-center loading=lazy }

!!! note
    La Search Console est connectée à la propriété Google Analytics, utilisé pour mesurer la fréquentation des sites Geotribu. Voir [la page consacrée à la confidentialité]({{ config.extra.geotribu_main_site }}about/confidentialite/).

----

## Bing Webmasters Tools

![logo Bing](https://cdn.geotribu.fr/img/logos-icones/entreprises_association/bing.webp){: .img-thumbnail-left }

Beaucoup moins utilisé que Google, le moteur de recherche de Microsoft n'en reste pas moins intéressant vu que son éditeur en fait le point d'entrée de certains de ses services (surtout depuis l'IA) et qu'il est utilisé par d'autres moteurs comme Qwant ou DuckDuckGo.
Par ailleurs, l'outillage est différent et assez complémentaire de la Google Search Console, tout en permettant d'en utiliser le compte pour identifier les sites et domaines à gérer.

[:material-microsoft-bing: Accéder à la console Bing Webmasters Tools](https://www.bing.com/webmasters/searchperf?siteUrl=https://geotribu.fr/){: .md-button }
{: align=middle }

Globalement, c'est le même principe que sur Google. La page d'accueil donne une idée des performances des contenus dans les recherches effectuées sur le moteur maison :

![Bing Webmasters Tools - Page d'accueil sur les performances](https://cdn.geotribu.fr/img/internal/contribution/seo_monitoring/bing_webmasters_tools_perfs.webp){: .img-center loading=lazy }

On peut aussi "inspecter" une URL en particulier avec la mise en avant d'éléments un peu différents :

![Bing Webmasters Tools - PInspection d'une URL](https://cdn.geotribu.fr/img/internal/contribution/seo_monitoring/bing_webmasters_tools_page-erreur-titre-trop-long.webp){: .img-center loading=lazy }
