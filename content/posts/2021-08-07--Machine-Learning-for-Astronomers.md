---
template: post
title: Getting Started with Deep Learning for Astronomers
slug: deep-learning-for-astro
draft: False
date: '2021-08-07T12:46:37.121Z'
description: >-
    How to start using deep learning for useful science
category: guide
tags:
  - deep learning
  - galaxies
  - quickstart
  - keras
  - pytorch
  - help
---

*Welcome - come on in. Sorry for the debris. Grab a chair - steal Nick's, he's always in the lab.*

*Your supervisor said you'll be using deep learning? But nobody actually told you how? Yes, you'll find that a lot around here. Let's see - what do you need to know...*

<!-- and asked if I could "get you started"? Y -->

## What are you trying to do?

It is tempting to take the latest, fanciest model design you can find on [paperswithcode](http://www.paperswithcode.com) and throw it at your problem.
If you are doing "CV-driven research", with the aim of moving into industry or just riding the deep learning hype, that's a perfectly fair approach - just be honest with your collaborators about what you're trying to achieve.
However, if you're trying to do good science, this will cause two headaches. First, fancy models have drawbacks; more code and more parameters make them generally slow, data-hungry, and hard to debug. Second, your paper will quickly be superceded by the next person using the inevitable slightly-fancier model.

A better strategy is to think carefully about what makes your problem hard, and then search for an approach that targets the hard parts. Maybe you are classifying faint tidal features in galaxy images, and so clever preprocessing might help. Maybe you have very little labelled data, and so you might look for ways to learn from unlabelled data or to intelligently choose which data to go out and label. Maybe the physics of the problem involves some symmetries or rules you can bake into your model. Some challenges will be unique to your astronomical context and this might be an opportunity to do some novel computer science research. When your method follows from your problem, your models will work better and your paper will stand out as thoughtful and original.

Most importantly, stay focused on the science question. The most common audience response at machine-learning-in-astro conference tracks is "So what?". You have to be able to say what your method will be able to tell us about the universe. Even if you don't have a single specific science question - perhaps you're aiming to make an automated catalog or optimize a telescope schedule - you should be clear on how those outcomes will support other people's science. This is the difference between making an impact in your field and spending a year carefully solving the wrong problem.

## What can I read?

The good news is that there are thousands of websites to learn from. The bad news is that there thousands of websites to learn from. Turns out, [anyone can make one](https://walmsley.dev). 

Books (remember them?) are surprisingly helpful. I suggest
[www.deeplearningbook.org](http://www.deeplearningbook.org). It's written by the inventor of GANs and strikes a good balance between words and math.

## What can I ignore at first?

Many blogs tell you all the things you need to learn. I found this overwhelming. Let me instead tell you what not to learn.

The big thing to ignore is math. You've made it through a Physics degree (probably) and that puts you ahead of most folks trying to pick up deep learning. You have all the rusty tools you need to get the basic math.

You should learn the *idea* of backpropogation (calculating first derivatives at each layer, then combining them with the chain rule ) but you don't have to work through the equations - that's what software is for. You should be able to explain to your supervisor how layers work (try at your own risk) but you don't need to write out the operations with Einstein notation.  You don't need to brush up on linear algebra (phew). 

Math is foundational and, like all foundations, is best left buried until an extension is needed.

The other big thing to ignore is deployment. Much of the online wisdom is written by engineers, for engineers.  They  want machine learning pipelines running on multiple GPUs, on the cloud, on Kubernetes, on microcontrollers, in the browser, inside your phone and behind your eyes. You need it to run on your laptop first. Most astronomers will probably never need it to run anywhere else.

This brings me to:

## Don't I need a GPU? 

Yes and no. 

You want to train models quickly so you can find out what works. The nightmare scenario is setting a model training, spending the rest of the day hiding from imposter syndrome on Netflix, waking up tomorrow to find it didn't work, and repeating until running out of either funding or sanity.

GPUs are unquestionably useful in shortening the cycle. Switching from a standard CPU to a standard gaming GPU might speed up your training by roughly a factor of five. But here are some things which will speed you up much more:

- Solving the right problem in the first place (see above)
- Using a "dummy" model to check your data pipeline and metrics
- Training for one epoch to make sure your model doesn't break
- Using live logging like [TensorBoard](https://www.tensorflow.org/tensorboard) to monitor your model and cancel training if it's going nowhere
- Using "early stopping", where training is cancelled if a model fails to improve over a set number of epochs, similarly

In short, train smart not hard.

## I'm ready for a GPU. How do I get access to one?

**Buy one.** Departments and supervisors apply for funding to carry out research, and this funding often includes [money for research equipment](). GPUs are useful (sometimes vital) equipment for your research; you may be able to pitch your supervisor or department to buy you one. You should be able to explain how it will help (tensors go brrr) and why the current computing resources aren't appropriate. It might feel like a lot to ask for as a lowly PhD student, and it might be, but it also might be less of a big deal than you think. If you don't ask, you'll never know. But do talk to your supervisor first.

**Share one**. Most Physics departments and universities will have compute clusters available to staff and graduate students. They grow organically by accreting hardware bought with grant money for particular projects. IT support can vary from "I've had a look at your submission script and fixed it for you, and by the way here's how to do better logging*" to "*obviously* you do it like *this*" to radio silence.

**Borrow one**.

Google has an excellent free resource called Collab. It is essentially Jupyter Notebook, but running on Google Cloud hardware instead of your computer. You launch a notebook, install the packages you want, off you go. The awkward part is moving data into the cloud so you can use it. One option is using GDrive. GDrive costs £2/month for 100GB and, since it's also hosted by Google, can be instantly attached to your notebook.

**Rent one**.

AWS and GCP will both rent you GPUs. They are quite cheap - around $0.40 an hour for "Spot" (i.e. may, rarely, be turned off at any time) reservations. However, you will also need to pay to store your data and probably your environment. This can add up over time and, if you aren't using the GPUs very much, might well cost more than the GPUs themselves. It's also the most complicated option to set up. If you want to go on and work in industry as a cloud engineer, it's a great CV building option, but for a quick project I would avoid it if possible.
## Which package should I use?

It doesn't matter.

You've got (sane) two choices; TensorFlow (by Google) and PyTorch (by Facebook & friends). They're very similar. Put briefly: training simple models is easier with TensorFlow, while loading unusual data and debugging are both easier with PyTorch. 

Both will let you write your own GPU-efficient functions in a style so teasingly close to numpy it'll constantly trip you up:

    np.mean(a)
    tf.reduce_mean(a)
    torch.mean(a)

    np.concat([a, b])
    tf.concat([a, b])
    torch.cat([a, b])

Both will let you define models and use them like scikit-learn estimators. TensorFlow is friendlier for this than PyTorch thanks to the Keras module*:

    from tensorflow import keras

    model = keras.Sequential([
        keras.layers.Conv2D(...)
        keras.layers.MaxPool(...)
        keras.layers.Dense(...)
    ])
    model.compile(optimizer='adam', loss='binary_crossentropy')
    model.fit(X, y)
    preds = model.predict(X)

*Told you the deep learning code would be the easy bit.*

PyTorch, on the other hand, requires two tutorials to show you how to [define](https://pytorch.org/tutorials/beginner/basics/buildmodel_tutorial.html) and [fit](https://pytorch.org/tutorials/beginner/basics/optimization_tutorial.html) a model. Fitting is a minefield; you should  **absolutely not** write the training loop for the model yourself. Let other people solve your problem by using the extension package PyTorch Lightning:

    from torch.utils.data import DataLoader
    import pytorch_lightning as pl

    train_loader = DataLoader(dataset)

    trainer = pl.Trainer()
    trainer.fit(model, train_loader)
    preds = model.predict(train_loader)

Ultimately, the most important difference is what your collaborators use.
Sharing tips and debugging help is crucial and much easier if you all use the same framework. 

If you are still not sure - use PyTorch. It's slightly more popular among computer scientists and so has slightly more of the latest and greatest models.
Now stop worrying about it and go do some science.



<!-- tf.data.Dataset and tf.keras

TensorFlow used to be tf.add(a, b) and it . 

Stay  away from TFRecords 

There are three different ways to write models

PyTorch, on the other hand

PyTorch has always been eager by default, which made it easier to debug more popular with CS researchers, so pick that if you want the 

Most of the code you write will be numpy and pandas.



If you haven't, Tensorflow

Tensorflow used to be a 

an originally-independent API for building models that TensorFlow liked so much they [ate it]():

*Shoutout  to... -->