---
title: "Nepali Stemmer"
layout: post
date: 2020-04-15 10:10
image: /assets/images/nepstem.png
headerImage: true
star: false
projects: true
category: project
author: oyashi
description: "Nepali Stemmer"
---
This project is my third project in Nepali language. This is a project 
to perform morphological segmentation of a given word into stem and suffix.

### Abstract
Nepali language is morphologically rich language which makes it difficult to perform
any computational tasks unlike English. Therefore, morphological segmentation performs
a vital role in improving the downstream NLP tasks like (Named Entity Recognition, Sentiment 
Analysis, Coreference resolution and many others). In this posts, two methods are presented, one
is simple rule-based method while other one is based on non-parametric bayesian model.

Examples:
* *अमेरिकाद्वारा -> अमेरिका   द्वारा*
* *रामलाई -> राम   लाई*


## Methods
### Rule-based
This simple rule-based method is based on [hindi-stemmer](https://github.com/sainimohit23/hindi-stemmer).
It iteratively separates out the suffixes (postpositions) until no more separation can be processed.
We also need to provide the dictionary, stem and suffix list.

### Bayesian
This is an unsupervised non-parametric bayesian method which learns to segment stem and suffix from
a given training corpus. No extra resources like dictionary or any rules are required. This method is
based on Chinese Restaurant Process. At the heart of this method, we used beta-geometric conjugate prior over 
the length of a given word and used Gibbs sampling for inference purpose.


## Github
- [Rule-based](https://github.com/oya163/nepali-stemmer)
- [Bayesian](https://github.com/oya163/gibbs)


## Deployment
- [Rule-based](https://nepali-stemmer.herokuapp.com/)
    * A simple flask based web app deployed on Heroku platform
    * Created [PyPI package](https://pypi.org/project/nepali-stemmer/) for convenience
- [Bayesian](http://nepsegment.us-west-2.elasticbeanstalk.com/)
    * This is also a flask based web app but deployed on AWS Elastic Beanstalk following
    CI/CD pipeline using Github Actions
    * *Note: It's deployed version may not produce desired result as we need to play more with hyperparameters*

## References
- [goldwater-etal-2006-contextual](https://www.aclweb.org/anthology/P06-1085/)
- [snyder-barzilay-2008-unsupervised](https://www.aclweb.org/anthology/P08-1084/)
- [Suffix list](https://github.com/birat-bade/NepaliStemmer)
- [Nepali Dictionary](https://github.com/PraveshKoirala/stemmer)
- [Rule-based algorithm](https://github.com/sainimohit23/hindi-stemmer)


###### Final Note:
- This project is limited to single split segmentation and inflectional morphemes.
- Rule-based is a hobby project and Bayesian project is completed with the help of Sushil Awale 
during my summer internship in [NAAMII](https://www.naamii.com.np/).
- There are no publications for this project.