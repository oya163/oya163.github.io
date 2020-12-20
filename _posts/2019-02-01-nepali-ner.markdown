---
title: "Nepali NER"
layout: post
date: 2019-02-01 00:00
image: /assets/images/nepner.jpg
headerImage: true
star: false
projects: true
category: project
author: oyashi
description: "Named Entity Recognition for Nepali Language"
---
This project is my initial project in Nepali language. As there were no
publicly available annotated dataset for Named Entity Recognition (NER), I created
a dataset following CoNLL English dataset standards.

### Abstract
NER has been studied
for many languages like English, German, Spanish, and others
but virtually no studies have focused on the Nepali language. One
key reason is the lack of an appropriate, annotated dataset. In
this paper, we describe a Nepali NER dataset that we created.
We discuss and compare the performance of various machine
learning models on this dataset. We also propose a novel NER
scheme for Nepali and show that this scheme, based on grapheme-
level representations, outperforms character-level representations
when combined with BiLSTM models. Our best models obtain
an overall F1 score of 86.89, which is a significant improvement
on previously reported performance in literature.


## Github
- The [code repo](https://github.com/oya163/nepali-ner/) is available on Github


## Web App
- A simple flask based <a href="https://nepner.herokuapp.com/" target="_blank">web app </a> is deployed on Heroku platform

## Publication
- This research is published in [IEEE CIC 2019](https://ieeexplore.ieee.org/document/8998477)
