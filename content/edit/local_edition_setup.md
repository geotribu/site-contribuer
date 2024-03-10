---
title: "Environnement pour l'édition en local"
categories:
    - contribution
    - tutoriel
comments: true
date: 2020-07-23
description: "Guide de contribution à Geotribu : comment déployer l'environnement local idéal pour contribuer tranquillement."
icon: octicons/desktop-download-16
image: "https://cdn.geotribu.fr/img/internal/contribution/geotribu_ide_vscode_local.png"
tags:
    - Git
    - guide
    - installation locale
    - Markdown
    - Python
    - Visual Studio Code
---

<!-- markdownlint-disable MD046 -->

# Installation et configuration de l'environnement de travail pour l'édition locale

![logo console terminal](https://cdn.geotribu.fr/img/logos-icones/divers/ligne_commande.png "logo console terminal"){: .img-thumbnail-left }

Cette page a pour but de vous guider dans les principales étapes afin de pouvoir gérer le site et ses contenus depuis une machine locale. Il est probable que chacun/e doive ajuster selon son propre environnement de travail (chemins de fichiers, répertoires...).

Après avoir rempli [les prérequis](../requirements.md) généraux, pour travailler sur le site en local, il faut donc :

- [Git](#git)
- [Python](#python)

Il est également recommandé :

- d'avoir une connexion autorisée vers le [CDN de Geotribu]
- utiliser, [markdownlint](../internal/markdown_linter.md), le vérificateur automatisé de syntaxe markdown, soit :
    - en activant les vérifications au moment de committer, [les git hooks](../internal/git_hooks_precommit.md)
    - en installant [Node.js (LTS)](https://nodejs.org) pour pouvoir utiliser markdownlint (voir [Rédiger en Markdown : enjeux de qualité et règles](../guides/markdown_quality.md) et ).
    - en utilisant un IDE avec l'extension markdownlint, comme VS Code par exemple

----

## Git

![logo Git](https://cdn.geotribu.fr/img/logos-icones/divers/git.png "logo Git"){: .img-thumbnail-left }

La gestion et la mise en ligne du contenu se font via [Git], une suite d'outils en ligne de commande ([CLI](https://fr.wikipedia.org/wiki/Interface_en_ligne_de_commande)). Si vous n'êtes pas à l'aise avec la ligne de commande, il est possible d'utiliser [GitHub Desktop] en suivant [la documentation officielle](https://docs.github.com/en/desktop).

### Installation

Il y a beaucoup de ressources sur la Toile pour installer et configurer [Git].  
Nous mettons ici une documentation minimaliste destinée à donner la trame globale de l'installation de Git, mais il y a fort à parier que cette documentation soit trop générique. A chacun/e de trouver tuto à son pied si besoin !

=== "Debian (Bash)"
    Pour avoir une version récente de Git, il faut ajouter les dépôts communautaires :

    ```bash
    sudo add-apt-repository ppa:git-core/ppa
    sudo apt update
    sudo apt install git
    ```

=== "Windows (Powershell)"
    Il "suffit" d'utiliser l'installateur à télécharger depuis le site officiel. Quelques ressources tout de même :

    - la [documentation Microsoft](https://docs.microsoft.com/fr-fr/devops/develop/git/install-and-set-up-git#windows)
    - le tutoriel d'[Astuces Informatiques](https://astuces-informatique.com/comment-installer-utiliser-git-sous-windows/)

### Récupérer le site localement

Cloner le dépôt :

- soit via [le bouton vert sur le dépôt avec GitHub Desktop](https://github.com/geotribu/website). Dans ce cas-là, ouvrez un terminal Powershell dans le dossier décompressé et passez à l'étape suivante.
- soit avec les commandes ci-dessous :

```sh
cd ~/git-repos/geotribu/
git clone https://github.com/geotribu/website.git
```

Ce qui donne :

```sh
Clonage dans 'website'...
remote: Enumerating objects: 46677, done.
remote: Counting objects: 100% (1754/1754), done.
remote: Compressing objects: 100% (170/170), done.
remote: Total 46677 (delta 840), reused 1705 (delta 827), pack-reused 44923
Réception d'objets: 100% (46677/46677), 59.82 Mio | 10.49 Mio/s, fait.
Résolution des deltas: 100% (36595/36595), fait.
```

!!! question "Et le SSH alors ?!"
    Pourquoi je ne mentionne pas la possibilité de cloner par SSH ?
    Parce-que si vous vous posez cette question, c'est que vous n'avez pas du tout besoin de lire cette partie-là car, félicitations : vous avez un niveau en Git supérieur à un *quickstart* ! :partying_face:

### Configurer Git

Il s'agit d'indiquer le nom et l'adresse email qui seront utilisés pour les commits. Par exemple, si vous vous appelez Mona Lisa :

```bash
# on se déplace dans le dossier qui contient le sous dossier caché '.git'
cd website
git config user.name "Mona Lisa"
git config user.email "mona.lisa@devinci.com"
```

### Mettre à jour son dépôt local

Vérifier que votre dépôt local (sur votre ordinateur) soit à jour par rapport au dépôt central (sur GitHub) :

```bash
git fetch --prune
git status
```

Ce qui donne :

```sh
Sur la branche master
Votre branche est à jour avec 'origin/master'.

rien à valider, la copie de travail est propre
```

Si la commande `git status` ne vous renvoie pas le même genre de message qu'au-dessus, cela signifie que vous n'êtes pas à jour. Il faut alors faire :

```bash
git pull origin --prune
```

Après qu'une branche ait été fusionnée (*merged*), elle est automatiquement supprimée sur le dépôt central (hébergé sur [GitHub]) afin de garder un dépôt propre et lisible. Il faut alors mettre à jour le dépôt local sur votre machine :

=== "Debian (Bash)"
    ```bash
    git pull origin

    # mettre le dépôt local en conformité avec le dépôt central (notamment en supprimant les branches locales déjà supprimées sur GitHub)
    git remote prune origin

    # supprimer les branches qui ont été fusionnées - sauf master et gh-pages
    git branch --merged | grep -i -v -E "main|master|gh-pages"| xargs git branch -d

    # supprimer les branches qui n'existent plus sur GitHub
    git fetch --prune && git branch -v | grep -i -E "\[disparue|gone\]" | grep -v -E "\*|main|master|gh-pages" | awk '{print $1}' | xargs git branch -D
    ```

=== "Windows (Powershell)"
    ```powershell
    git pull origin

    # mettre le dépôt local en conformité avec le dépôt central (notamment en supprimant les branches locales déjà supprimées sur GitHub)
    git remote prune origin

    # supprimer les branches qui ont été fusionnées - sauf master et gh-pages
    git branch --merged | Select-String -Pattern '^(?!.*(main|master|gh-pages)).*$' | ForEach-Object { git branch -d $_.ToString().Trim() }

    # ou en ouvrant une fenêtre de sélection des branches à supprimer
    git branch --format "%(refname:short)" --merged  | Out-GridView -PassThru | % { git branch -d $_ }
    ```

----

## Python

![logo Python](https://cdn.geotribu.fr/img/logos-icones/programmation/python.png "logo Python"){: .img-thumbnail-left }

Pour éditer localement et visualiser le résultat final avant de publier sur le dépôt, il faut installer [Python] 3.10 ou supérieure et les dépendances du projet.

### Installation de Python

=== "Debian (Bash)"
    ```bash
    # lister les versions de Python installées
    ls -1 /usr/bin/python* | grep '[2-3].[0-9]$'

    # si aucune version de Python >= 3.10 n'est installée, installons la 3.10 par exemple
    sudo apt install software-properties-common
    sudo add-apt-repository ppa:deadsnakes/ppa
    sudo apt update
    sudo apt install python3.10
    ```

=== "Windows (Powershell)"
    Le mieux est encore de suivre l'article dédié :

    [Python : installation et configuration sur Windows :fontawesome-brands-windows:]({{ config.extra.geotribu_main_site }}articles/2020/2020-06-19_setup_python/#installation-et-configuration){: .md-button }
    {: align=middle }

### Création de l'environnement de travail

Pour travailler tranquillement sans risquer de casser quoi que ce soit dans l'installation de Python au niveau du système, on préfère utiliser un environnement virtuel.

=== "Debian (Bash)"
    ```bash
    # se rendre à la racine du dépôt local - adapter au dossier dans lequel vous avez cloné le dépôt
    cd ~/git-repos/geotribu/website/

    # créer un environnement virtuel
    python3 -m venv .venv
    source .venv/bin/activate

    # mettre à jour pip et les outils de packaging
    python -m pip install -U pip
    python -m pip install -U setuptools wheel
    ```

=== "Windows (Powershell)"
    Si ça n'est pas encore fait, il faut [autoriser l'utilisation des environnements virtuels]({{ config.extra.geotribu_main_site }}articles/2020/2020-06-19_setup_python/#autoriser-lutilisation-des-environnements-virtuels).  
    Puis :

    ```powershell
    # se rendre à la racine du dépôt local - adapter au dossier dans lequel vous avez cloné le dépôt
    cd ~/git-repos/geotribu/website/

    # lister les versions de Python installées
    py --list

    # créer un environnement virtuel - Attention : ne fonctionne pas avec Python installé depuis le Windows Store
    py -3.10 -m venv .venv  

    # activer l'environnement virtuel
    .\.venv\Scripts\activate

    # mettre à jour pip
    python -m pip install -U pip
    python -m pip install -U setuptools wheel
    ```

### Installer les dépendances du site

Toujours dans l'environnement virtuel, il s'agit maintenant d'installer les dépendances (Mkdocs, thème, plugins...) requises pour [générer le site](../internal/generer_les_sites_web_geotribu.md) localement.

La liste des packages à installer dépend de l'accès à la version payante du thème [Material for Mkdocs](https://squidfunk.github.io/mkdocs-material/insiders/).

=== "Version gratuite du thème"

    ```sh
    python -m pip install -U -r requirements-free.txt
    ```

=== "Version payante du thème (jeton *Insider*)"

    Ajouter le jeton d'accès en variable d'environnement : `GH_TOKEN_MATERIAL_INSIDERS`. Par exemple avec Bash :

    ```sh
    export GH_TOKEN_MATERIAL_INSIDERS=ghp_************
    ```

    ou avec PowerShell :

    ```powershell
    $env:GH_TOKEN_MATERIAL_INSIDERS='ghp_************
    ```

    Puis lancer l'installation des dépendances :

    ```sh
    python -m pip install -U -r requirements-insiders.txt
    ```

<!-- markdownlint-enable MD046 -->

Les dépendances du projet sont mises à jour mensuellement. Il est donc recommandé de mettre son environnement virtuel local à jour **avant** de contribuer, en relançant la commande ci-dessus.

----

## Pre-commit

![logo pre-commit](https://cdn.geotribu.fr/img/logos-icones/programmation/precommit.png "logo pre-commit"){: .img-thumbnail-left }

Le projet vient avec une [configuration](https://github.com/geotribu/website/blob/master/.pre-commit-config.yaml) pour [pre-commit], qui permet d'appliquer des scripts (des [*git hooks*](https://git-scm.com/book/fr/v2/Personnalisation-de-Git-Crochets-Git)) de vérification et de nettoyage des fichiers avant qu'ils ne soit enregistrés dans le dépôt (d'où le nom).

L'installation est optionnelle mais recommandée car l'outil garantit :

- un socle minimal de qualité des contenus et codes sources
- une cohérence d'ensemble entre les contributions
- qu'une fois poussée sur le dépôt central, la contribution passe [les checks exécutés dans la CI](https://results.pre-commit.ci/repo/github/248722492).

[En savoir plus sur les crochets Git :material-hook:](../internal/git_hooks_precommit.md){: .md-button }
{: align=middle }

Installer [pre-commit] :

```bash
# depuis l'intérieur de l'environnement virtuel
pre-commit install
```

Une fois installés, les scripts s'exécuteront à chaque commit. Ne pas se laisser impressionner par les messages verbeux :wink: :

```bash
> git commit

[WARNING] Unstaged files detected.
[INFO] Stashing unstaged files to C:/Users/mPokora/.cache/pre-commit/patch1588143245.
Trim Trailing Whitespace.............................(no files to check)Skipped
Detect Private Key...................................(no files to check)Skipped
Fix End of Files.....................................(no files to check)Skipped
Check Yaml...........................................(no files to check)Skipped
Check for added large files..........................(no files to check)Skipped
[INFO] Restored changes from C:/Users/mPokora/.cache/pre-commit/patch1588143245.
```

Il est également possible de tous les exécuter manuellement :

```bash
pre-commit run -a
```
