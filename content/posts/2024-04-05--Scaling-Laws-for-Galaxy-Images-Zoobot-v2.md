---
template: post
title: Zoobot v2
slug: zoobot-scaling-laws
draft: False
date: '2024-04-05T09:00:00.000Z'
description: >-
    New foundation models for galaxy morphology
category: research
tags:
  - zoobot
  - foundation
  - euclid
  - deep learning
  - fellowship
  - toronto
---

## Zoobot 2.0: Scaling Up

> **Note:** This blog summarises the paper "Scaling Laws for Galaxy Images". Our pretrained foundation models are public as part of the [Zoobot 2.0](https://github.com/mwalmsley/zoobot) Python package. [Colab](https://colab.research.google.com/drive/1A_-M3Sz5maQmyfW2A7rEu-g_Zi0RMGz5?usp=sharing), [code](https://github.com/mwalmsley/zoobot), [docs](https://zoobot.readthedocs.io/en/latest/), [models](https://huggingface.co/collections/mwalmsley/zoobot-encoders-65fa14ae92911b173712b874), [embeddings](https://zoobot.readthedocs.io/en/latest/science_data.html#precalulated-representations).


All my best work has come from giving a talk, getting asked a good question, and replying with totally the wrong answer.

I spent the last couple of years making and then refining a set of AI models called Zoobot.
Zoobot is designed to be adapted to new galaxy tasks with minimal new labels.
I often call Zoobot a foundation model [^1].
But after my talks, people often say: "Zoobot works, but it's not really a *foundation* model. It's too tiny! Foundation models should have billions of parameters. What happens if you make it bigger?"

Like literally every AI researcher in the world, I enjoy training big models. *Of course* I tried making Zoobot bigger. It didn't help much. I might even go further and say

**"There is no evidence that bigger AI models work better in astronomy"**

It's clear that big models do better when fueled by limitless labelled data (think billions of web-scraped images with alt-text). But astronomers work with thousands of labelled images, not billions.
Even ImageNet, with 1.3M images, is about four times larger than the largest astronomy equivalent (GZ DESI). I think that's why there are no astronomy papers showing a convincing[^2] improvement from (say) large vision transformers.

But if data is the limiting factor - what if we scaled that as well?

## Pretraining Experiments

I spent (insert silly amount of time here) pulling together a dataset of 800k+ galaxies with 100M+ Galaxy Zoo volunteer annotations. That's more than half of all human galaxy annotations ever collected. Then I trained models to predict those annotations. I systematically worked through every popular architecture (ConvNexT, EffNetV2, MaxViT, etc.) and every model size, from 1M to 200M parameters. I then I did it twice more for error bars.

Here's what I found.

#### Adding data predictably improves performance for every model

We expect that more labels help. What's interesting is that the amount of improvement for each label added is *totally predictable*.
For every architecture, we see an almost-identical power law relationship between labels and performance.
This means we can predict how much our models will improve as we add new labels.

Each time we double the training data, we get the same improvement in performance. But we can't keep doubling training data; this paper used more than half of all the annotations ever collected. There's not much left to add!

#### Bigger models do better, up to a point

When we use a quarter of our dataset, training bigger models does almost no better: they overfit. But our full dataset provides enough labelled galaxies to meaningfully train models up to around 100M parameters.
Our best models (ConvNeXT-Base and MaxViT-Base) outperform the small (5M) EfficientNetB0 model used for GZ DECaLS and GZ DESI.

Beyond around 100M parameters, we start overfitting again. We could add some tricks like learning rate scheduling, aggressive regularization, etc., but it's clear that we can't scale indefinitely and expect better performance "for free".

#### More data helps every task, but more parameters help only some tasks

We can measure performance separately for each of our Galaxy Zoo questions (88 in total). Adding data improves performance on every question. Remember the power law above? That applies to individual questions! But adding parameters only helps for some questions (possibly the more subjectively challenging ones).

This is subtly different from the typical view that models are either data-limited or parameter-limited. Our models are data-limited for *all* questions and parameter-limited for *some* questions.

## Finetuning Experiments

I then finetuned our models on five real scientific tasks: finding and characterising faint tidal features, classifying JWST galaxies, detecting ringed galaxies, and (okay, slightly less real) galaxy morphology in Galaxy10 DECaLS.

#### We can do well at new tasks with just hundreds to thousands of labelled examples

For every new task, the galaxy-pretrained models did better than models pretrained only on ImageNet (31% better, on average). They were good enough to do useful science on every task.

Galaxy pretraining was most helpful when finetuning labels were scarce. This is exactly what Zoobot is for!

#### For new tasks, pretraining on galaxy images is more important than using big models

Bigger models did measurably better at the finetuning tasks, but the improvement was much smaller than from galaxy pretraining.
A large ImageNet-trained model is much worse than a small galaxy-pretrained model, and a large galaxy-pretrained model only adds a little extra benefit.

That suggests that if you want good performance on galaxy images, you should focus on using a similar pretraining dataset rather than downloading the biggest possible model and hoping it works.

## Where Next?

Alexander wept when there were no more hills to conquer, and I weep now I'm out of human labels. Bigger models will require other techniques.

Self-supervised learning is an obvious choice. I and my collaborators have been experimenting with this for a couple of years (e.g. [here](https://arxiv.org/pdf/2110.12735.pdf), [here](https://academic.oup.com/mnras/article/514/2/2599/6575929)). Scientific foundation models are attracting [ever](https://polymathic-ai.org/blog/announcement/) [more](https://arxiv.org/abs/2306.00258) [interest](https://arxiv.org/abs/2309.06126) and are still in their infancy in astronomy. I'm excited to see what we build together.

If you'd like to join in, reach out - [mike.walmsley@dunlap.utoronto.ca](emailto:mike.walmsley@dunlap.utoronto.ca). I'm also part of the [deepskies](https://deepskieslab.com/) and [universeTBD](https://universetbd.org/) open AI communities.

---

I'm grateful to Prof. Anna Scaife for encouraging me in building Zoobot, even when it meant spending more time writing code than papers. It was a gamble and I'm glad it's paying off. I'm also grateful to the Dunlap Institute for continuing to support my work building these tools for the community and applying them to Euclid.

---

[^1]: In the [first](https://arxiv.org/abs/2110.12735) Zoobot paper, I studiously avoided using the phrase `foundation model' because it was controversial at the time; some researchers felt it dangerously implied that AI models could be a solid foundation. Instead I just described what they are: "adaptable" models that "learn meaningful semantic representations of galaxies that are useful for new tasks on which the models were never trained".

[^2]: With the possible exception of [Daglit 2023](https://arxiv.org/abs/2304.05350), who recently trained a large CoAtNet-like model (~300M params) on Galaxy10 DECaLS (27k images) and hit about 94% top-1 accuracy. That's much better than baselines with typical models, and about the same as [another paper](https://ui.adsabs.harvard.edu/abs/2023arXiv231101500P/abstract) focused on using equivariance (and small models). I'm doubtful that the underlying Galaxy10 DECaLS labels are 94% accurate - but that's a topic for another blog.
