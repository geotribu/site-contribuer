# Site web dédié à la contribution à Geotribu

[![🚀 Déploiement](https://github.com/geotribu/site-contribuer/actions/workflows/deploy.yml/badge.svg)](https://github.com/geotribu/site-contribuer/actions/workflows/deploy.yml)
[![pre-commit.ci status](https://results.pre-commit.ci/badge/github/geotribu/site-contribuer/main.svg)](https://results.pre-commit.ci/latest/github/geotribu/site-contribuer/main)

----

## Démarrage rapide

### Prérequis

- Python >= 3.10

#### Recommandés

- Visual Studio Code (configuration intégrée)

### Installation

Après avoir cloné ou téléchargé le dépôt, installer les prérequis (de préférence dans un environnement virtuel) :

```bash
python -m pip install -U pip setuptools wheel
python -m pip install -U -r requirements.txt
```

### Générer le site

Version complète :

```bash
properdocs build
```

Version complète :

```bash
properdocs build -f properdocs.yml --dirtyreload
```

### Servir le site en local

Version complète :

```bash
properdocs serve --dirtyreload
```

Version complète :

```bash
properdocs serve -f properdocs.yml --dirtyreload
```

Le site est accessible en local à l'adresse suivante : <http://localhost:8000/>.  
Quand un contenu est modifié, le site est automatiquement rechargé.

----

## Soutenir

Afin de pérenniser le site, nous avons ouvert un compte sur Liberapay : <https://liberapay.com/Geotribu/>.

![Liberapay receiving](https://img.shields.io/liberapay/receives/Geotribu?color=green&label=re%C3%A7oit&style=flat-square)
![Liberapay patrons](https://img.shields.io/liberapay/patrons/Geotribu?color=blue&label=soutiens&style=flat-square)

L'objectif de ce financement est de :

- financer les outils open-source que l'on utilise pour le site :
- soutenir GeoRezo (pour le CDN)
- financer les suffixes du nom de domaine (geotribu.fr/.net/.org)
