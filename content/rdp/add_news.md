---
title: Ajouter une news
authors:
    - Geotribu
categories:
    - contribution
comments: true
date: 2021-09-30
description: Ajouter une actualité à la prochaine revue de presse de Geotribu (GeoRDP).
icon: material/newspaper-plus
image:
license: default
tags:
    - guide
    - GeoRDP
    - workflow
---

# Proposer une news pour une revue de presse

![icône news générique](https://cdn.geotribu.fr/img/internal/icons-rdp-news/news.png "icône news générique"){: .img-thumbnail-left }

La rédaction des revues de presse (GeoRDP) est collaborative et ouverte à toute personne souhaitant partager une actualité ou contribuer à la veille commune. L'équipe est là pour coordonner les différentes contributions et s'assurer de la cohérence et de la qualité de la publication.

Gardez en tête que le travail de l'équipe est **bénévole**. A ce titre, plus votre contenu est conforme à nos prérequis et aux guides de contribution, moins il ne demande de travail de notre part. Ce que vous ne faites pas, nous devrons le faire.

Bref, appliquons le principe du *fair-use* au bénévolat :hugging: !

<!-- markdownlint-disable MD034 -->
[Proposer une news par email :fontawesome-solid-paper-plane:](mailto:geotribu+rdp@gmail.com?subject=Contribution à la GeoRDP){: .md-button }
[Proposer une news via GitHub :fontawesome-solid-ticket:](https://github.com/geotribu/website/issues/new?assignees=Guts&labels=contribution+externe%2Crdp%2Ctriage&template=RDP_NEWS.yml){: .md-button }
{: align=middle }
<!-- markdownlint-enable MD034 -->

----

## Ajouter une news en utilisant son environnement local

![logo Git](https://cdn.geotribu.fr/img/logos-icones/divers/git.png "logo Git"){: .img-thumbnail-left }

Pour les plus aguerri/es d'entre nous, il est évidemment possible de contribuer à une revue de presse en travaillant localement. C'est d'ailleurs beaucoup plus confortable !

On considère ici que la revue de presse existe déjà (sinon [voir ici pour la créer](./create_rdp.md)) et que l'on dispose d'un environnement local correctement configuré et **à jour**, en particulier en ce qui concerne les dépendances Python et du dépôt Git :

[Configurer son environnement local pour contribuer à Geotribu :octicons-desktop-download-16:](../edit/local_edition_setup.md){: .md-button .md-button--primary }
{: align=middle }

### 1. Trouver la branche de la revue de presse active

Pour afficher les différentes branches actives afin de sélectionner celle souhaitée, il suffit de lister les branches locales et distantes en filtrant sur celles des revues de presse `rdp/`.

Lister les branches locales :

```sh
> git branch --list '*rdp/*'
  rdp/2021-09-17
  rdp/2022-12-16
  rdp/2023-01-06
```

Lister les branches distantes :

```sh
> git branch --list '*rdp/*' -r
  origin/rdp/2023-01-06
```

### 2. Basculer sur la branche de la revue de presse active

```sh
> git checkout rdp/2023-01-06
Switched to a new branch 'rdp/2023-01-06'
```

!!! important
    Avant d'ajouter du contenu sur une branche déjà existante, bien penser à récupérer les changements faits par les autres contributeurs avant, en faisant : `git fetch --prune` puis `git pull --prune`
    ```  

### 3. Ajouter sa news

Le bon moment de se rappeler [la structure d'une news](./structure_news.md) et de consulter les guides de rédaction pour écrire du bon Markdown :

- les [bases de la syntaxe Markdown](../guides/markdown_basics.md)
- les [règles de rédaction de Geotribu](../guides/markdown_quality.md)
- [comprendre l'en-tête des fichiers Markdown](../guides/metadata_yaml_frontmatter.md)
- comment [intégrer une image](../guides/image.md) sur Geotribu

Il y en a d'autres (intégration Twitter, vidéo, etc.) : ne pas hésiter à les lire et relire !

### 4. Enregistrer sa modification

Allez, cette fois, vous avez ajouté votre Markdown, il s'agit désormais de l'enregistrer et de l'envoyer.

Ajouter un message bref qui décrit votre ajout ou modification :

```sh
git commit -am "Ajout news sur la carte de la semaine"
```

### 5. Pousser son contenu avec Git vers le dépôt central

> ou vers un dépôt de son compte (*fork*) si on n'a pas les droits.

- si c'est une nouvelle branche

    ```bash
    git push -u origin "rdp/2023-01-06"
    ```

- Ou, si c'est une branche déjà existante

    ```bash
    git push
    ```
