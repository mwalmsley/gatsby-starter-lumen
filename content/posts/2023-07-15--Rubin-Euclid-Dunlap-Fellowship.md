---
template: post
title: Dunlap Fellowship Plans
slug: rubin-euclid-dunlap-fellowship
draft: False
date: '2023-07-15T23:46:37.121Z'
description: >-
    My Dunlap Fellowship plan for detailed morphology in Euclid and Rubin
category: research
tags:
  - euclid
  - rubin
  - deep learning
  - dunlap
  - fellowship
  - toronto
---

I'll be starting as a Dunlap Fellow at the University of Toronto from September 2023. I'm hyped to start officially running my own research program.

In this post, I'd like to share what I'm hoping to do. The truth is I can't do it on my own. If you're interested in building shared software tools across surveys, in crowdsourcing and human-AI collaboration, and of course in applying deep learning to answer science questions, then please reach out. I'd love to chat.

## Detailed Morphology for Euclid and Rubin

I want to measure the detailed morphology of every resolved galaxy in Euclid and Rubin.

I've spent the last four years building the tools to make this possible. The keystone is [Zoobot](www.github.com/mwalmsley/zoobot) ([paper](https://joss.theoj.org/papers/10.21105/joss.05312)). Zoobot trains models that accurately answer every question we've ever asked at Galaxy Zoo. These "foundation" models can then be adapted to new problems and new surveys using very little new labels. Other folks have adapted Zoobot models to find interacting galaxies in HSC and the Hubble archives, segment bars in SDSS, locate clumps in DESI galaxies, find interesting anomalies like strong lenses, and more.

Zoobot is effective because of the training data. It's not an especially large model - the current best-performing architecture is MaX-ViT Tiny, with 300M parameters, and larger architectures don't (currently) perform better on my benchmarks. But it's trained on a unique pretext task - 92M human clicks from Galaxy Zoo volunteers selecting an answer to a Galaxy Zoo question. Predicting the answer to any question is a broad task (like predicting the next word for any sentence). This broad task helps Zoobot learn a general representation that's easily adaptable to new questions.

My Fellowship plan is less about trying the very latest architectures (that's easy thanks to Zoobot's `timm` support) and more about thoughtfully including more data and more pretext tasks. Practitioners sometimes call this data-centric ML.

The outline below is simply Plan A for applying Zoobot to Euclid and Rubin. Perhaps you have better ideas - if so, tell me!

## Label-efficient Learning: Contrastive Learning, Active Learning, and Domain Adaption

I want to have a morphology catalog ready on the day of Euclid's DR1 release (Dec 2024, internally).

First, we can use contrastive learning to exploit unlabelled images. We've tested this on [DESI](https://arxiv.org/abs/2206.11927) and on [Radio Galaxy Zoo](https://arxiv.org/abs/2305.16127) and found that contrastive learning reduces the amount of labels needed to achieve a given performance. We did this with BYOL, and might next try some more recent frameworks like MAE/DINO.

Second, we can use active learning to select the most informative galaxies to label (from Euclid's preview release, DP1). I first tested this [back in 2019](https://academic.oup.com/mnras/article/491/2/1554/5583078?login=false) for SDSS galaxies (in collab. with Lewis Smith and Yarin Gal, from Oxford CS). Since then we've simplified the system and put it serverside on the Zooniverse. We ran it in anger from GZ HSC and we're now testing it again for GZ JWST.

Third, we can use domain adaption to make our models more robust to differences between surveys. Work by others, particularly [A. Ciprianovic](https://academic.oup.com/mnras/article-abstract/506/1/677/6296645) and [Y. Mes](https://home.strw.leidenuniv.nl/~mes/), shows domain adaption can be effecive for galaxy images. I haven't tried this yet!

## Going Beyond Classification: Segmentation, Personalised Anomaly Detection

We know we can build accurate classification models, especially with many (50k+) labelled galaxies. Our [DECaLS](https://ui.adsabs.harvard.edu/abs/2021AAS...23811902W/abstract) and DESI models are as accurate as 5 to 15 human volunteers. So let's up our ambition. Let's do segmentation.

Accurate classification models must be able to recognise morphological features within the image. So in some fundamental sense, they must "know" where those features are. Work by [P. Bhambra](http://dx.doi.org/10.1093/mnras/stac368) showed this for barred galaxy classifiers using SmoothCAM. Separate early work by A. Spindler shows UNet works straightforwardly on galaxy images. Let's extend our pretrained classifiers to do segmentation as well. I have a small grant from Meta to develop annotation tools with human-in-the-loop active learning (i.e. you start drawing on the image, and the model completes your brushstroke).

I'm especially keen to do segmentation for low surface brightness features. Unlike detailed morphology in general (bars, spiral arms, etc) accurately identifying LSB features is an unsolved problem. I think segmentation annotations will be much better for this than classification labels, because these would explicitly tell the model which small part of the image contains the LSB feature. I'm putting together a cross-survey collaboration to give this a try.

## Get It Right: Benchmarking, Annotators as Individuals

Visual galaxy morphology measurements from volunteers currently lacks the statistical rigor of, say, weak lensing inference. I think this is partly because, unlike weak lensing, we have historically not been able to simulate datasets where the right answer is known.
Comparing to expert labels has been used as a work-around - but there simply aren't many large expert morphology catalogs for modern (post-SDSS) surveys.
If we want visual morphology to be taken as a serious quantative galaxy property in the 21st century, we need to know how often we get the right answer. We could do this with modern galaxy-scale sims (such as FIRE) with [realistic synthetic images](https://github.com/cbottrell/RealSim) and particle/kinematic or new expert labels.

If we can create a set of galaxies with known "ground truth" labels, we can then adapt our pipeline design to best recover these. I'm especially interested in understanding how individual volunteers learn to answer different questions. When collecting labels we could add personalised advice and examples to guide volunteers who struggle on a question. And we could build models that understand that annotators (both volunteer and expert) as individuals with individual preferences.

## Closing Appeal

They say that man makes plans and God laughs. Perhaps I'll look back on this post as a moment of hubris. But I think everything I'm pitching here is technically feasible. If we can pull it off, we'll transform our understanding of why galaxies look the way they do. And along the way we'll enjoy a unique playground for building human-AI collaborative systems.

It's a lot to do. I'd love to put together a team of people interested in working on this together. If you'd like to join in, reach out - [mike.walmsley@dunlap.utoronto.ca](emailto:mike.walmsley@dunlap.utoronto.ca).
