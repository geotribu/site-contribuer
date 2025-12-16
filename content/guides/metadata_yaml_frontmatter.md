---
title: "Rédiger en Markdown : comprendre l'en-tête"
subtitle: "YAML front-matter"
categories:
    - article
    - contribution
    - tutoriel
comments: true
date: 2021-01-05
description: "Rédiger en Markdown : de l'importance de l'en-tête (YAML front-matter) pour définir les métadonnées, la navigation et le référencement."
icon: material/page-layout-header
image: "https://cdn.geotribu.fr/img/internal/contribution/markdown/markdown_yaml_frontmatter.png"
tags:
    - Markdown
    - rédaction
    - SEO
    - YAML
---

# L'en-tête des contenus (*YAML frontmatter*)

Les sites statiques basés sur du contenu en Markdown (ou autre syntaxe *flat* comme rst ou autre) ont recours à un en-tête en YAML, appelé aussi *YAML front-matter*, qui contient des métadonnées sur la page voire carrément des instructions spécifiques à son rendu (template à utiliser, activation/désactivation de fonctionnalités...).

Le site Geotribu tire également profit de ce mécanisme.

## Syntaxe

L'en-tête est défini en haut de la page par un ensemble de clés/valeurs encadré par `---`. Chaque élément a un rôle ou une réutilisation :

