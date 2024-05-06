---
title: Signer ses contributions
subtitle: Assumer son biopic
authors:
    - Geotribu
categories:
    - article
    - contribution
    - tutoriel
comments: true
date: 2020-08-04
description: "Contribuer à Geotribu : comment signer ses articles et contributions au site."
icon: fontawesome/solid/signature
tags:
    - auteur
    - autrice
    - paternité
    - signature
    - tutoriel
---

# Signer ses articles et contributions

## Page auteur·ice et bloc signature

Les contributeurs/rices doivent se créer une page contenant une présentation (biographie professionnelle plus ou moins liminaire) dans le dossier [`content/team/`](https://github.com/geotribu/website/new/master/content/team) en respectant le nommage des fichiers déjà créés, à savoir le prénom et le nom de famille sans caractères spéciaux et en utilisant le trait d'union (`-`) comme caractère de sépararation : `prenom-nom.md`.

Exemples de nommage :

- `jean-marc-viglino.md`
- `samuel-deschamps-berger.md`

<!-- markdownlint-disable MD046 -->
!!! question "Un doute sur le nommage ?"
    Les noms de fichiers sont déterminés par une fonction intégrée au [CLI Geotribu](https://cli.geotribu.fr/).  
    Il est donc possible de l'utiliser par vous-même pour déterminer le nom de votre fichier :

    ```python
    >>> from geotribu_cli.utils.slugger import sluggy
    >>> print(f"{sluggy('Élisabeth Amélie Eugénie en Bavière')}.md")
    elisabeth-amelie-eugenie-en-baviere.md
    ```
<!-- markdownlint-enable MD046 -->

### Bloc signature

Le bloc signature est une partie de la page auteur/ice qui est destinée à être intégrée en bas des articles et délimitée par deux commentaires :

```markdown
Cette partie-là ne s'affiche sur la page de l'auteur/ice.

<!-- --8<-- [start:author-sign-block] -->

Cette partie-là est intégrée comme signature en bas des articles de l'auteur/ice.

<!-- --8<-- [end:author-sign-block] -->

Cette partie-là ne s'affiche sur la page de l'auteur/ice.
```

Quelques règles encadrent cet usage :

- le bloc doit contenir le portrait
- le bloc ne doit pas contenir de titre de niveau 1 (`#`), 2 (`###`) ou 3 (`###`) dans le bloc signature

### Exemple

Prenons l'exemple d'une autrice s'appelant [Élisabeth Amélie Eugénie en Bavière](https://fr.wikipedia.org/wiki/%C3%89lisabeth_de_Wittelsbach). Elle crée donc un fichier `content/team/elisabeth-amelie-eugenie-en-baviere.md` contenant sa biographie et différents éléments servant aux mentions sur les réseaux sociaux :

```markdown title="Page autrice d'Élisabeth" linenums="1" hl_lines="20 30"
---
title: Élisabeth Amélie Eugénie en Bavière
subtitle: Princesse Sissi
categories:
    - contributeur
social:
    - bluesky:
    - github: https://github.com/psissi37
    - gitlab:
    - linkedin:
    - mail: p.sissi@empire-austro-hongrois.com
    - mastodon:
        - instance: mapstodon.space
        - username: princesse-sissi
    - twitter:
---

# Élisabeth Amélie Eugénie en Bavière

<!-- --8<-- [start:author-sign-block] -->

![Portrait Princesse Sissi](https://cdn.geotribu.fr/img/internal/contributeurs/psissi.png "Portrait Princesse Sissi"){: .img-thumbnail-left }

Je suis Élisabeth de Wittelsbach, mieux connue sous le nom de Sissi. Née le 24 décembre 1837 à Munich, en Bavière, j'ai toujours été fascinée par la beauté du monde qui m'entourait.
En plus de mes devoirs royaux, j'ai nourri rapidement une passion dévorante pour la cartographie,
utilisant le logiciel QGIS pour explorer et représenter le monde qui m'entourait.

À l'âge de 16 ans, j'ai épousé l'empereur François-Joseph Ier d'Autriche, devenant impératrice d'Autriche et reine de Hongrie. Malgré les défis de la vie à la cour, j'ai continué à cultiver ma passion pour la cartographie, trouvant du réconfort dans l'exploration des nouveaux horizons.

<!-- --8<-- [end:author-sign-block] -->

Ma vie a été marquée par des tragédies personnelles et des troubles de santé, mais ma dévotion à la cartographie m'a aidée à trouver la beauté et la vérité dans un monde souvent tumultueux.

Le 10 septembre 1898, ma vie a pris fin de façon tragique à Genève, en Suisse. Mais mon héritage en tant que cartographe et impératrice perdure, inspirant ceux qui cherchent à comprendre le monde qui nous entoure.
```

Notez les lignes surlignées correspondant aux commentaires encadrant le bloc signature.

Le plus simple reste encore de repartir des pages existantes :

[:material-github: S'inspirer des pages auteur/ices existantes sur Github](https://github.com/geotribu/website/tree/master/content/team){: .md-button }
{: align=middle }

### Syntaxe d'intégration dans l'article

L'intégration dans l'article consiste en deux éléments :

- [ ] le prénom et le nom dans [l'en-tête de l'article](./metadata_yaml_frontmatter.md#syntaxe "En-tête YAML des contenus Markdown")
- [ ] le commentaire d'intégration du/des bloc/s signature de/s auteur/ices

Si on poursuit avec Élisabeth, voilà à quoi ressemble l'en-tête de son article :

```markdown linenums="1" title="content/articles/2024/2024-05-06_ia-revolution-retour-experience-imperiale.md"
---
title: Mon précieux retour d'expérience sur une méthodologie d'IA révolutionnaire
subtitle: empireGPT
authors:
    - Élisabeth Amélie Eugénie en Bavière
[...]
---

[...]
```

Et en bas de l'article, on retrouve le commentaire qui intègre le bloc signature automatiquement :

```markdown linenums="289" title="content/articles/2024/2024-05-06_ia-revolution-retour-experience-imperiale"
[...]
<!-- geotribu:authors-block -->
```

----

## Signature automatique

En plus du bloc de signature, le site se repose sur l'extension [mkdocs-git-authors-plugin](https://github.com/timvink/mkdocs-git-authors-plugin) qui utilise l'historique Git pour déterminer qui a contribué à quelle page :

![Git authors plugin](https://cdn.geotribu.fr/img/internal/contribution/authoring/auto_from_git_log.png "Exemple de la liste des personnes ayant contribué à une page")

!!! warning "Précision importante"
    Ce système a été mis en place à partir de la refonte du site en mars 2020. Tous les contenus antérieurs ayant été récupérés et poussés sur GitHub par moi-même (Julien), c'est mon compte qui est indiqué, mais ça n'est évidemment pas moi qui aie rédigé tous les contenus !  
    Ainsi, pour tout contenu créé avant avril 2020, cette information ne reflète donc pas la contribution réelle à l'ensemble des contenus. Se référer au bloc auteur/e indiqué en bas du contenu.

### Pourcentage de contribution

Le pourcentage de contribution est proportionnel au nombre de lignes créées ou modifiées.

!!! note
    Le pourcentage de contribution par page n'est plus affiché depuis fin 2023, remplacé par les avatars Github des comptes ayant contribué.

### Informations remontées et personnalisation

Les informations (nom, adresse email) correspondent à la configuration locale de Git utilisées lors du commit. C'est l'adresse email qui fait office d'identifiant unique.

Si vous en utilisez plusieurs ou si vous souhaitez personnaliser le nom d'affichage, il est possible d'établir une table de correspondance en modifiant le fichier [.mailmap](https://github.com/geotribu/website/blob/master/.mailmap).

### Signer pour quelqu'un d'autre

Si la personne ayant contribué ne dispose pas d'un compte GitHub ou ne souhaite pas mettre les mains dans la mécanique de contribution, il est tout de même possible de lui attribuer au moment de faire le _commit_ :

```sh
git commit --author="Boris Mericksay <bmericskay@users.noreply.github.com>"
```
