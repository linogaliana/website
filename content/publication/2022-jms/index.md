---
abstract: |

  Food retailers’ scanner data provide unprecedented details on local consumption, provided that product identifiers allow a linkage with features of interest, such as nutritional information.

  In this paper, we enrich a large retailer dataset with nutritional information extracted from Open Food Facts, completed with the ANSES Ciqual dataset. To compensate for imperfect matching through the bar code, we develop a methodology to efficiently match short textual descriptions. After a preprocessing step to normalize short labels, we resort to fuzzy matching based on several tokenizers (including n-grams) by querying an ElasticSearch customized index and validate candidates echos as matches with a Levenstein edit-distances. The pipeline is composed of several steps successively relaxing constraints to find relevant matching candidates.

  We finally develop a similarity based on a word embedding obtained by training a siamese network on bar code matches. This alternative measure is used to evaluate our final matching.

authors:
- admin
- Milena Suarez-Castillo
doi: ""
featured: true
image:
  caption: "Low-income distribution at 6am"
  focal_point: ""
  preview_only: false
links:
- name: Proceedings
  url: http://www.jms-insee.fr/2022/S28_2_ACTE_GALIANA_JMS2022.pdf
- name: Paper and talk on the website
  url: http://jms-insee.fr/jms2022s28_2/
projects:
- PhoneSegregation
publication: ""
publication_short: ""
publication_types:
- "2"
publishDate: "2022-03-31"
summary: |

  Food retailers’ scanner data provide unprecedented details on local consumption, provided that product identifiers allow a linkage with features of interest, such as nutritional information.

  In this paper, we enrich a large retailer dataset with nutritional information extracted from Open Food Facts, completed with the ANSES Ciqual dataset. To compensate for imperfect matching through the bar code, we develop a methodology to efficiently match short textual descriptions. After a preprocessing step to normalize short labels, we resort to fuzzy matching based on several tokenizers (including n-grams) by querying an ElasticSearch customized index and validate candidates echos as matches with a Levenstein edit-distances. The pipeline is composed of several steps successively relaxing constraints to find relevant matching candidates.

  We finally develop a similarity based on a word embedding obtained by training a siamese network on bar code matches. This alternative measure is used to evaluate our final matching.

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

A temporary version of the research I lead with Milena Suarez-Castillo 
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
