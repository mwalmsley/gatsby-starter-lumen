---
template: post
title: Paper: Practical Galaxy Morphology Tools
slug: practical-galaxy-tools
draft: False
date: '2021-10-21T23:46:37.121Z'
description: >-
    Using deep learning representations to solve new tasks
category: research
tags:
  - deep learning
  - galaxies
  - galaxy zoo
  - paper
---

*This blog is a summary of my paper “Practical Galaxy Morphology Tools from Deep Supervised Representation Learning”.*

What does a CNN learn? 

At heart, deep learning is all about learning to make useful representations. Models need to learn a function to map an image (say) onto a lower-dimensional vector that represents the content of the image. That representation can then be used to make predictions, often by feeding it to a few final dense layers.

Representations have recently become really important in the natural language (NLP) community. By training models to predict masked words in sentences, essentially all digital text becomes useful training data. This allows groups with enough cash and engineers to train ridiculously large models with billions to trillions of parameters. While there’s a degree of “mine is bigger than yours”, larger models are genuinely useful: thanks to all that training data, the representations they learn are excellent for fine-tuning to downstream tasks like summarising news articles or writing code.

Could such an approach work for astronomy? Self-supervised methods have made huge progress in the last year (see e.g. simCLR). That said, self-supervised methods (and unsupervised methods more broadly) are not informed by human labels and thus might learn representations that are not as scientifically meaningful as supervised methods. 

In my paper, I show that we can train excellent galaxy representations using a supervised approach. The key is Galaxy Zoo. Because the GZ questions are very broad, and my models have to learn to answer all those questions with the same representation, they end up learning a general representation that is useful for new tasks on which the model was never trained. 

This is much like how ImageNet pretraining is helpful for other terrestrial tasks because ImageNet is so broad. Pretraining on a broad galaxy task, however, works much better than a broad terrestrial task. I test this by trying to find ring galaxies using networks pretrained on GZ, pretrained on ImageNet, or trained from scratch. The GZ-pretrained network performs best. Interestingly, finetuning layers below the head is much less important than for the ImageNet-pretrained network, exactly as expected if the GZ representation is more useful.


<figure class="alignleft is-resized">
  <img src="https://galaxyzooblog.files.wordpress.com/2021/10/loss_by_rings-1.png?w=1024" alt="" class="wp-image-9572" width="456" height="319"/>
  <figcaption>Galaxies/day required to keep pace with upcoming surveys now, by 2019 year-end, and by 2022 year-end. Estimates from internal science plan.
  </figcaption>
</figure>

The obvious upshot is that you can finetune our GZ models to solve your own galaxy morphology tasks using just hundreds of labelled examples. Want to find irregular galaxies? You can do that. Close pairs? Probably! Tidal features? Go for it.

You can find code, documentation and pretrained models at https://github.com/mwalmsley/zoobot. I’ve spent a long time writing guides and minimal working examples for you to adapt. Everything is designed to be accessible to researchers with no previous deep learning experience. 

Our representations are also already useful to solve certain new tasks for which the models were never trained. I show two such tasks in the paper:  a similarity search, where you can find the most similar galaxies to a query galaxy, and a human-in-the-loop anomaly detection system, which uses the models’ understanding of similarity to learn which galaxies a user is personally most interested in. Both work very well thanks to the representations.

I think the future of ML for galaxy morphology will start to include the thoughtful sharing of representations. The jury is still out whether those representations are supervised, self-supervised, some combination of the two, or some method that hasn’t been found yet.
