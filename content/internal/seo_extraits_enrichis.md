---
title: "Web sémantique et extraits enrichis"
categories:
    - article
    - Geotribu
comments: true
date: 2022-06-15 14:20
description: "Sous le GéoCapot : comment l'en-tête des contenus est utilisé pour générer des données structurées et extraits enrichis."
icon : material/semantic-web
image: "https://cdn.geotribu.fr/img/internal/contribution/rich_snippets/seo_extrait_enrichi_article_multi-auteurs.png"
tags:
    - coulisses
    - GéoCapot
    - Jinja2
---

# Utilisation de l'en-tête pour le web sémantique et les extraits enrichis

Lors de la rédaction d'un contenu sur Geotribu, que ce soit une revue de presse, un article, un guide de contribution ou autre, on insiste beaucoup sur l'en-tête du fichier, comme [l'illustre ce guide](../guides/metadata_yaml_frontmatter.md).

Pourquoi ? car c'est ainsi que le site génère des données structurées standardisées qui sont notamment utilisées pour les extraits enrichis des moteurs de recherche.

![Google Search - Exemple d'extrait enrichi](https://cdn.geotribu.fr/img/internal/contribution/rich_snippets/extrait_enrichi_google_dashboard_qgis.webp "Extrait enrichi sur une recherche Google"){: loading=lazy .img-center }

## Web sémantique et données structurées

![logo JSON-LD](https://cdn.geotribu.fr/img/logos-icones/programmation/json-ld.webp "logo JSON-LD"){: .img-thumbnail-left }

La structuration des données géographiques, ça vous semble important ? Eh bien, c'est la même chose quand il s'agit des pages webs. C'est d'ailleurs le principe fondateur de ce qu'on appelle plus communément le **web sémantique** : intégrer des données structurées dans le contenu des pages de façon à faciliter le travail d'indexation et de mise en relation des contenus.  
Evidemment les structures répondent à des standards dont l'élaboration est liée aux acteurs de l'industrie mais aussi aux initiatives communautaires. Les schémas de données, qui gèrent le relationnel, sont documentés et regoups sur le site : [schema.org](https://schema.org/). C'est ce site et cette dynamique qui ont inspiré [schema.data.gouv.fr](https://schema.data.gouv.fr/).

Ces données sont également utilisées par les moteurs de recherche et favorisent le référencement ou plutôt permettent un affichage enrichi dans les pages de résultats, d'où le nom donné par Google : extrait enrichi (_rich snippet_ en anglais).

Il existe de nombreux schémas décrivant différents types d'objets : _Article_, _WebSite_, _Author_, _Organization_, etc. Les personnes qui travaillent sur les portails de données ouvertes connaissent bien le sujet puisqu'il existe un schéma standardisé pour les [jeux de données](https://schema.org/Dataset) qui permet notamment de faire indexer ses données dans le moteur dédié de Google : [Dataset Search](https://datasetsearch.research.google.com/).

![Google Dataset Search](https://cdn.geotribu.fr/img/internal/contribution/rich_snippets/google_dataset_search_exemple_bornes_elec.webp "Google Dataset Search"){: loading=lazy .img-center }

----

## Processus

![logo Jinja](https://cdn.geotribu.fr/img/logos-icones/logiciels_librairies/jinja.png "logo Jinja"){: .img-thumbnail-left }

Au moment de la [transformation des fichiers markdown en fichiers HTML](./markdown_engine.md), le site génère un objet au format [JSON-LD] (_JSON Linked Data_), intégré à la page HTML, à partir de plusieurs éléments :

- l'en-tête fournit l'essentiel des informations : date, auteur(s), mots-clés, description, image, etc.
- l'URL : permet d'affiner le type de contenu, notamment pour distinguer un article d'une revue de presse.
- le fichier de l'auteur s'il respecte le nommage `team/{pnnn}.md`

Les informations sont manipulées avec la syntaxe [Jinja](https://fr.wikipedia.org/wiki/Jinja_(moteur_de_template)), utilisée par [Mkdocs], dans le [template `main.html`](https://github.com/geotribu/website/blob/master/content/theme/main.html).

Exemple du bloc permettant de gérer les contenus avec plusieurs auteurs :

{% raw %}

```jinja linenums="1"
[...]
{% elif author is iterable and (author is not string and author is not mapping) %}
  {% if author | length > 1 %}
    {# plusieurs auteurs #}
    "author": [
    {% for a in author %}
        {% if a != config.site_author %}
          {% set author_split = a.split(' ', 1) %}
          {
          "@type": "Person",
          "name": "{{ a }}",
          "url": "{{ config.extra.geotribu_main_site }}team/{{ author_split[0][:1] | lower }}{{ author_split[1][:3] | lower }}"
          {% if a != author|last %}
          {# Si l'auteur n'est pas le dernier de la liste, on ajoute une virgule #}
            },
          {% else %}
          {# Si c'est le dernier, pas de virgule #}
            }
          {% endif %}
        {% endif %}
    {% endfor %}
    ],
[...]
```

{% endraw %}

----

## Exemples de données structurées générées sur Geotribu

Les données structurées sont stockées en [JSON-LD] (_JSON Linked Data_) sous forme de script intégré dans l'en-tête du fichier HTML (balise `head`). Pour y accéder, il suffit d'ouvrir le fichier source (++ctrl+u++ sur le navigateur) ou d'utiliser un validateur ou visualiseur dédié (il y en a plein sur le net).

### Page d'accueil

On y intègre les métadonnées principales du site, ainsi que des informations fonctionnelles, notamment le moteur de recherche, conformément au [schéma Sitelink](https://developers.google.com/search/docs/advanced/structured-data/sitelinks-searchbox) :

```jsonld linenums="1"
{
  "@context": "http://www.schema.org",
  "@type": "WebSite",
  "name": "Geotribu",
  "alternateName": "Géotribu",
  "url": "{{ config.extra.geotribu_main_site }}",
  "sameAs": [
    "http://geotribu.fr",
    "http://geotribu.net",
    "https://github.com/geotribu/",
    "https://facebook.com/geotribu",
    "https://twitter.com/geotribu"
  ],
  "potentialAction": {
    "@type": "SearchAction",
    "target": "{{ config.extra.geotribu_main_site }}?q={search_term}",
    "query-input": "required name=search_term"
  }
}
```

### Article

Exemple pour [cet article]({{ config.extra.geotribu_main_site }}articles/2021/2021-02-15_ignfr2map_carte_liens_IGN_open-data_7_etapes/) :

```jsonld linenums="1"
{
  "@context": "https://schema.org",
  "@type": "Article",
  "mainEntityOfPage": {
    "@type": "WebPage",
    "@id": "{{ config.site_url }}articles/2021/2021-02-15_ignfr2map_carte_liens_IGN_open-data_7_etapes/"
  },
  "headline": "Geotribu - ign2map : Du site à la carte en 7 étapes",
  "abstract": "ign2map : le petit projet de Geotribu pour rendre l’expérience de téléchargement des données ouvertes de l'IGN plus interactive.",
  "datePublished": "2021-02-15 11:11",
  "image": "https://cdn.geotribu.fr/img/articles-blog-rdp/articles/ign_opendata_map/ign_opendata_map_html_rendu.png","author": [
    {
      "@type": "Person",
      "name": "Florian Boret",
      "url": "{{ config.site_url }}team/fbor"
    },
    {
      "@type": "Person",
      "name": "Julien Moura",
      "url": "{{ config.site_url }}team/jmou"
    }
  ],
  "publisher": {
    "@type": "Organization",
    "name": "Geotribu",
    "logo": {
      "@type": "ImageObject",
      "url": "https://cdn.geotribu.fr/img/internal/charte/geotribu_logo_64x64.png"
    }
  }
}
```

![Schéma article validé par schema.org](https://cdn.geotribu.fr/img/internal/contribution/rich_snippets/seo_extrait_enrichi_article_multi-auteurs.png "Schéma article validé par schema.org"){: loading=lazy.img-center }

### Revue de presse

Exemple pour [cette GeoRDP]({{ config.extra.geotribu_main_site }}rdp/2022/rdp_2022-06-03/) :

```jsonld linenums="1"
{
  "@context": "https://schema.org",
  "@type": "Article",
  "mainEntityOfPage": {
    "@type": "WebPage",
    "@id": "{{ config.extra.geotribu_main_site }}rdp/2022/rdp_2022-06-03/"
  },
  "headline": "{{ config.site_name }} - Revue de presse du 3 juin 2022",
  "abstract": "Cette semaine on vous propose une déambulation autour de divers sujets : traduction de logiciels libres, une carte de bruit, le programme Lidar HD, la donnée OSO, la détection de bâtiments et les logiciels libres en thèse",
  "datePublished": "2022-06-03 14:20",
  "image": "https://cdn.geotribu.fr/img/articles-blog-rdp/capture-ecran/carte_bruit_Paris.jpg",
  "author": {
    "@type": "Organization",
    "name": "Geotribu",
    "url": "{{ config.extra.geotribu_main_site }}"
    },
  "publisher": {
    "@type": "Organization",
    "name": "Geotribu",
    "logo": {
      "@type": "ImageObject",
      "url": "https://cdn.geotribu.fr/img/internal/charte/geotribu_logo_64x64.png"
    }
  }
}
```

----

## Ressources

- le [schéma d'un article sur schema.org](https://schema.org/Article)
- le [validateur de schéma](https://validator.schema.org/)
- [description de l'extrait enrichi de type Article dans la documentation de Google](https://developers.google.com/search/docs/advanced/structured-data/article/)
- Google propose un [site pour tester les extraits enrichis](https://search.google.com/test/rich-results).

<!-- Liens de référence -->
[JSON-LD]: https://json-ld.org/
