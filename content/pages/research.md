---
title: "Research"
template: "page"
---

## Overview

I try to solve scientific problems with machine learning. Much of my work has been on combining machine learning and crowdsourcing to do better science than with either alone. 
You can find my published research on [ORCID](https://orcid.org/0000-0002-6408-4181) and [Google Scholar](https://scholar.google.co.uk/citations?user=mfQ57x8AAAAJ&hl=en). 

Machine learning is only useful if it's applied to real data.
I spend more than half my time building end-to-end data pipelines to enable data-centric ML.
I also spend a lot of time trying to design ML systems which are effective at solving a scientific task and yet simple enough to run reliably in production. 
<!-- In industry terms, I would say I'm somewhere between a machine learning researcher and a machine learning engineer. -->

## Foundation Models with Galaxy Zoo

I am the lead data scientist for [Galaxy Zoo](https://www.galaxyzoo.org), a citizen science project where hundreds of thousands of volunteers classify images of millions of galaxies. 

I build machine learning systems that collaborate with our volunteers to label galaxies hundreds of times faster. My deep learning models learn to [accurately predict](https://arxiv.org/abs/2102.08414) what volunteers would say for 1.5 million galaxies. They can then be adapted to solve new tasks with little (if any) additional labels.

These pretrained foundation models are available to all astronomers via my open-source package [Zoobot](https://github.com/mwalmsley/zoobot). Zoobot is part of the software pipeline for space telescope Euclid, launching in July 2023.

Please find more details on [this blog](https://walmsley.dev/posts/practical-galaxy-tools) and [this ICML workshop paper](https://arxiv.org/abs/2206.11927).

<!-- 
Two techniques I've found important for training effective foundation models are hybrid contrastive learning and active learning.

### Hybrid Contrastive Learning

[this blog](https://walmsley.dev/posts/galaxy-foundation-models)

### Active Learning

[TensorFlow blog](https://blog.tensorflow.org/2020/05/galaxy-zoo-classifying-galaxies-with-crowdsourcing-and-active-learning.html)

[here](https://walmsley.dev/posts/scaling-galaxy-zoo) production -->

## Mysterious Bursts from Space with CHIME

I am the Principal Investigator of Bursts from Space. We're searching for fast radio bursts using citizen scientists and machine learning. We've investigated more than 55,000 candidate signals in our search and have made some intruiging finds which I hope to share soon.

## Funding

I started and am a co-lead of the €150,000 European Space Agency grant "Exploring the evolution of galaxy morphology in different environments with Zoobot" (2022), funding a two year postdoc to apply my software to ESA's archives.

I started and am co-lead of the $29,000 Meta (Facebook) grant "AI-assisted Soft Segmentation of Distant Galaxies by Citizen Scientists", funding a software developer to help me create efficient methods for semantic segmentation.

I am grateful to have been also awarded funding by the Software Sustainability Institute ([Fellow](https://www.software.ac.uk/about/fellows/mike-walmsley) '22), and the Alan Turing Institute (Postdoctoral Enrichment Scheme '22).

<!-- ## Principles

Whatever I work on, I find these principles helpful.


### Embrace Uncertainty

Science demands quantified uncertainty, on both our measurements and our predictions. Unfortunately, most machine learning methods don't deal well with uncertainty. Deep neural networks tend to be overconfident, for example, which prevents scientists from relying on them. But Bayesian deep learning can help mitigate this. And once we have trustworthy uncertainties, we can use them to do cool stuff like active learning.

### Design Systems, not SOTA

Much CS research focuses on showing state-of-the-art (SOTA) results on carefully constructed benchmarks.
However, as scientists, we require our methods to work reliably on real data. Real data is often very different to common benchmarks (highly uncertain, heteroskedastic, imbalanced, etc.) and so models that do well on benchmarks often fail in practice.
Collaborating with CS researchers to understand and tailor our methods is obviously helpful.
More broadly, I try to break free of the SOTA paper mentality and do what works. 
I aim to build systems that (hopefully) perform greater than the sum of their parts. -->
