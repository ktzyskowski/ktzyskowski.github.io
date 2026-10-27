---
layout: page
title: From Input to Identity
description: "M.S. thesis: player identification from Counter-Strike 2 gameplay telemetry"
dates: Sep 2024 – Apr 2025
importance: 2
category: research
---

**From Input to Identity: An AI-Driven Approach to Player Identification in Counter-Strike 2**<br>
M.S. thesis, Northeastern University, Sep 2024 – Apr 2025. Advised by [Prof. Rajagopal Venkat](https://rajagopalvenkat.com/).

## Question

Can a player be recognized from _how_ they play, from their mouse movement and keystrokes alone, without knowing which account they're on?

## Data

- Built a scraping and parsing pipeline (**Playwright + Polars**) that collected **1,000+ professional _Counter-Strike 2_ match replays** from HLTV and turned them into mouse and keystroke telemetry.
- This produced **103K recoil-spray samples from 1,411 players**: short bursts of sustained fire, where each player's aim control is easy to compare.
- Separately engineered **108 behavioral features** per 5-second window, covering keystroke timing, key-press entropy, and mouse speed and acceleration, over roughly **520 hours of gameplay from 154 players**.

## Model

A **Siamese LSTM** in PyTorch, trained with **online triplet mining** over packed variable-length sequences. It learns an embedding in which windows from the same player sit close together.

## Results

- On **31 held-out players**, nearest-neighbor identification (5-NN) on the learned embeddings reached **22% accuracy against 3% chance**, about 7× chance.
- Pairwise _verification_ (deciding whether two samples come from the same player) stayed near chance with random-forest and MLP verifiers. I traced this to missing **game context**: the same hand moves differently depending on what's happening in the game. As a next step, I proposed a cross-attention Transformer that fuses input dynamics with game state.
- I also evaluated the embeddings with silhouette scores and t-SNE.

## Deployment risk

Any system like this can wrongly ban someone, and it may be biased against players with motor impairments. I recommended setting its decision threshold to favor missed detections over false accusations, with human review before any action.

## What came next

[PlayID](/projects/1_playid/) returns to this question with roughly 300× more accounts, a different game, and a stricter open-set evaluation.
