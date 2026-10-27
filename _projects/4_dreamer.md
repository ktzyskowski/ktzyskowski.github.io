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

I reimplemented **DreamerV3** ([Hafner et al., 2023](https://arxiv.org/abs/2301.04104)) from the paper in PyTorch. DreamerV3 is an agent that learns a model of its environment, then trains its policy on rollouts _imagined_ inside that model rather than only on real experience.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/cartpole_comparison.gif" title="CartPole rollout" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

## Result

Solved **CartPole-v1**, with a mean return of **491** over 100 greedy evaluation episodes, above the standard solve threshold of 475.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/cartpole-eval.png" title="CartPole evaluation return" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

## What I built

- The **RSSM world model** with categorical latents.
- An **actor-critic trained in imagination**, with twohot reward and value heads and an EMA target critic.
- A **custom environment** to test whether the world model can remember information over time.

## Debugging silent failures

Both of the hardest bugs let training run without errors while quietly producing the wrong result:

- Using `sample()` instead of `rsample()` cut the gradients through the stochastic latents, so part of the model stopped learning.
- Stochastic start states added noise to the KL term. I fixed it by masking the KL on the initial recurrent states.
