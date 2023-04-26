---
title: "Vérification des hyperliens"
subtitle: 404/2 - 2
authors:
    - Julien MOURA
categories:
    - article
    - meta
date: 2023-04-26 19:20
description: "Sous le GéoCapot : comment on vérifie la syntaxe des liens HTTP (internes et externes) sur Geotribu, notamment avec LinkChecker."
icon : material/link-box-variant-outline
robots: index, follow
tags:
    - coulisses
    - GéoCapot
    - linkchecker
    - linter
    - Markdown
---

# Vérification automatisée de la syntaxe des liens web

![logo LinkChecker](https://cdn.geotribu.fr/img/internal/contribution/markdown/linkchecker_logo.png){: .img-rdp-news-thumb loading=lazy }

En Markdown, il est facile de faire des erreurs au moment d'insérer un hyperlien avec des questionnements HVE (Haute Valeur Existentielle) : s'il est [interne](/guides/markdown_quality/#liens-internes-relatifs), doit-il être relatif ou absolu ? Dois-je préciser le protocole ou bien cela est-il déterminé par le navigateur ?  
En plus de faire un mauvais choix, on s'expose aussi aux fautes de frappe qui mènent à un lien cassé. Et là c'est 404 drames !

C'est pourquoi une vérification des liens est exécutée régulièrement et automatiquement sur les contenus Geotribu, à l'aide de [linkchecker](https://linkchecker.github.io/linkchecker/), un utilitaire développé en Python et utilisable en ligne de commande (CLI).

----

## Configuration

Les possibilités de [configuration de l'outil sont impressionnantes](https://linkchecker.github.io/linkchecker/man/linkcheckerrc.html) ! Elles prennent place dans un fichier `.lincheckrrc` (format `.ini`) qui est à la racine du dépôt [Git] du site :

```ini title="Fichier .linkcheckrrc"
--8<-- "https://github.com/geotribu/website/raw/master/.linkcheckrrc"
```

Explication :

- on utilise jusqu'à 100 threads pour paralléliser les requêtes
- on ignore certains liens ou fichiers, ici les fichiers des flux RSS et modèles
- on indique le format et l'emplacement du fichier de sortie

----

## Utilisation en local

1. Disposer de l'environnement de travail en local : [voir cette page](/edit/local_edition_setup/)
1. Générer le site web de Geotribu
1. Installer le package :

    ```sh
    python -m pip install --upgrade "LinkChecker>=10"
    ```

1. Lancer LinkChecker :

    ```sh
    linkchecker build/mkdocs/site/ --config .linkcheckrrc --no-warnings
    ```

Si cela vous intéresse, déplier pour voir à quoi ressemble la sortie :

<!-- markdownlint-disable MD046 -->
??? example "Sortie de l'exécution"

    ```sh
    INFO linkcheck.cmdline 2023-04-26 18:09:03,788 MainThread Checking intern URLs only; use --check-extern to check extern URLs.
    LinkChecker 10.2.1
    Copyright (C) 2000-2016 Bastian Kleineidam, 2010-2022 LinkChecker Authors
    LinkChecker comes with ABSOLUTELY NO WARRANTY!
    This is free software, and you are welcome to redistribute it under
    certain conditions. Look at the file `LICENSE' within this distribution.
    Read the documentation at https://linkchecker.github.io/linkchecker/
    Écrivez des commentaires et rapports de bogue à https://github.com/linkchecker/linkchecker/issues

    Démarrage du contrôle à 2023-04-26 18:09:03+002
    39 threads active,     0 links queued,   41 links in  80 URLs checked, temps d'exécution 1 secondes
    WARNING bs4.dammit 2023-04-26 18:09:04,981 CheckThread-file:///home/username/Git/Geotribu/website/build/mkdocs/site/sitemap.xml.gz Some characters could not be decoded, and were replaced with REPLACEMENT CHARACTER.
    97 threads active,   859 links queued,  647 links in 1603 URLs checked, temps d'exécution 6 secondes
    96 threads active,  1052 links queued, 2726 links in 3874 URLs checked, temps d'exécution 11 secondes
    98 threads active,  1071 links queued, 3896 links in 5065 URLs checked, temps d'exécution 16 secondes
    94 threads active,  1292 links queued, 5252 links in 6638 URLs checked, temps d'exécution 21 secondes
    92 threads active,  1222 links queued, 6849 links in 8163 URLs checked, temps d'exécution 26 secondes
    92 threads active,  1157 links queued, 7851 links in 9100 URLs checked, temps d'exécution 31 secondes
    96 threads active,  1163 links queued, 9406 links in 10665 URLs checked, temps d'exécution 36 secondes
    96 threads active,  1064 links queued, 10792 links in 11952 URLs checked, temps d'exécution 41 secondes
    94 threads active,   988 links queued, 12035 links in 13117 URLs checked, temps d'exécution 46 secondes
    95 threads active,   929 links queued, 13447 links in 14471 URLs checked, temps d'exécution 51 secondes
    94 threads active,   889 links queued, 14764 links in 15747 URLs checked, temps d'exécution 56 secondes
    92 threads active,   844 links queued, 15762 links in 16698 URLs checked, temps d'exécution 1 minute, 1 secondes
    98 threads active,   795 links queued, 17003 links in 17896 URLs checked, temps d'exécution 1 minute, 6 secondes
    95 threads active,   671 links queued, 18219 links in 18985 URLs checked, temps d'exécution 1 minute, 11 secondes
    97 threads active,   602 links queued, 19167 links in 19866 URLs checked, temps d'exécution 1 minute, 16 secondes
    95 threads active,   526 links queued, 20057 links in 20678 URLs checked, temps d'exécution 1 minute, 21 secondes
    92 threads active,   206 links queued, 21456 links in 21754 URLs checked, temps d'exécution 1 minute, 26 secondes

    Statistiques :
    Downloaded: 80.9MB.
    Content types: 45 image, 1519 text, 0 video, 0 audio, 1750 application, 22 mail and 18902 other.
    URL lengths: min=8, max=932, avg=66.

    Fin. 22238 links in 22238 URLs checked. 0 avertissements trouvées.. 0 erreurs trouvées..
    Stopped checking at 2023-04-26 18:10:34+002 (1 minute, 30 secondes)
    ```

----

## Exécution automatisée sur la CI

![icône GitHub Actions](https://cdn.geotribu.fr/img/logos-icones/divers/github_actions.png "GitHub Actions"){: .img-rdp-news-thumb }

Pour soulager la charge mentale des gentils bénévoles, la vérification est exécutée automatiquement dans la CI et une notification est envoyée uniquement si des erreurs sont trouvées.  
Étant donné que l'exécution est assez longue et nécessite de générer tout le site avant, j'ai opté pour une exécution trimestrielle plutôt que sur chaque Pull Request ou commit.

Les [résultats des exécutions sont visibles sur le dépôt du site](https://github.com/geotribu/website/actions/workflows/links_checker.yml). Voici la configuration pour les curieux/ses :

<!-- markdownlint-disable MD046 -->
??? example "Workflow GitHub Actions"

    ```ini
    --8<-- "https://github.com/geotribu/website/raw/master/.github/workflows/links_checker.yml"
    ```

----

## Ressources

- LinkChecker dispose également d'une interface graphique : <https://github.com/linkchecker/linkchecker-gui>

----

## Remerciements et crédits

Merci à mon collègue [Thomas Muguet](https://tmuguet.me/) pour avoir mis cet outil en place pour le wiki interne à Oslandia et son aide pour accélerer la prise en main.
