---
title: "Research"
template: "page"
---

I try to solve scientific problems with machine learning. Much of my work has been on combining machine learning and crowdsourcing to do better science than with either alone. Read more on [this blog post](/posts/scaling-galaxy-zoo).

Whatever I work on, I find these principles helpful.


#### Embrace Uncertainty

Science demands quantified uncertainty, on both our measurements and our predictions. Unfortunately, most machine learning methods don't deal well with uncertainty. Deep neural networks tend to be overconfident, for example, which prevents scientists from relying on them. But Bayesian deep learning can help mitigate this. And once we have trustworthy uncertainties, we can use them to do cool stuff like active learning.

#### Design Systems, not SOTA

Much CS research focuses on showing state-of-the-art (SOTA) results on carefully constructed benchmarks.
However, as scientists, we require our methods to work reliably on real data. Real data is often very different to common benchmarks (highly uncertain, heteroskedastic, imbalanced, etc.) and so models that do well on benchmarks often fail in practice.
Collaborating with CS researchers to understand and tailor our methods is obviously helpful.
More broadly, I try to break free of the SOTA paper mentality and do what works. 
I aim to build systems that (hopefully) perform greater than the sum of their parts.
