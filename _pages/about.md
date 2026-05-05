---
layout: about
title: about
permalink: /
subtitle: PhD Student at the University of Washington advised by Dr. Joshua Smith

profile:
  align: right
  image: prof_pic.jpg
  image_circular: false # crops the image to make it circular
  address: >
    <p>252 CSE2</p>
    <p>3800 E Stevens Way</p>
    <p>Seattle, WA 98195</p>

news: false  # includes a list of news items
latest_posts: false  # includes a list of the newest posts
selected_projects: true # includes a list of projects marked as "selected: true"
selected_papers: true # includes a list of papers marked as "selected={true}"
social: true  # includes social icons at the bottom of the page
---

I'm a first year PhD student advised by [Dr. Joshua Smith](https://www.cs.washington.edu/people/faculty/jrs) in the [Sensors and Systems Lab](https://sensor.cs.washington.edu/) at the University of Washington. My research focuses on robotic manipulation in cluttered, unstructured environments — the kind of settings you encounter in a kitchen cabinet or a storage bin, where objects collide, novel items appear, and safe control matters.

One thread of my work addresses object rearrangement in clutter: given a bin packed with objects, how does a robot plan and execute a sequence of pushes to retrieve a target? I'm building an end-to-end system that uses GPU-parallel MCTS to search over push sequences in simulation, then hands off to an MPPI controller that re-optimizes each motion online — rolling out hundreds of trajectories simultaneously in Isaac Lab and closing the loop against a real UR16e at control frequency.

The other thread focuses on scene understanding. Effective manipulation requires knowing not just what objects are present, but how they relate to each other spatially. I'm developing a neural architecture that takes a short sequence of RGB frames and produces a structured scene graph — nodes are objects, edges are labeled spatial predicates like *on_top_of* or *left_of*. Temporal context across frames lets the model resolve ambiguities no single view could answer, using a DINO Vision Transformer for feature extraction, a BiLSTM for temporal aggregation, and iterative message passing for relational reasoning.

Before starting my PhD, I worked on [vision-based assessment of cranberry crop ripening](https://arxiv.org/abs/2309.00028) using deep learning applied to aerial imagery.
