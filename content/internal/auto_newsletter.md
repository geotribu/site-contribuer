---
title: "Newsletter automatique"
subtitle: No spam, no cry
authors:
    - Julien MOURA
categories:
    - article
    - meta
comments: true
date: 2023-02-01
description: "Sous le GéoCapot : comment fonctionne la newsletter automatique du site Geotribu."
icon: material/email-newsletter
image: https://cdn.geotribu.fr/img/internal/newsletter/newsletter_mailchimp_template.png
robots: index, follow
tags:
    - coulisses
    - GéoCapot
    - newsletter
---

# Lettre d'informations automatisée

![icône newsletter](https://cdn.geotribu.fr/img/logos-icones/divers/newsletter.webp "icône newsletter"){: .img-thumbnail-left loading=lazy }

En complément des autres modes de diffusion (réseaux sociaux notamment), la lettre d'informations par mail, ou plus simplement _newsletter_, reste un moyen très apprécié pour faire de la veille et pour suivre l'actualité d'un site. De plus, la revue de presse de Geotribu peut être considérée comme une forme de newsletter et correspond donc très bien à ce format de diffusion.

Pour ne pas disperser l'énergie nécessaire à l'effort éditorial de rédaction, relecture et publication des contenus, l'idée est d'automatiser l'envoi des mails au maximum. Et tant pis pour le contenu original marketing !

C'est pour cette raison que l'on a opté pour Mailchimp, plateforme certes propriétaire mais l'une des rares à proposer cette fonctionnalité sans exiger davantage de maintenance ou travail.

Différents éléments sont mis en place :

- le flux RSS (via le [plugin dédié pour Mkdocs](https://github.com/Guts/mkdocs-rss-plugin)) est un prérequis
- le formulaire d'inscription à la newsletter
- les groupes de contacts selon l'option de fréquence
- le modèle de mail qui gère la syntaxe de remplacement à partir du RSS
- les campagnes d'envoi

----

## Fonctionnement

La newsletter utilise en fait le flux RSS pour remplir automatiquement les contenus publiés depuis l'envoi précédent. Voici le déroulé global depuis la publication d'un (ou plusieurs) nouveau(x) contenu(s) sur le site et l'envoi des mails :

```mermaid
flowchart TB
    A[Ajout de contenus sur la branche principale] -->|mkdocs build| B("Génération du flux RSS")
    B -->|"Publication<br/>sur GitHub Pages"| C{"Site et flux RSS en ligne<br/>sur geotribu.fr"}
    C -->|Lundi suivant| D(Mailchimp consulte le flux RSS)
    C -->|Mois suivant| D
    D -->E["Pas de nouveau contenu, <br/>pas de mail"]
    D ===>F["Mailchimp itère sur chaque contenu<br/>publié depuis la dernière fois,<br/>et remplit le template"]
    F -->G["Envoi du mail au groupe mensuel"]
    F -->H["Envoi du mail au groupe hebdomadaire"]

    click C href "http://geotribu.fr/feed_rss_created.xml" "Ouvrir le flux dans un nouvel onglet"
```

----

## Modèle de mail et syntaxe de remplacement

![Mailchimp - Modèle de mail Geotribu](https://cdn.geotribu.fr/img/internal/newsletter/newsletter_mailchimp_template.png){: .img-center loading=lazy }

Voici à quoi ressemble le modèle Mailchimp avec les balises de remplacement des valeurs issues du flux RSS :

```html
*|RSSITEMS:|*
*|RSSITEM:TITLE|*
Publié le *|RSSITEM:DATE:d/m/Y|* par *|RSSITEM:AUTHOR|*

🏷 Mots-clés : *|RSSITEM:CATEGORIES|*

Illustration - *|RSSITEM:TITLE|*

<a href="*|RSSITEM:URL|*"><img alt="Illustration - *|RSSITEM:TITLE|*" loading="lazy" src="*|RSSITEM:ENCLOSURE_URL|*" title="Illustration - *|RSSITEM:TITLE|*" width="100%" /></a>
<br />

*|RSSITEM:CONTENT_FULL|*

Continuer la lecture » - Commenter » - Partager : *|RSSITEM:SHARE:Facebook,LinkedIn,Reddit,Twitter|*
*|END:RSSITEMS|*

Les autres contenus récents :
*|RSS:RECENT|*
```

----

## Fréquences de réception

Afin de proposer différentes fréquences de réception que l'abonné peut choisir, il faut créer [un groupe dans l'administration des contacts sur Mailchimp](https://us5.admin.mailchimp.com/lists/dashboard/groups?id=491210) de type radio (c'est-à-dire choix exclusif) :

![Mailchimp - Groupes de contacts Geotribu](https://cdn.geotribu.fr/img/internal/newsletter/newsletter_mailchimp_groupes_frequence.png){: .img-center loading=lazy }

Puis, il faut créer une campagne par option de fréquence. A date, nous en proposons deux :

![Mailchimp - Campagnes](https://cdn.geotribu.fr/img/internal/newsletter/newsletter_mailchimp_campagnes.webp){: .img-center loading=lazy }

----

## Ressources

- la [documentation officielle de Mailchimp sur la syntaxe RSS](https://mailchimp.com/fr/help/rss-merge-tags/)
