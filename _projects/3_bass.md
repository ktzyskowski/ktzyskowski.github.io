---
layout: page
title: Bass Transcription
description: Transcribing bass lines from source-separated audio
dates: Jul – Aug 2026
importance: 1
category: independent
github: https://github.com/ktzyskowski/bass-transcription
---

Independent project, Jul – Aug 2026. Built with PyTorch, Demucs, mir_eval, and MLflow. [Code on GitHub](https://github.com/ktzyskowski/bass-transcription).

## Goal

Turn a song's bass line into notes. The input is bass audio separated from a full mix with **Demucs**, so the model has to cope with whatever artifacts the separator leaves behind.

## Headline result

Raised held-out note F1 on Demucs-separated audio from **0.74 to 0.88** (synthetic Slakh2100), entirely through **single-variable ablations**: no larger model and no extra data.

## Model

A multi-head temporal CNN in PyTorch with separate heads for **onset, voicing, and pitch**, operating on harmonic-CQT features. Its outputs are decoded into note events and scored with `mir_eval`.

## How the gains came

- **Error breakdown.** I split errors into over-segmentation, voicing, and pitch to find which one was the bottleneck at each stage. That showed the pYIN pitch tracker was capping recall. Replacing it with a learned pitch head added **+5.0 F1** on test.
- **A padding bug.** Padding was being filled with 0 dB, the _loudest_ value, instead of −80 dB silence. After fixing it, stem dropout cut the penalty for a missing mixture from **24 F1 points to 3**, at a cost of 0.5 F1.
- **A negative result.** I built a differentiable semi-Markov CRF decoder trained with truncated backpropagation through time, and traced a NaN gradient that appeared even though the forward pass was finite. The decoder still trailed simple greedy decoding by **4.1 F1** on held-out test, so I retired it.

## Out-of-domain check

On real jazz recordings (FiloBass) the model reached **0.66 F1**. When I re-sorted its false positives, about **40%** turned out to be correctly pitched notes placed just outside the 50 ms onset tolerance. That means much of the drop is timing, not wrong notes.
