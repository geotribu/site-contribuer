---
title: Noms de domaine
subtitle: Se présenter à l'enregistrement au moins 2h avant
authors:
    - Julien MOURA
categories:
    - article
    - meta
comments: true
date: 2025-06-20
description: "Sous le GéoCapot : gestion des noms de domaine."
icon: simple/gandi
robots: index, follow
tags:
    - coulisses
    - DNS
    - GéoCapot
    - hébergement
    - nom de domaine
---

# Gestion des noms de domaine

![icône globe flux](https://cdn.geotribu.fr/img/internal/icons-rdp-news/flux.png "icône globe flux"){: .img-thumbnail-left }

Geotribu possède 3 noms de domaines :

- `geotribu.fr` pour le site principal et les sites secondaires en français
- `geotribu.net` pour le blog anglophone et les slides
- `geotribu.org` inutilisé actuellement

Ils sont tous enregistrés chez [Gandi](https://www.gandi.net/), un bureau d'enregistrement français.

Côté hébergement, la plupart des sites sont hébergés sur [GitHub Pages](https://pages.github.com/). Pour le reste :

- les [images](../guides/cdn-images-hebergement.md) sur le serveur prêté gracieusement par GeoRezo et géré principalement par Julien
- les sites de prévisualisation des PR sur [Netlify](https://www.netlify.com/)
- les serveurs gischat pour QChat sur un serveur géré principalement par Guilhem

## Autoriser GitHub Pages à utiliser le nom de domaine geotribu.fr

Il s'agit de suivre les instructions de [la documentation pour configurer un domaine personnalisé](https://docs.github.com/fr/pages/configuring-a-custom-domain-for-your-github-pages-site/verifying-your-custom-domain-for-github-pages#verifying-a-domain-for-your-organization-site). Concrètement, cela consiste à ajouter un enregistrement DNS de type TXT pour le domaine `geotribu.fr` :

```zone
_github-pages-challenge-geotribu 10800 IN TXT "7am9zq..."
```

Pour le site principal, cela passe par un enregistrement DNS de type CNAME pour le domaine `www.geotribu.fr` vers `geotribu.github.io.` (notez le point final à la fin de l'enregistrement).

```zone
www 600 IN CNAME geotribu.github.io.
```

Il faut aussi ajouter les enregistrements pour les sous-domaines :

```zone
qtribu 10800 IN CNAME geotribu.github.io.
```

Pour tester, on utilise l'utilitaire `dig` :

```sh
> dig geotribu.fr +noall +answer -t AAAA
geotribu.fr.  600 IN AAAA 2606:50c0:8003::153
geotribu.fr.  600 IN AAAA 2606:50c0:8001::153
geotribu.fr.  600 IN AAAA 2606:50c0:8000::153
geotribu.fr.  600 IN AAAA 2606:50c0:8002::153
> dig geotribu.fr +noall +answer -t A
geotribu.fr.  600 IN A 185.199.108.153
geotribu.fr.  600 IN A 185.199.111.153
geotribu.fr.  600 IN A 185.199.110.153
geotribu.fr.  600 IN A 185.199.109.153
> dig www.geotribu.fr +nostats +nocomments +nocmd
;www.geotribu.fr.  IN A
www.geotribu.fr. 585 IN CNAME geotribu.github.io.
geotribu.github.io. 585 IN A 185.199.111.153
geotribu.github.io. 585 IN A 185.199.108.153
geotribu.github.io. 585 IN A 185.199.110.153
geotribu.github.io. 585 IN A 185.199.109.153
```

## Autoriser GitHub Pages à utiliser le nom de domaine geotribu.net

De même pour le [blog anglophone](https://blog.geotribu.net/) ou les [slides](https://slides.geotribu.net/) qui sont hébergés sur GitHub Pages, il faut un enregistrement DNS de type CNAME pour chaque sous-domaine de `geotribu.net` vers `geotribu.github.io.` :

```zone
blog 1000 IN CNAME geotribu.github.io.
slides 1000 IN CNAME geotribu.github.io.
```

## Autoriser MailChimp à envoyer des mails pour geotribu.fr

## Gérer l'envoi des notifications mails des commentaires

## Ressources

- [Doc GitHub sur la gestion des domaines personnalisés](https://docs.github.com/fr/pages/configuring-a-custom-domain-for-your-github-pages-site/about-custom-domains-and-github-pages)
- [Consignes Google pour les expéditeurs de mails](https://support.google.com/a/answer/81126)
- [Formulaire Google pour indiquer qu'on est indûment considéré comme spam](https://mail.google.com/support/bin/request.py?contact_type=bulk_send&hl=fr)
- [Sécurité de la messagerie : SPF, DKIM et DMARC pour les débutants](https://www.it-connect.fr/securite-messagerie-spf-dkim-dmarc-pour-les-debutants/)
