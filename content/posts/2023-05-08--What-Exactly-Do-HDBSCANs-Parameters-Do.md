---
template: post
title: What exactly do HDBSCAN's parameters do?
slug: hdbscan-parameters
draft: False
date: '2023-05-08T23:46:37.121Z'
description: >-
    How HDBSCAN's parameters affect your clusters
category: guide
tags:
  - hdbscan
  - guide
---

The HDBSCAN docs have an excellent high-level overview of how the algorithm works. But they don't explain exactly how the HDSCAN code parameters fit in. This is my best effort at working that out.

  min_points

HDBSCAN constructs a graph linking all nearby points (single linkage tree). But we don't want to accidentally link two clusters with a single random noise point halfway between. To avoid this, we should try to move noise points further away from other points. That's done by defining a new distance measure - the mutual reachability distance.

  core distance: distance to min_point'th nearest neighbour
  mutual reachability distance: max(core(a), core(b), distance(a, b))

- If a or b is in a high density area, core(a) or core(b) is low and MRD=distance i.e. no effect.
- If a and b are in low density areas, core(a) and core(b) are high and MRD > distance

So the general effect of using MRD as a distance metric is to push points in low density areas further away from points in high density areas. Use this distance to safely construct the graph without accidentally including noise points.

As you increase `min_samples`, this increases how many points count as a high density area. Consider two points in a moderate density area. If `min_samples` is low, distance(a, b) > core(a) and core(b) and MRD=distance(a, b) i.e. MRD does nothing. But if `min_samples` is high, core(a) and core(b) > distance(a, b) and so MRD > distance(a, b) i.e. MRD is effectively pushing the points apart before constructing the graph. Greater `min_samples` means you need more neighbours to avoid having your graph distance (MRD) penalised.

Further, `min_samples` affects whether you're ultimately considered in a cluster at all.
Points are considered noise once the distance (edge weight) being dropped from the graph reaches that points core distance (i.e. distance to that point's min_point'th nearest neighbor).

<!-- singleton points are considered noise, and self-loop of core distance weight is added to every point. 

core distance (self-loop) can't be longer than MRD to any other points in the MST. If it has a single nearby point, MRD to that point is the core distance (greater than small actual distance) -->

  min_cluster_size

This is a bit more intuitive, thankfully.

We have the graph and now we're cutting edges to create clusters. Edges are cut in order of longest first. Clearly we can count the number of connected nodes as we go. But not every set of still-connected nodes should be considered a cluster - some will be just a few points falling away. When cutting the graph, how many connected points should count as a cluster?

If you make a cut and the now-split part of the graph includes more than `min_cluster_size` nodes, consider the cluster as split - we now have two clusters, each with more than `min_cluster_size` points. 

If, instead, fewer than `min_cluster_size` points were disconnected, consider that not a true split creating a new cluster - we just lost a few points in the course of refining our one cluster.

I hope this helps. Good luck!
