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

# Générer le(s) site(s) web avec Mkdocs

![icône générateur de site web statique](https://cdn.geotribu.fr/img/logos-icones/divers/web_static_generator.webp "icône générateur de site web statique"){: .img-thumbnail-left }

Cette page décrit comment générer le site web principal de Geotribu (<{{ config.extra.geotribu_main_site }}>) à partir des sources. A noter que les sites secondaires (comme celui-ci) suivent en général peu ou prou la même logique.

Avant d'aller plus loin, il faut disposer des sources du site et avoir configuré son environnement local :

[Configurer son environnement local :octicons-desktop-download-16:](../edit/local_edition_setup.md){: .md-button .md-button--primary }
{: align=middle }

----

## Mkdocs

C'est l'outil qui sert à générer le site web à partir des contenus rédigés en [markdown] et configuré dans le fichier `mkdocs.yml` et dérivés. Voici quelques bases pour l'utiliser... qui ne vous épargnent pas le droit de regarder l'aide `mkdocs --help` :wink:.

### Différentes configurations

Depuis la rentrée 2021, Geotribu sponsorise le thème [Material for Mkdocs](https://squidfunk.github.io/mkdocs-material/insiders/) afin de pérenniser le projet et tirer parti des fonctionnalités réservées aux financeurs (principe du Sponsorware).

La clé de licence (en fait, un *token* GitHub lié au compte de Julien) devant rester secrète, nous gérons donc plusieurs fichiers de configuration afin de pouvoir s'adapter aux différents cas.

Selon la configuration retenue, il faut [installer les dépendances correspondantes à l'aide du token](../edit/local_edition_setup.md#installer-les-dépendances-du-site).

| Fichier              | Fonctionnalités payantes | Complet | Commentaire |
| :------------------- | :-----: | :-----: | :---------- |
| `mkdocs.yml`         | **X**   | **X**   | Configuration complète utilisée pour le site en production. Utilisé par défaut. |
| `mkdocs-free.yml`    |         | **X**   | Configuration sans les fonctionnalités payantes. |
| `mkdocs-minimal.yml` |         |         | Configuration minimaliste qui n'active qu'un minimum de plugins et d'extensions pour obtenir de meilleures performances lors de l'édition en local. |

### Fusionner les configurations

Le site grossissant à mesure des usages et contenus, la maintenance s'est complxifiée, surtout avec plusieurs fichiers de configuration à gérer, rendus parfois illisibiles par certaines configurations verbeuses.  

La gestion de certains plugins et extensions a été modularisée dans des fichiers YAML séparés et certains éléments sont générés via des scripts (comme les derniers contenus).

Retenir que :

- le sous-dossier `config` contient les fichiers YAML "enfants" à fusionner
- le sous-dossier `scripts` contient les scripts à exécuter dans l'ordre de leur nom

Version complète :

```sh
python scripts/050_mkdocs_populate_latest.py -c mkdocs.yml
python scripts/100_mkdocs_config_merger.py -c mkdocs.yml
```

Version gratuite :

```sh
python scripts/050_mkdocs_populate_latest.py -c mkdocs-free.yml
python scripts/100_mkdocs_config_merger.py -c mkdocs-free.yml
```

### Générer le site web

Version complète :

```sh
mkdocs build
```

Version complète gratuite :

```sh
mkdocs build -f mkdocs-free.yml
```

Version minimale :

```sh
mkdocs build --config-file mkdocs-minimal.yml
```

Le site généré est dans le répertoire : `./build/mkdocs/site`.

### Avoir un rendu local mis à jour selon les modifications

Pour voir les changements en local sans les pousser sur le dépôt central, il est possible de servir le site et qu'il se recharge automatiquement quand on modifie les fichiers :

Version complète :

```sh
# regénération complète
mkdocs serve
# regénération rapide
mkdocs serve --dirtyreload
```

Version complète gratuite :

```sh
# regénération complète
mkdocs serve -f mkdocs-free.yml --dirtyreload
# regénération rapide
mkdocs serve --config-file mkdocs-free.yml --dirtyreload
```

Version minimale :

```sh
# regénération complète
mkdocs serve --config-file mkdocs-minimal.yml
# regénération rapide
mkdocs serve --config-file mkdocs-minimal.yml --dirtyreload
```

Par défaut, le site est accessible sur <http://localhost:8000> mais il est possible de spécifier le port à utiliser : `mkdocs serve -a localhost:8085`.

----

## Avec Docker

![logo Docker](https://cdn.geotribu.fr/img/logos-icones/logiciels_librairies/docker.png){: .img-thumbnail-left }

Il est également possible d'utiliser Docker. L'avantage est que c'est alors la seule dépendance à installer (plus besoin de Python, NodeJS ou même de Git si vous téléchargez le dépôt). L'inconvénient est que c'est assez lourd pour un site qui se veut léger :wink: !

```sh
docker-compose -f "docker-compose-mkdocs.dev.yml" up --build
```

Le site est alors accessible sur : <http://0.0.0.0:8000>
