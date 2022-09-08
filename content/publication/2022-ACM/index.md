---
abstract: |

  Food retailers’ scanner data provide unprecedented details on local consumption, provided that product identifiers allow a linkage with features of interest, such as nutritional information.
  
  In this paper, we enrich a large retailer dataset with nutritional information extracted from crowd-sourced and administrative nutritional datasets. To compensate for imperfect matching through the barcode, we develop a methodology to efficiently match short textual descriptions.
  
  After a preprocessing step to normalize short labels, we resort to fuzzy matching based on several tokenizers (including n-grams) by querying an `ElasticSearch` customized index and validate candidates echos as matches with a Levensthein edit-distance and an embedding-based similarity measure created from a siamese neural network model. The pipeline is composed of several steps successively relaxing constraints to find relevant matching candidates.

authors:
- admin
- Milena Suarez-Castillo

# Author notes (optional)
author_notes:
  - 'Equal contribution'
  - 'Equal contribution'

doi: "10.1145/3524458.3547244"
featured: true
image:
  caption: "Siamese network performance"
  focal_point: ""
  preview_only: false
links:
- icon: twitter
  icon_pack: fab
  name: Follow
  url: https://twitter.com/linogaliana
- name: Proceedings
  url: https://dl.acm.org/doi/10.1145/3524458.3547244
- name: Oral presentation
  url: https://linogaliana.github.io/relevanc-goodIT22
projects:
- RelevanC
tags:
- NLP
- Deep Learning
- RelevanC
publication: ""
publication_short: ""
publication_types:
- "1"
publishDate: "2022-03-31"
summary: |

  Food retailers’ scanner data provide unprecedented details on local consumption, provided that product identifiers allow a linkage with features of interest, such as nutritional information.

  In this paper, we enrich a large retailer dataset with nutritional information extracted from Open Food Facts, completed with the `ANSES Ciqual` dataset. To compensate for imperfect matching through the bar code, we develop a methodology to efficiently match short textual descriptions. After a preprocessing step to normalize short labels, we resort to fuzzy matching based on several tokenizers (including n-grams) by querying an `ElasticSearch` customized index and validate candidates echos as matches with a Levenstein edit-distances. The pipeline is composed of several steps successively relaxing constraints to find relevant matching candidates.

  We finally develop a similarity based on a word embedding obtained by training a Siamese neural network on bar code matches. This alternative measure is used to evaluate our final matching.

title: "Fuzzy matching on big-data : an illustration with scanner data and crowd-sourced nutritional data"
url_pdf: "https://dl.acm.org/doi/pdf/10.1145/3524458.3547244"
url_slides: https://linogaliana.github.io/relevanc-goodIT22
---

Paper in the Association for Computing Machinery (ACM) Proceedings
is available at https://dl.acm.org/doi/10.1145/3524458.3547244 

## Purpose

To make the most of automatically collected scanner data for consumption studies, we link these products with crowd-sourced nutritional databases using textual search techniques. This approach requires the application of state-of-the-art textual analysis methods, including word embeddings, as well as efficient search tools to scale up.

Understand what is the nature, nutritional or environmental quality of food products consumed in supermarket will help to develop a sustainable and healthy consumption. The development of applications that provide information on products (nutritional characteristics, packaging, carbon footprint, etc.) opens up new perspectives on the analysis of scanner data at population scale once they have been matched. It is thus important to propose a method to associate these data sources that is reliable, flexible and efficient.

This work allowed us to evaluate the contributions and limitations of some NLP methods in a context where the textual data are noisy. In addition to the constructed database which can be used for multiple applications, one of the possibilities is to make available to the community the most efficient textual processing and matching models.

<object data="https://dl.acm.org/doi/pdf/10.1145/3524458.3547244" type="application/pdf" width="700px" height="700px">
    <embed src="https://dl.acm.org/doi/pdf/10.1145/3524458.3547244">
        <p>This browser does not support PDFs embedding. Please download the PDF to view it: <a href="https://dl.acm.org/doi/pdf/10.1145/3524458.3547244">Download PDF</a>.</p>
    </embed>
</object>

<!------ AUTRES OPTIONS POSSIBLES
url_code: '#'
url_dataset: '#'
url_pdf: "https://www.cairn.info/revue-idees-economiques-et-sociales-2015-2-page-14.htm"
url_poster: '#'
url_project: ""
url_slides: ""
url_source: '#'
url_video: '#'
slides: example
------>

