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

This project transcribes bass lines from full songs. I separate the bass from the mix with Demucs, and a model transcribes the separated audio into notes. Since the model works on Demucs output instead of clean bass recordings, it has to deal with separation artifacts, like bleed from other instruments during rests.

## Model

The model is a temporal CNN over harmonic-CQT features, with three output heads for onset, voicing, and pitch. I decode its outputs into notes and score them with `mir_eval`.

## Results

I changed one component at a time and measured each change, and improved note F1 on held-out Demucs-separated audio (Slakh2100) from 0.74 to 0.88, with the same model size and training data.

Breaking errors down into over-segmentation, voicing, and pitch errors showed that the pYIN pitch tracker was limiting recall. Replacing it with a learned pitch head added 5 F1 points on test.

I also found a padding bug: padded frames were filled with 0 dB, the loudest value, instead of −80 dB silence. After the fix, stem dropout reduced the F1 drop from a missing mixture input from 24 points to 3, at a cost of 0.5 points.

I implemented a differentiable semi-Markov CRF decoder, and debugged a NaN gradient that appeared even though the forward pass was finite. It still scored 4.1 F1 below greedy decoding on test, so I didn't use it.

## Real recordings

On real jazz recordings from FiloBass, the model reached 0.66 F1. About 40% of its false positives had the correct pitch but fell just outside the 50 ms onset tolerance, so much of the gap is in timing rather than pitch.
