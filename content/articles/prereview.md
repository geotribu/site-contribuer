---
title: Pré-relecture d'un article
subtitle: PNC aux PR, armement des toboggans, vérification de la preview générée
authors:
    - Geotribu
categories:
    - contribution
comments: true
date: 2025-01-10
description: "Publication d'un article sur Geotribu : préparation de la relecture collaborative"
icon: octicons/checklist-24
image:
license: default
tags:
    - article
    - guide
    - relecture
    - workflow
---

# Préparation de la relecture collaborative

![Balai magique - Fantasia](https://cdn.geotribu.fr/img/logos-icones/divers/balai_magique_Fantasia.webp){ .img-thumbnail-left }

La relecture des articles proposés sur Geotribu est une étape essentielle du processus de publication. Avant de notifier tous les bénévoles, il est important de s'assurer que l'article est prêt à être relu. Cette pré-relecture permet de vérifier que l'article est correctement formaté, que les ressources nécessaires sont disponibles, que les informations essentielles sont renseignées et que l'auteur/ice est prêt/e à recevoir des retours.

Comme souvent, ce guide est principalement un mémo, pas une ablation du bon sens. Il est donc à adapter en fonction de l'article et de l'auteur/ice. S'inspirer des [articles précédemment publiés](https://github.com/geotribu/website/pulls?q=is%3Apr+label%3Aarticles+) est forcément une bonne idée.

## Check-list pour l'équipe Geotribu

- [ ] remercier l'auteur/ice pour sa proposition d'article
- [ ] proposer son aide à l'auteur/ice pour la mise en forme de l'article
- [ ] créer un dossier dédié à l'article sur le CDN
- [ ] faire parvenir les accès en écriture au CDN à l'auteur/ice par un moyen relativement sécurisé (message privé, mail à expiration, etc.)
- [ ] faire une première passe sur l'article pour corriger les éventuels points bloquant la génération de la prévisualisation :
    - [ ] s'assurer que le fichier de l'article est au bon endroit et   bien nommé
    - [ ] vérifier que l'en-tête YAML est correctement renseigné
    - [ ] vérifier que la page de profil de l'auteur/ice existe bien (voir [Signer ses articles](../guides/authoring.md))
- [ ] éditer la description de la PR avec les liens importants et les ressources nécessaires
- [ ] lancer la relecture collaborative en affectant [l'équipe Relecture](https://github.com/orgs/geotribu/teams/relecture) en review de la PR

![GitHub - Assigner l'équipe Relecture aux reviewers d'une Pull Request](https://cdn.geotribu.fr/img/internal/contribution/github_pull-request_assign_reviewers.webp){: loading=lazy .img-center }

----

## Modèles

![Fantasia - Magicien fainéant](https://cdn.geotribu.fr/img/logos-icones/divers/Mickey_endormi_Fantasia.webp){ .img-thumbnail-left }

Voici quelques modèles de messages à utiliser pour la relecture d'un article. Attention cependant, ces modèles sont à adapter en fonction de l'article et de l'auteur/ice.

!!! tip "Enregistrer ces modèles comme Saved Replies dans GitHub"
    Pour gagner du temps, il est possible d'enregistrer ces modèles de messages comme [Saved Replies](https://docs.github.com/en/github/writing-on-github/working-with-saved-replies) dans GitHub pour les utiliser facilement.

### Commentaire pour remercier l'auteur/ice

```markdown title="Commentaire à laisser sur la PR"
Merci pour la proposition d'article 🙏 !
```

----

### Description de la Pull Request

GitHub ne gérant qu'un modèle de description de PR et étant donné qu'il est utilisé pour les Revues de Presse, il faut remplacer le texte par défaut par celui ci-dessous.  
À noter que les lignes surlignées dans le bloc ci-dessous **sont à adapter** en fonction de l'article et de l'auteur/ice.

```markdown title="Texte à insérer dans la description de la PR, sous le texte de l'auteur/ice" hl_lines="8-11"

----

:information_source: Message automatique à lire et remplir :arrow_down:

## Liens importants

- pour modifier ton article via l'interface de GitHub,  tu peux le retrouver facilement via l'icône crayon en haut de la page sur le site de prévisualisation ([cf. documentation](https://contribuer.geotribu.fr/edit/fix_content_from_website/)). Sinon :
  - [article]()
  - [page auteur/ice]()
- le dossier dédié pour les illustrations de l'article sur notre "[CDN](https://contribuer.geotribu.fr/guides/cdn-images-hebergement/)" : <https://cdn.geotribu.fr/tinyfilemanager.php>. Si besoin des accès, contacter en message privé (Mastodon, mail, Slack,...)
- [site temporaire de prévisualisation de l'article]() - merci de ne pas le diffuser

## Ressources

Pour info, voici quelques extraits de notre guide de contribution :

- [comprendre et compléter l'en-tête](https://contribuer.geotribu.fr/guides/metadata_yaml_frontmatter/)
- [intégrer une image](https://contribuer.geotribu.fr/guides/image/)
- [choisir une licence](https://contribuer.geotribu.fr/guides/licensing/)
- [signer un article](https://contribuer.geotribu.fr/guides/authoring/)

## 📢 Diffusion

Une fois l'article publié, il sera alors temps de le diffuser. Il sera automatiquement intégré au [flux RSS](http://geotribu.fr/feed_rss_created.xml) et à [la newsletter](http://geotribu.fr/newsletter/signup/).

Mais la publication sur les réseaux sociaux est manuelle.

Nous avons des comptes officiels sur ces réseaux sociaux :

- [BlueSky](https://bsky.app/profile/geotribu.bsky.social)
- ~~[Facebook](https://www.facebook.com/geotribu) - inactif~~
- [LinkedIn](https://www.linkedin.com/company/geotribu/) avec [le hashtag #Geotribu](https://www.linkedin.com/feed/hashtag/?keywords=geotribu)
- [Mastodon](https://mapstodon.space/@geotribu)
- ~~[X/Twitter](https://twitter.com/geotribu)~~ - inactif

Merci d'indiquer en commentaire tes comptes à utiliser pour être cité/e dans les messages et de cocher ci-après la "stratégie" de diffusion qui te convient pour chaque réseau.

### BlueSky

- [ ] un/e membre de Geotribu publie, tu repartages avec ton compte
- [ ] tu publies, on repartage
- [ ] chacun/e publie de son côté
- [ ] je souhaite que mon contenu ne soit pas diffusé sur ce réseau

### LinkedIn

- [ ] un/e membre de Geotribu publie, tu repartages avec ton compte
- [ ] tu publies, on repartage
- [ ] chacun/e publie de son côté
- [ ] je souhaite que mon contenu ne soit pas diffusé sur ce réseau

### Mastodon

- [ ] un/e membre de Geotribu publie, tu repartages avec ton compte
- [ ] tu publies, on repartage
- [ ] chacun/e publie de son côté
- [ ] je souhaite que mon contenu ne soit pas diffusé sur ce réseau
```

----

### Commentaire pour initier la relecture collective

```markdown title="Commentaire pour lancer la relecture"
Allez zou, c'est parti pour la @geotribu/relecture  !

:information_source: **Aux relecteur/ices**

Merci d'utiliser autant que possible le mode **Suggestion** de la review : https://docs.github.com/fr/pull-requests/collaborating-with-pull-requests/reviewing-changes-in-pull-requests/reviewing-proposed-changes-in-a-pull-request#starting-a-review

![image](https://github.com/geotribu/website/assets/1596222/4d081e97-7290-48c4-b1ff-587a8920b508)

C'est vraiment **IMPORTANT** pour le confort de l'auteur/ice :pray:
```

----

### Structure du message Slack pour notifier l'équipe

L'objectif est d'ouvrir un fil de discussion interne par article soumis à relecture en résumant les informations essentielles. Le message est à publier dans le canal `#articles` et à adapter au sujet de l'article. Structure type :

```markdown title="Structure du message à l'équipe"
:gdal:  Nouvel article de Nicolas Rochard sur la génération de COG avec GDAL :filet_de_but:

- la PR : https://github.com/geotribu/website/pull/XXXX
- la preview : https://preview-pullrequest-XXXX--geotribu-preprod.netlify.app/

:date: 11 février

@canal à vos tipex et plus belles suggestions de correction !
```
