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

Geotribu c'est 1 nom de domaine et 3 suffixes, payés de longue date par Fabien :

- `geotribu.fr` pour le site principal et les sites secondaires en français
- `geotribu.net` pour le blog anglophone et les slides
- `geotribu.org` pour les éventuels outils liés à la dynamique de groupe ou communautaire. Inutilisé actuellement.

Ils sont tous enregistrés chez [Gandi](https://www.gandi.net/), un bureau d'enregistrement français.

Côté hébergement, la plupart des sites sont hébergés sur [GitHub Pages](https://pages.github.com/). Pour le reste :

- les [images](../guides/cdn-images-hebergement.md) sur le serveur prêté gracieusement par GeoRezo et géré principalement par Julien
- les sites de prévisualisation des PR sur [Netlify](https://www.netlify.com/)
- les serveurs gischat pour QChat sur un serveur géré principalement par Guilhem

## Autoriser GitHub Pages à utiliser le nom de domaine geotribu.fr

Il s'agit de suivre les instructions de [la documentation pour configurer un domaine personnalisé](https://docs.github.com/fr/pages/configuring-a-custom-domain-for-your-github-pages-site/verifying-your-custom-domain-for-github-pages#verifying-a-domain-for-your-organization-site). Concrètement, cela consiste à ajouter un enregistrement DNS de type `TXT` pour le domaine `geotribu.fr` avec une clé de vérification fournie par GitHub. Par exemple :

```zone
_github-pages-challenge-geotribu 10800 IN TXT "7am9zq..."
```

Pour le site principal, cela passe par un enregistrement DNS de type `CNAME` pour le domaine `www.geotribu.fr` vers `geotribu.github.io.` (notez le point final à la fin de l'enregistrement).

```zone
www 600 IN CNAME geotribu.github.io.
```

Il faut aussi ajouter les enregistrements pour les sous-domaines :

```zone
cli 10800 IN CNAME geotribu.github.io.
contribuer 10800 IN CNAME geotribu.github.io.
pyqgis-icons-cheatsheet 10800 IN CNAME geotribu.github.io.
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

De même pour le [blog anglophone](https://blog.geotribu.net/) ou les [slides](https://slides.geotribu.net/) qui sont hébergés sur GitHub Pages, il faut un enregistrement de type `TXT` pour le domaine `geotribu.net` avec une clé de vérification fournie par GitHub et un enregistrement DNS de type `CNAME` pour chaque sous-domaine de `geotribu.net` vers `geotribu.github.io.` :

```zone
blog 1000 IN CNAME geotribu.github.io.
slides 1000 IN CNAME geotribu.github.io.
```

## Autoriser MailChimp et isso (commentaires) à envoyer des mails pour geotribu.fr

La [newsletter](./auto_newsletter.md) et les notifications des commentaires sont envoyées par MailChimp et Isso avec l'adresse `facteur@geotribu.fr`. Pour ne pas être considéré comme spam, il faut être en conformité avec SPF, DMARC et DKIM. Pour rappel :

- SPF : autorise une IP à envoyer des mails pour un domaine
- DKIM : signature cryptographique des mails pour prouver qu’ils viennent bien de Geotribu
- DMARC : politique globale pour définir quoi faire si SPF/DKIM échoue

Voici la configuration DNS établie au printemps 2025 pour le domaine `geotribu.fr` :

```zone
@ 10800 IN MX 10 spool.mail.gandi.net.
@ 10800 IN MX 50 fb.mail.gandi.net.
@ 10800 IN TXT "v=spf1 include:_mailcust.gandi.net ip4:195.200.217.9 ip6:2a06:ac01:cafe:3217::9 ~all"
_dmarc 300 IN TXT "v=DMARC1; p=quarantine; rua=mailto:facteur+dmarc@geotribu.fr"
gm1._domainkey 1200 IN CNAME gm1.gandimail.net.
gm2._domainkey 1200 IN CNAME gm2.gandimail.net.
gm3._domainkey 1200 IN CNAME gm3.gandimail.net.
k2._domainkey 300 IN CNAME dkim2.mcsv.net.
k3._domainkey 300 IN CNAME dkim3.mcsv.net.
mandrill._domainkey 10800 IN TXT "v=DKIM1; k=rsa; p=MIG[...];"
```

## Ressources

- [Doc GitHub sur la gestion des domaines personnalisés](https://docs.github.com/fr/pages/configuring-a-custom-domain-for-your-github-pages-site/about-custom-domains-and-github-pages)
- [Consignes Google pour les expéditeurs de mails](https://support.google.com/a/answer/81126)
- [Formulaire Google pour indiquer qu'on est indûment considéré comme spam](https://mail.google.com/support/bin/request.py?contact_type=bulk_send&hl=fr)
- [Sécurité de la messagerie : SPF, DKIM et DMARC pour les débutants](https://www.it-connect.fr/securite-messagerie-spf-dkim-dmarc-pour-les-debutants/)
