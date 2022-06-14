---
title: "Research"
template: "page"
---

## Overview

I try to solve scientific problems with machine learning. Much of my work has been on combining machine learning and crowdsourcing to do better science than with either alone. 
You can read about some of my work on the [TensorFlow blog](https://blog.tensorflow.org/2020/05/galaxy-zoo-classifying-galaxies-with-crowdsourcing-and-active-learning.html) and on this blog [here](https://walmsley.dev/posts/scaling-galaxy-zoo) and [here](https://walmsley.dev/posts/practical-galaxy-tools).

You can see my published research on [ORCID](https://orcid.org/0000-0002-6408-4181) and [Google Scholar](https://scholar.google.co.uk/citations?user=mfQ57x8AAAAJ&hl=en). I also have three accepted ICML'22 workshop papers to be presented shortly.

I am grateful to have been awarded funding by Meta (co-lead, "AI-assisted Soft Segmentation of Distant Galaxies by Citizen Scientists"), the Software Sustainability Institute ([Fellow](https://www.software.ac.uk/about/fellows/mike-walmsley) '22), and the Alan Turing Institute (Postdoctoral Enrichment Scheme '22).

## Principles

Whatever I work on, I find these principles helpful.


### Embrace Uncertainty

Science demands quantified uncertainty, on both our measurements and our predictions. Unfortunately, most machine learning methods don't deal well with uncertainty. Deep neural networks tend to be overconfident, for example, which prevents scientists from relying on them. But Bayesian deep learning can help mitigate this. And once we have trustworthy uncertainties, we can use them to do cool stuff like active learning.

### Design Systems, not SOTA

Much CS research focuses on showing state-of-the-art (SOTA) results on carefully constructed benchmarks.
However, as scientists, we require our methods to work reliably on real data. Real data is often very different to common benchmarks (highly uncertain, heteroskedastic, imbalanced, etc.) and so models that do well on benchmarks often fail in practice.
Collaborating with CS researchers to understand and tailor our methods is obviously helpful.
More broadly, I try to break free of the SOTA paper mentality and do what works. 
I aim to build systems that (hopefully) perform greater than the sum of their parts.
