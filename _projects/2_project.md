---
layout: page
title: "Spatio-Temporal Scene Graph Generation from Image Sequences"
description: A neural architecture that generates structured scene graphs from multi-frame RGB observations, using temporal context to improve object and relationship prediction on tabletop manipulation scenes.
img: # assets/img/scene_graph_teaser.jpg
importance: 2
category: wip
related_publications: # comma-separated bib keys once published
selected: false
---

<!-- TODO: teaser — uncomment and fill in path -->
<!--
<div class="row mt-3">
  <div class="col-12">
    {% include figure.html path="assets/img/scene_graph_teaser.jpg" title="Scene graph generation overview" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
  Given a sequence of RGB frames, the model predicts a scene graph encoding object categories and spatial relationships (e.g., <em>on_top_of</em>, <em>left_of</em>).
</div>
-->

## Overview

Robots operating in tabletop environments need to understand not just what objects are present, but how they relate to each other — which block is on top of which, which cup is to the left of another. Scene graphs provide exactly this: a structured representation where nodes are objects and edges are labeled spatial or semantic relationships. This project extends scene graph generation from single images to **temporal sequences**, letting the model exploit consistency across frames to resolve ambiguities that would be undetectable from any one view.

The system takes a sequence of RGB frames as input and produces a scene graph with predicted object categories and directed relationship predicates (e.g., `in_front_of`, `behind`, `on_top_of`, `right_of`). It is evaluated on synthetic tabletop scenes from NVIDIA IsaacLab and the STOW (Semantic Tabletop Object Workspace) benchmark.

<!-- TODO: system diagram -->
<!--
<div class="row mt-3">
  <div class="col-12">
    {% include figure.html path="assets/img/sg_architecture.jpg" title="Architecture diagram" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
  Architecture overview: visual features are extracted per-frame with DINO ViT, aggregated temporally with BiLSTM, then refined through iterative message passing to produce node and edge predictions.
</div>
-->

---

## Approach

**Feature extraction.** Each frame is processed by a pre-trained DINO Vision Transformer (or ResNet FPN), and ROI-Align crops isolate per-object and per-edge-candidate regions. This produces rich visual features grounded to bounding box proposals.

**Temporal encoding.** A BiLSTM aggregates features across the sequence, allowing the model to accumulate evidence from multiple viewpoints. Latent dimension, depth, and dropout rate are all tunable via Hydra configuration.

**Spatial reasoning.** A message-passing module (inspired by Xu et al.) iteratively refines node and edge representations through GRU updates and adaptive pooling, propagating relational context across the full graph.

**Output.** MLPs classify each node (object category) and each directed edge (spatial predicate), and a regression head predicts relative 3D offsets between object pairs.

<div class="row mt-3">
  <div class="col-sm mt-3 mt-md-0">
    <!-- TODO: example input sequence -->
    <!--
    {% include video.html path="assets/video/sg_input_sequence.mp4" class="img-fluid rounded z-depth-1" autoplay=true loop=true muted=true %}
    -->
    <div class="caption">Input: multi-frame RGB sequence from the STOW tabletop domain.</div>
  </div>
  <div class="col-sm mt-3 mt-md-0">
    <!-- TODO: predicted scene graph visualization -->
    <!--
    {% include figure.html path="assets/img/sg_output.jpg" title="Predicted scene graph" class="img-fluid rounded z-depth-1" %}
    -->
    <div class="caption">Output: predicted scene graph with object nodes and labeled relationship edges.</div>
  </div>
</div>

---

## Results

<!-- TODO: results figures -->
<!--
<div class="row mt-3">
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.html path="assets/img/sg_recall.jpg" title="Recall@K" class="img-fluid rounded z-depth-1" %}
    <div class="caption">Recall@K: temporal model vs. single-frame baseline.</div>
  </div>
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.html path="assets/img/sg_f1.jpg" title="F1 by relationship type" class="img-fluid rounded z-depth-1" %}
    <div class="caption">Per-predicate F1 scores on the STOW benchmark.</div>
  </div>
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.html path="assets/img/sg_ablation.jpg" title="Ablation" class="img-fluid rounded z-depth-1" %}
    <div class="caption">Ablation: effect of sequence length and BiLSTM depth.</div>
  </div>
</div>
-->

---

## Status

Active. Training runs on the [Hyak HPC cluster](https://hyak.uw.edu/) at UW. Current work focuses on improving recall on symmetric spatial predicates and transferring to real-world scenes.

*University of Washington Sensors and Systems Lab.*