- `title` : utilisé dans le menu de navigation de gauche, le RSS et le SEO (référencement). Cela autorise par exemple un titre différent que celui affiché dans l'article
- `subtitle` : texte court affiché sous le titre dans le menu de navigation de gauche. Idéal pour indiquer qu'il s'agit d'un article au sein d'une série, ajouter un jeu de mots, etc.
- `authors` : liste des contributeurs réutilisée dans les meta-tags de la page et le SEO (via schema.org)
- `categories` : contient la typologie du contenu permettant des comportements adaptés. Utilisé pour classer les contenus, définir le schéma JSON-LD à utiliser et dans le flux RSS et la newsletter.
- `date` : date de création publique de l'article, correspondant à la date de première publication. Format : `AAAA-MM-JJ HH:mm`.
- `description` : texte de 155 caractères maximum (enfin, plus exacteent, le reste est tronqué) qui résume le contenu et qui est utilisé par les moteurs d'indexation et de recherche (SEO, Google, Bing...), l'index de recherche interne du site, le flux RSS et la newsletter. Voir [le guide de Google sur les descriptions de contenus web](https://developers.google.com/search/docs/appearance/snippet?hl=fr#meta-descriptions).
- `image` : lien vers l'image qui s'affiche sur la page d'accueil du site, lors du partage du contenu dans le flux RSS, la newsletter, les réseaux sociaux : [voir la page Cartes de partage](../internal/social_cards.md "Outillage interne : les Social Cards"). Format : PNG ou JPG. Dimensions : entre 600x300 et 800x400. Ratio: viser 2/1 ou proche. Laisser vide pour une image générée automatiquement (voir [ci-dessous](#image-illustration-contenu)).
- `legacy` : stocke des informations relatives aux anciens sites Geotribu, notamment pour assurer la continuité. Uniquement pour un **usage interne** et pour les contenus créés avant 2020.
- `license` : détermine si la licence du contenu est celle par défaut (`license: default`) ou non (`license: none`). Si la clé n'est pas renseignée, c'est la licence par défaut qui s'applique. Voir le guide [Choisir sa licence](./licensing.md).
- `robots` : détermine si le contenu doit être indexé par les moteurs de recherche ou non. Par défaut: `index, follow`.
- `tags` : liste dans l'ordre alphabétique des mots-clés qui permet le [classement des contenus par mots-clés]({{ config.extra.geotribu_main_site }}tags/). De préférence, choisir parmi les [mots-clés existants]({{ config.extra.geotribu_main_site }}tags/), en respectant la casse.

----

## Image d'illustration { #image-illustration-contenu }

En résumé, un/e auteurice a 2 options :

- soit **utiliser une image de son cru**, respectant [la charte éditoriale](../requirements.md#charte-éditoriale) et les contraintes techniques :
    - Format : PNG ou JPG
    - Dimensions : entre 600x300 et 800x400
    - Ratio : viser 2/1 ou proche

    Par exemple, l'en-tête de [l'article sur BAM (Biodiversité Autour de Moi)]({{ config.extra.geotribu_main_site }}articles/2025/2025-12-11_BAM-widget/) spécifie une image :

    ```markdown title="En-tête d'article qui spécifie une image à utiliser"  linenums="1" hl_lines="12"
    ---
    title: BAM (Biodiversité Autour de Moi)
    subtitle: Les données ouvertes de biodiversité accessibles facilement à tous, partout !
    authors:
        - Camille MONCHICOURT
    categories:
        - article
    comments: true
    date: 2025-12-11
    description: Un nouveau widget de biodiversité pour afficher les espèces observées autour d'un lieu.
    icon: material/bee-flower
    image: https://cdn.geotribu.fr/img/articles-blog-rdp/articles/2025/bam_widget/BAM-widget-thumb.png
    license: cc4_by-sa
    robots: index, follow
    tags:
        - biodiversité
        - open source
        - widget
    ---

    # BAM (Biodiversité Autour de Moi), les données ouvertes de biodiversité accessibles facilement à tous, partout !
    [...]
    ```

    Elle est donc utilisée sur la page d'accueil, les réseaux sociaux et la [newsletter](http://eepurl.com/juwDj-/) :

    ![Exemple d'image spécifiée dans l'en-tête qui se retrouve en page d'accueil](https://cdn.geotribu.fr/img/internal/contribution/geotribu_page-accueil_image_article_specifique_exemple_BAM.webp){: .img-center loading=lazy }

- soit **laisser le moteur du site générer une image** aux bonnes dimensions en utilisant des éléments de base du site (fond, nom, police, couleur dominante, logo...) et les métadonnées de l'article (titre, sous-titre, icône). Il suffit alors de laisser le champ `image` de l'en-tête YAML vide. Par exemple, l'en-tête de l'article [Que se cache-t-il derrière l'image Docker officielle de QGIS Server ? ]({{ config.extra.geotribu_main_site }}articles/2025/2025-04-15_official-qgis-server-docker-image/) :

    ```markdown title="En-tête d'article qui ne spécifie pas d'image"  linenums="1" hl_lines="12"
    ---
    title: "Que se cache-t-il derrière l'image Docker officielle de QGIS Server ?"
    subtitle: (bouh !)
    authors:
        - Paul BLOTTIERE
    categories:
        - article
    comments: true
    date: 2025-04-15
    description: "Les mystères de l'image Docker officielle de QGIS Server"
    icon: material/docker
    image:
    license: default
    robots: index, follow
    tags:
        - Docker
        - QGIS
        - QGIS Server
    ---

    # Que se cache-t-il derrière l'image Docker officielle de QGIS Server ?
    [...]
    ```

    Génère cette image :

    ![Exemple d'image d'illustration générée par le site](https://geotribu.fr/assets/images/social/articles/2025/2025-04-15_official-qgis-server-docker-image.png){: .img-center loading=lazy }

[Voir aussi la page dédiée aux aperçus de partage](../internal/social_cards.md){: .md-button }
{: align=middle }

----

## Catégories

Voici les valeurs possibles pour les catégories de contenus :

- article : terme générique pour tout contenu, hormis les GeoRDP.
- contribution : pour les contenus du guide de contribution
- événement : qualifie les articles présentant une conférence, un salon, etc.
- meta : qualifie un contenu potentiellement destiné à la section "A propos" évoquant l'équipe, Geotribu, etc.
- revue de presse : Réservé aux GeoRDP. Exclusif.
- tutoriel : qualifie un article

Il est possible d'en cumuler certaines :

```yaml
categories:
    - article
    - tutoriel
```

## Exemple

Exemple pour la GeoRDP de Noël 2020 :

```yaml
---
title: "Revue de presse du 25 décembre 2020"
authors:
    - Geotribu
categories:
    - revue de presse
date: 2020-12-25
description: "GeoRDP du 25 décembre 2020 : la revue de presse géomatique de Geotribu pour souhaiter Joyeux Noël et bonnes fêtes !"
image: "https://cdn.geotribu.fr/img/articles-blog-rdp/merry_christmas_blender.png"
license: default
tags:
    - Cerema
    - drone
    - FIG
    - GeoRezo
    - GeoServer
    - IGN
    - Mapbox
    - Nominatim
    - open data
    - OpenLayers
    - PostGIS
---
```
