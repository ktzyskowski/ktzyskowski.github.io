---
layout: page
title: DreamerV3 from Scratch
description: A from-scratch PyTorch reimplementation of model-based RL
img: assets/img/projects/cartpole-eval.png
dates: Mar – Apr 2026
importance: 2
category: independent
github: https://github.com/ktzyskowski/dreamer
---

Independent project, Mar – Apr 2026. [Code on GitHub](https://github.com/ktzyskowski/dreamer).

I find that the best way to learn how something works is to build it myself, so I reimplemented **DreamerV3** ([Hafner et al., 2023](https://arxiv.org/abs/2301.04104)) from the paper in PyTorch. DreamerV3 is a model-based reinforcement learning agent: it learns a model of its environment, then trains its policy on rollouts imagined inside that model, rather than only on real experience.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/cartpole_comparison.gif" title="CartPole rollout" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

I kept the two halves separate, the same way the paper splits them. The world model (an RSSM with categorical latents) represents the environment, and the agent plans in it, with an actor-critic trained on imagined rollouts.

## Result

The agent solves CartPole-v1, averaging a return of 491 over 100 evaluation episodes, above the standard solve threshold of 475.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/cartpole-eval.png" title="CartPole evaluation return" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

MiniGrid Memory, where the agent has to remember a cue over many steps, is still in progress. To test the memory part on its own, I built a small environment: the agent sees a cue, its observations go blank for a while, and then it has to act on the cue it saw.

## Two silent bugs

The two hardest bugs I hit didn't throw any errors. Training ran, the losses went down, and the agent still didn't learn.

The first was calling `sample()` instead of `rsample()` when drawing the stochastic latents. `sample()` doesn't pass gradients through, so part of the world model quietly stopped learning. This one took me a while to track down.

The second showed up in environments with random start states. At the first step, the prior only sees an empty recurrent state, so it has no way to predict which start it's in, but the KL term still pulled the posterior toward it. Masking the KL on those initial states fixed it. I haven't seen this handled explicitly in other public implementations.
