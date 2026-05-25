---
title: Générer le(s) site(s) web à partir des sources
subtitle: Déploie tes HTM-ailes Geotribu !
categories:
    - publication
    - tutoriel
comments: true
date: 2023-08-25
description: "Sous le GéoCapot : générer le(s) site(s) web de Geotribu à partir des sources avec Mkdocs et le thème Material."
icon: material/web-sync
tags:
    - GéoCapot
    - Markdown
    - Mkdocs
---

# Générer le(s) site(s) web avec ~~Mkdocs~~ Properdocs

![icône générateur de site web statique](https://cdn.geotribu.fr/img/logos-icones/divers/web_static_generator.webp "icône générateur de site web statique"){: .img-thumbnail-left }

Cette page décrit comment générer le site web principal de Geotribu (<{{ config.extra.geotribu_main_site }}>) à partir des sources. À noter que les sites secondaires (comme celui-ci) suivent en général peu ou prou la même logique.

!!! question "Properdocs ? Mkdocs ? gné ?"
    Suite à un [drama](https://github.com/orgs/ProperDocs/discussions/33) dans la communauté [Mkdocs] et après avoir étudié les différentes alternatives ([voir cette PR](https://github.com/geotribu/site-contribuer/pull/159)), nous avons opté pour le principal fork : [Properdocs]. Cela ne devrait pas impacter les contributions vu que tout se passe sous le capot. Si tu aimes le :popcorn:, ce [billet de blog](https://fpgmaas.com/blog/collapse-of-mkdocs/) retrace le match.

Avant d'aller plus loin, il faut disposer des sources du site et avoir configuré son environnement local :

[Configurer son environnement local :octicons-desktop-download-16:](../edit/local_edition_setup.md){: .md-button .md-button--primary }
{: align=middle }

----

## Properdocs

Anciennement Mkdocs, c'est l'outil qui sert à générer le site web à partir des contenus rédigés en [Markdown] et configuré dans le fichier `properdocs.yml` (le fichier `mkdocs.yml` est aussi supporté). Voici quelques bases pour l'utiliser... qui ne vous épargnent pas le droit de regarder l'aide `properdocs --help` :wink:.

### Le thème "micro-framework" Material

De 2021 à l'arrêt du projet en novembre 2025, Geotribu a sponsorisé le thème [Material for Mkdocs](https://squidfunk.github.io/mkdocs-material/insiders/) afin de pérenniser le projet et tirer parti des fonctionnalités réservées aux financeurs (principe du Sponsorware).

Depuis leur choix de créer un tout nouveau projet Zensical (sur lequel Geotribu pourrait basculer un de ces jours), nous avons opté pour le [principal fork MaterialX](https://jaywhj.github.io/mkdocs-materialx/differences.html).

### Fusionner les configurations

Le site grossissant à mesure des usages et contenus, la maintenance s'est complexifiée, avec un fichier de configuration `mkdocs/properdocs.yml` rendu parfois illisibile par certaines configurations verbeuses.  

La gestion de certains plugins et extensions a été modularisée dans des fichiers YAML séparés et certains éléments sont générés via des scripts (comme les derniers contenus).

Retenir que :

- le sous-dossier `config` contient les fichiers YAML "enfants" à fusionner
- le sous-dossier `scripts` contient les scripts à exécuter dans l'ordre de leur nom

Avant de lancer la génération du site, il faut donc fusionner les configurations en un seul fichier :

```sh
python scripts/100_mkdocs_config_merger.py
```

### Générer le site web

```sh
properdocs build
```

Le site généré est dans le répertoire : `./build/mkdocs/site`.

### Avoir un rendu local mis à jour selon les modifications

Pour voir les changements en local sans les pousser sur le dépôt central, il est possible de servir le site et qu'il se recharge automatiquement quand on modifie les fichiers :

```sh
# regénération complète
properdocs serve
# regénération rapide
properdocs serve --dirtyreload
```

Par défaut, le site est accessible sur <http://localhost:8000> mais il est possible de spécifier le port à utiliser : `mkdocs serve -a localhost:8085`.

----

## Avec Docker

![logo Docker](https://cdn.geotribu.fr/img/logos-icones/logiciels_librairies/docker.png){: .img-thumbnail-left }

Il est également possible d'utiliser Docker. L'avantage est que c'est alors la seule dépendance à installer (plus besoin de Python, NodeJS ou même de Git si vous téléchargez le dépôt). L'inconvénient est que c'est assez lourd pour un site qui se veut léger :wink: !

```sh
docker compose -f "docker-compose-mkdocs.dev.yml" up --build
```

Le site est alors accessible sur : <http://0.0.0.0:8000>
