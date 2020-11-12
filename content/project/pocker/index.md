---
date: "2019-07-01"
external_link: ""
image:
  caption: A docker container integrating together R and Python
  focal_point: Smart
links:
- icon: github
  icon_pack: fab
  name: Github
  url: https://github.com/linogaliana/pocker
summary: A docker container integrating together R and Python (anaconda environment) that can be used in gitlab CI/CD
tags:
- docker
- Python
- CI/CD
title: PockeR
url_code: "https://github.com/linogaliana/pocker"
url_pdf: ""
url_slides: ""
url_video: ""
---

`PockeR` is a `docker` container with both `Python` and `R`. They can interact with `Reticulate`, either using `R` source files or `Rmarkdown` engine.

You can visit the following pages to know more about the project:

* [A blog post](/post/pocker/pocker-a-docker-container-to-use-r-and-python-together) I wrote that explains a little bit the philosophy

* [`github`](https://github.com/linogaliana/pocker) connected to `dockerhub` to build image base from `DockerFile`
* [`gitlab`](https://gitlab.com/linogaliana/pocker) with continuous integration using `/gitlabCI/.simple_configuration.yml` example file as a reproducible workflow
* [`dockerhub`](https://cloud.docker.com/u/linogaliana/repository/docker/linogaliana/pocker) that builds automatically from github repository the docker image

