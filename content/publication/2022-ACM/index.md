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
- name: Proceedings
  url: http://www.jms-insee.fr/2022/S28_2_ACTE_GALIANA_JMS2022.pdf
- name: Oral presentation
  url: http://www.jms-insee.fr/2022/S28_2_ACTE_GALIANA_JMS2022.pdf
- name: Paper and talk on the conference website
  url: http://www.jms-insee.fr/2022/S28_2_PPT_GALIANA_JMS2022.pdf
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
url_pdf: "http://www.jms-insee.fr/2022/S28_2_ACTE_GALIANA_JMS2022.pdf"
---

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

A temporary version of the research I lead with [Milena Suarez-Castillo](https://milenasuarezcastillo.netlify.app/) 
on the way we can use state-of-the-art NLP techniques to bring
together sources using food product names.

{{% callout note %}}
Working paper can be downloaded [there](http://www.jms-insee.fr/2022/S28_2_ACTE_GALIANA_JMS2022.pdf)
{{% /callout %}}

<object data="/pdf/JMS2022/S28_2_ACTE_GALIANA_JMS2022.pdf" type="application/pdf" width="700px" height="700px">
    <embed src="/pdf/JMS2022/S28_2_ACTE_GALIANA_JMS2022.pdf">
        <p>This browser does not support PDFs embedding. Please download the PDF to view it: <a href="http://www.jms-insee.fr/2022/S28_2_ACTE_GALIANA_JMS2022.pdf">Download PDF</a>.</p>
    </embed>
</object>



<!----
Supplementary notes can be added here, including [code and math](https://sourcethemes.com/academic/docs/writing-markdown-latex/).
------>
