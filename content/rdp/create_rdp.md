---
title: "Créer une revue de presse"
subtitle: Et le géoHumain créa la GeoRDP
authors:
    - Geotribu
    - Julien MOURA
categories:
    - contribution
comments: true
date: 2021-12-29
description: "Guide de création d'une revue de presse (GeoRDP) sur Geotribu : méthodologie, script bash, GitHub Workflow, etc."
icon: material/newspaper
image:
license: default
tags:
    - GeoRDP
    - GitHub
    - GitHub Actions
    - GitHub Workflow
    - guide
    - workflow
---

# Création d'une revue de presse

![icône news générique](https://cdn.geotribu.fr/img/internal/icons-rdp-news/news.png "icône news générique"){: .img-thumbnail-left }

Concrètement, une revue de presse est un fichier markdown, nommé d'une certaine façon, stocké dans le dossier `content/rdp/` et organisé en sections dans lesquelles les contributeur/ices viennent ensuite insérer leurs "news". Le processus de contribution est bâti autour de la logique de Git.

Avant d'ouvrir la revue de presse aux contributions, il est donc nécessaire de créer :

1. une branche dédiée dans le dépôt du site
2. le fichier Markdown avec la structure type
3. la Pull Request permettant de visualiser les différentes contributions puis de publier (fusionner) la revue de presse une fois finalisée

Il est possible de créer une revue de presse de plusieurs façons détaillées dans cette page. Choisissez selon vos préférences et votre niveau de confort avec les outils Git/GitHub :

- en déclenchant la création automatique en 3 clics depuis l'interface web de GitHub
- en utilisant l'interface web de GitHub
- en utilisant le script intégré au dépôt
- en utilisant Git en ligne de commande

!!! info "Zone réservée"
    La création d'une nouvelle revue de presse nécessite de disposer des droits d'écriture sur le dépôt GitHub du site principal : <https://github.com/geotribu/website/>.

## Automatiquement via GitHub

![icône GitHub Actions](https://cdn.geotribu.fr/img/logos-icones/divers/github_actions.png "GitHub Actions"){: .img-thumbnail-left }

L'outillage et la logique de publication de Geotribu sont largement basés sur Git et la plateforme GitHub. Nous utilisons notamment les principes de l'intégration et du déploiement continus ([CI/CD pour les intimes](https://fr.wikipedia.org/wiki/CI/CD)).

La méthode la plus simple pour créer une nouvelle revue de presse est donc d'utiliser le *workflow* ":newspaper2: New GeoRDP" disponible sur GitHub :

1. Se rendre sur l'onglet `Actions` et sélectionner le *workflow* ":newspaper2: New GeoRDP" ou [cliquer ici](https://github.com/geotribu/website/actions/workflows/manual_new_rdp.yml)
2. Cliquer sur `Run workflow`
3. Entrer les infos demandées :
    - branche : `master`
    - date de la revue de presse : doit être au format `YYYY-MM-DD` et pointer sur un vendredi
    - cocher la case pour envoyer automatiquement une notification sur Matrix
4. Cliquer sur le bouton vert `Run workflow`.

Après une trentaine de secondes, on obtient :

- une branche dédiée pour la revue de presse
- un fichier Markdown avec la structure type et la date de publication
- une Pull Request basée sur le modèle
- une notification dans le canal Matrix de Geotribu pour informer la communauté

Voici une vidéo illustrant le déroulé :

<iframe width="100%" height="400" src="https://www.youtube-nocookie.com/embed/7-G_gRJrUPA" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<!-- markdownlint-disable MD046 -->
!!! abstract "Prérequis"
    La bonne exécution du workflow dépend de ces éléments :

    - la revue de presse ou sa branche n'ont pas déjà été créées par ailleurs
    - le modèle de revue de presse est à jour et bien présent : `content/rdp/templates/template_rdp.md`
    - le modèle de Pull Request est bien présent : `.github/PULL_REQUEST_TEMPLATE/RDP.md`
    - le jeton d'authentification pour envoyer la notification sur Matrix est bien configurée dans [les secrets du dépôt](https://github.com/geotribu/website/settings/secrets/actions)
<!-- markdownlint-enable MD046 -->

----

## Manuellement via l'interface web de GitHub

Il est également possible d'utiliser l'ancienne procédure manuelle.  
Voici une vidéo retraçant les étapes de création d'une revue de presse via l'interface web de GitHub :

<iframe width="100%" height="400" src="https://www.youtube.com/embed/dVpOdGYAtIk" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

----

## Utiliser le script intégré

Si Git est installé et que vous disposez d'un terminal Bash et du dépôt du site localement, il est possible d'utiliser [le script intégré](https://github.com/geotribu/website/blob/master/scripts/new_rdp.sh) :

```bash
# stocker la date de la RDP au format YYYY-MM-DD
DATE_RDP=2022-01-07

# exécuter le script
scripts/new_rdp.sh $DATE_RDP

# pousser vers le dépôt distant
git pull
git checkout -b rdp/$DATE_RDP
git add content/rdp/
git commit -am "Crée la GeoRDP $DATE_RDP"
git push origin rdp/$DATE_RDP

# ouvrir la Pull Request sur GitHub
xdg-open "https://github.com/geotribu/website/compare/master...rdp/$DATE_RDP?quick_pull=1&template=RDP.md"
```

Ne pas oublier ensuite de :

1. se rendre sur [GitHub pour créer la Pull Request](https://github.com/geotribu/website/pulls) :
    1. cliquer sur le bouton vert `New pull request`
    1. sélectionner votre branche dans la liste déroulante à droite (`compare:`)
2. sur [le canal Matrix](https://matrix.to/#/#geotribu:matrix.org) pour notifier l'équipe et la communauté

----

## Processus détaillé avec Git en ligne de commande

Cette partie explique chaque étape du processus de création d'une revue de presse pour comprendre ce que font les automatisations présentées au-dessus (script, GitHub Actions...).

### 1. Créer la branche de la revue de presse

![logo Git](https://cdn.geotribu.fr/img/logos-icones/divers/git.png "logo Git"){: .img-thumbnail-left }

La première étape consiste à créer une branche [Git] pour la revue de presse. Elle n'est réalisable que par une personne disposant d'un compte GitHub ayant les droits en écriture sur le dépôt du site : <https://github.com/geotribu/website/>.

Il est important de respecter la convention de nommage `rdp/YYYY-MM-DD` où :

- `YYYY` est l'année de publication
- `MM` le mois de publication prévisionelle
- `DD` le jour de publication prévisionnelle

Exemple si la GeoRDP devait être publiée le 17 septembre 2021 : `rdp/2021-09-17`.

1. Mettre à jour le dépôt local :

    ```bash
    git pull --prune origin master
    ```

2. Vérifier qu'une branche n'existe pas déjà en listant les branches du dépôt sur GitHub en filtrant sur la structure de nommage :

    ```bash
    git branch -r -l 'origin/rdp/*'
    ```

3. Créer la nouvelle branche :

    ```bash
    $ git checkout -b rdp/2021-09-17
    Switched to a new branch 'rdp/2021-09-17'
    ```

4. Pousser la branche sur GitHub :

    ```bash
    $ git push origin rdp/2021-09-17
    Total 0 (delta 0), réutilisés 0 (delta 0), réutilisés du pack 0
    remote:
    remote: Create a pull request for 'rdp/2021-09-17' on GitHub by visiting:
    remote:      https://github.com/geotribu/website/pull/new/rdp/2021-09-17
    remote:
    To github.com:geotribu/website.git
    * [new branch]        rdp/2021-09-17 -> rdp/2021-09-17
    ```

### 2. Créer le fichier de la revue de presse

![icône globe tricot](https://cdn.geotribu.fr/img/internal/icons-rdp-news/matiere.png "icône globe tricot"){: .img-thumbnail-left }

Afin d'accueillir les news, il s'agit de créer un fichier en respectant l'organisation et le nommage des fichiers : `content/rdp/YYYY/rdp_YYYY-MM-DD.md` où :

- `YYYY` est l'année de publication
- `MM` le mois de publication prévisionelle
- `DD` le jour de publication prévisionnelle

Exemple si la GeoRDP devait être publiée le 17 septembre 2021 : `content/rdp/2021/rdp_2021-09-17.md`.

#### Structure type et modèle

Les revues de presse sont structurées de la même façon d'une édition à l'autre, facilitant leur consultation et les traitements automatiques. Le plus simple est donc de copier/coller la structure type à partir du [modèle maintenu à jour sur GitHub](https://raw.githubusercontent.com/geotribu/website/master/content/rdp/templates/template_rdp.md)

Ensuite, il faut mettre à jour certains éléments dans [l'en-tête du fichier](../guides/metadata_yaml_frontmatter.md), mettre à jour les valeurs de `title:`, `date:` et `description:`. L'image est à choisir plutôt juste avant la publication en piochant dans celles des news retenues (voir [aussi cette page sur les images](../guides/metadata_yaml_frontmatter.md/#image-illustration-contenu)).

Les lignes concernées sont surlignées ci-dessous (attention, cela peut varier selon le modèle utilisé) :

```markdown hl_lines="2 7 8" linenums="1" title="Lignes à éditer dans le modèle de revue de presse"
---
title: "Revue de presse du 21 août 2021"
authors:
    - Geotribu
categories:
    - revue de presse
date: 2021-08-21
description: "Description de 160 caractères maximum qui résume la RDP. Cette description est présente dans le flux RSS, la newsletter, les moteurs de recherche, en page d'accueil... "
image: "Image d'illustration de la RDP qui sert ensuite dans la mise en avant : réseaux sociaux, flux RSS... 400x800 en PNG"
license: default
robots: index, follow
tags:
    - tag 1
    - tag 2
    - ...
---

[...]
```

### 3. Pousser le fichier sur GitHub

Enfin, il faut pousser le fichier sur la branche créée sur GitHub.

```bash
git add content/rdp/
git commit -am "Crée la GeoRDP du DD MM YYYY"
git push origin rdp/YYYY-MM-DD
```

<!-- Footnotes -->

<!-- Hyperlinks reference -->
[Git]: https://fr.wikipedia.org/wiki/Git
