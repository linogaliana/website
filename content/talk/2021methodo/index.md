---
abstract: |
  Appariements flous de données textuelles en grande dimension. 
  Présentation au séminaire de la méthodologie à l'Insee
all_day: false
authors: ~
date: "2021-12-09"
date_end: "2021-12-09"
event: Séminaire de la méthodologie à l'Insee
#event_url: https://www.spyrales.fr/
featured: true
image:
  caption: "Appariements flous de données textuelles en grande dimension"
  focal_point: Right
links:
- icon: twitter
  icon_pack: fab
  name: Follow
  url: https://twitter.com/linogaliana
location: Paris
math: true
projects:
- RelevanC
publishDate: "2021-12-09"
summary: Une présentation de mon travail actuel pour apparier les noms de produits dans des données de caisse de grande dimension avec ceux disponibles dans l'OpenFoodFacts
tags: ['Elastic','python', "relevanC"]
title: Appariements flous de données textuelles en grande dimension.
slug: dsm-relevanc
---

L'objectif de cette étude est de proposer une cartographie fine des
comportements de consommation d'aliments gras, sucrés et salés. 

Pour cela, il est nécessaire d'utiliser des méthodes d'appariement
flous pour trouver, à partir de noms de produits dans les données
de supermarchés, le même produit dans
l'[`OpenFoodFacts`](https://fr.openfoodfacts.org/data).
Nous utilisons `ElasticSearch` pour effectuer ce travail.

La présentation est disponible [ici](https://epic-davinci-acb57b.netlify.app/#1).

Voici une vision partielle du *pipeline* mis en oeuvre pour mettre en 
relation les différentes sources:

![](pipeline-relevanc.png)

<!-----------
url_code: ""
url_pdf: ""
url_slides: ""
url_video: ""

{{% callout note %}}
Click on the **Slides** button above to view the built-in slides feature.
{{% /callout %}}

Slides can be added in a few ways:

- **Create** slides using Academic's [*Slides*](https://sourcethemes.com/academic/docs/managing-content/#create-slides) feature and link using `slides` parameter in the front matter of the talk file
- **Upload** an existing slide deck to `static/` and link using `url_slides` parameter in the front matter of the talk file
- **Embed** your slides (e.g. Google Slides) or presentation video on this page using [shortcodes](https://sourcethemes.com/academic/docs/writing-markdown-latex/).

Further talk details can easily be added to this page using *Markdown* and $\rm \LaTeX$ math code.
--------------->
