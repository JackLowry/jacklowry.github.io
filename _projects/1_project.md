---
layout: page
title: "Planning and Control for Object Rearrangement in Clutter"
description: A two-stage system that uses GPU-parallel sampling-based planning to solve bin-clearing tasks, then executes the resulting plan with an MPPI trajectory optimizer on a real robot.
img: # assets/img/puzzles_teaser.jpg
importance: 1
category: research
related_publications: # comma-separated bib keys once published
selected: true
---

<!-- TODO: teaser — uncomment one of the options below and fill in the path -->
<!--
<div class="row mt-3">
  <div class="col-12">
    {% include video.html path="assets/video/teaser.mp4" class="img-fluid rounded z-depth-1" autoplay=true loop=true muted=true %}
  </div>
</div>
<div class="caption">
  System overview: the planner finds a push sequence in simulation (left), which the MPPI controller executes on the real robot (right).
</div>
-->

## Overview

Rearranging a target object out of a cluttered bin requires reasoning about cascading contacts across a long horizon — every push reshapes the environment, and mistakes are hard to undo. This project builds an end-to-end system that addresses both halves of the problem: **planning** a sequence of pushes that solves the rearrangement task, and **executing** each push on a real robot in a way that is robust to contact uncertainty.

The planner uses Monte-Carlo Tree Search (MCTS) or RRT with a GPU-accelerated physics simulator as a black-box forward model, evaluating thousands of candidate action sequences in parallel to find a collision-aware push plan. That plan is then handed off to an MPPI (Model Predictive Path Integral) controller, which re-optimizes each step online by rolling out 750–1000 trajectory samples simultaneously in Isaac Lab, closing the loop against the real robot at control frequency.

<!-- TODO: system diagram — replace path with your pipeline figure -->
<!--
<div class="row mt-3">
  <div class="col-12">
    {% include figure.html path="assets/img/system_diagram.jpg" title="System pipeline" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
  The two-stage pipeline: a sampling-based planner produces a waypoint sequence offline; the MPPI controller tracks it online against the real robot.
</div>
-->

---

## Planning

A simulator-agnostic abstraction layer (supporting Genesis and NVIDIA IsaacLab as backends) exposes a common `batch_evaluate()` interface, so planners can run against any physics backend without modification. MCTS uses UCB1 selection with virtual visit counts for safe parallel expansion; RRT weights tree growth by rollout reward to concentrate exploration on promising regions. Both operate over a discrete push action space (cardinal and diagonal directions, multiple heights) and run a multi-rollout verification pass to filter brittle plans.

<div class="row mt-3">
  <div class="col-sm mt-3 mt-md-0">
    <!-- TODO: planning sim video — replace path below -->
    <!--
    {% include video.html path="assets/video/mcts_planning.mp4" class="img-fluid rounded z-depth-1" autoplay=true loop=true muted=true %}
    -->
    <div class="caption">MCTS solving a bin-clearing task in simulation.</div>
  </div>
  <div class="col-sm mt-3 mt-md-0">
    <!-- TODO: planning result image/gif — replace path below -->
    <!--
    {% include figure.html path="assets/img/rrt_solution.jpg" title="RRT solution path" class="img-fluid rounded z-depth-1" %}
    -->
    <div class="caption">RRT solution path overlaid on the bin scene.</div>
  </div>
</div>

---

## Control

The MPPI controller treats each of N parallel Isaac Lab environments as one candidate trajectory. At every control step, joint velocity targets are broadcast, physics is stepped, per-environment costs are computed, and the MPPI update produces an optimally-weighted action. Cost functions are modular — the planner's waypoints seed a sequential task objective that the controller tracks while respecting joint limits and avoiding unexpected contacts.

<div class="row mt-3">
  <div class="col-sm mt-3 mt-md-0">
    <!-- TODO: MPPI rollouts visualization video -->
    <!--
    {% include video.html path="assets/video/mppi_rollouts.mp4" class="img-fluid rounded z-depth-1" autoplay=true loop=true muted=true %}
    -->
    <div class="caption">750 parallel MPPI trajectory samples evaluated in Isaac Lab.</div>
  </div>
  <div class="col-sm mt-3 mt-md-0">
    <!-- TODO: real robot execution video -->
    <!--
    {% include video.html path="assets/video/real_robot.mp4" class="img-fluid rounded z-depth-1" autoplay=true loop=true muted=true %}
    -->
    <div class="caption">UR16e executing a planned push sequence on the real robot.</div>
  </div>
</div>

---

## Results

<!-- TODO: results figures — replace paths below; add or remove columns as needed -->
<!--
<div class="row mt-3">
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.html path="assets/img/success_rate.jpg" title="Success rate vs. object count" class="img-fluid rounded z-depth-1" %}
    <div class="caption">Success rate vs. number of obstacles.</div>
  </div>
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.html path="assets/img/planning_time.jpg" title="Planning time comparison" class="img-fluid rounded z-depth-1" %}
    <div class="caption">Wall-clock planning time: MCTS vs. RRT across backends.</div>
  </div>
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.html path="assets/img/bin_example.jpg" title="Example bin scene" class="img-fluid rounded z-depth-1" %}
    <div class="caption">Example 20-object bin-clearing task.</div>
  </div>
</div>
-->

---

## Status

Active. Current work focuses on closing the sim-to-real gap and scaling to higher object counts.

*With [Paolo Torrado](mailto:paolo.a.torrado@gmail.com). University of Washington Sensors and Systems Lab.*
