---
template: post
title: Hooks in PyTorch Lightning v2
slug: pytorch-lightning-v2-hooks
draft: False
date: '2023-03-19T23:46:37.121Z'
description: >-
    Updating hooks from Lightning v1 to v2
category: guide
tags:
  - lightning
  - guide
---

PyTorch Lightning v2.0 has made two major changes to `pl.LightningModule` methods:

`training_epoch_end(outputs)` methods have been removed in favor of `on_training_epoch_end()`.

Notice how the `_on` function is not quite the same: the `outputs` argument has been removed, requiring users to explicitly store outputs as a `self.` property. The [release notes](https://github.com/Lightning-AI/lightning/releases/tag/2.0.0) have a good example:

Before:

    class LitModel(L.LightningModule):
        
        def training_step(self, batch, batch_idx):
            ...
            return {"loss": loss, "banana": banana}
        
        # `outputs` is a list of all bananas returned in the epoch
        def training_epoch_end(self, outputs):
            avg_banana = torch.cat(out["banana"] for out in outputs).mean()  

Now:

    class LitModel(L.LightningModule):
        def __init__(self):
            super().__init__()
            # 1. Create a list to hold the outputs of `*_step`
            self.bananas = []
        
        def training_step(self, batch, batch_idx):
            ...
            # 2. Add the outputs to the list
            # You should be aware of the implications on memory usage
            self.bananas.append(banana)
            return loss
        
        # 3. Rename the hook to `on_*_epoch_end`
        def on_training_epoch_end(self):
            # 4. Do something with all outputs
            avg_banana = torch.cat(self.bananas).mean()
            # Don't forget to clear the memory for the next epoch!
            self.bananas.clear()

The idea is to make it obvious that using `epoch_end` actions requires all the batch-level outputs to be stored until the end of the epoch rolls around.

If you were using `training_epoch_end(outputs)` to update your `TorchMetric` metrics, as the [TorchMetrics docs suggest](https://torchmetrics.readthedocs.io/en/stable/pages/lightning.html) ([my issue](https://github.com/Lightning-AI/metrics/issues/1632)), this will now raise an error.

You might think you could replace the TorchMetrics updates in the similar `training_step_end(outputs)` methods. Unfortunately, and much less obviously from the release notes, these have also been removed - and will now silently do nothing.

Instead, you can use `on_train_batch_end(outputs)`. For example, in Zoobot:

    def on_train_batch_end(self, outputs, *args):
        self.train_loss_metric(outputs['loss'])
        self.log(
            "finetuning/train_loss", 
            self.train_loss_metric, 
            prog_bar=self.prog_bar, 
            on_step=False,
            on_epoch=True
        )

This appears to work well for multi-GPU training. It also avoids storing batch outputs in memory. Overall I'm grateful to the Lightning team for the new release - I hadn't noticed the previous memory-consuming behaviour before.

The Zoobot changes are now on `dev` and will be live on `main` and PyPI in the next few days.
