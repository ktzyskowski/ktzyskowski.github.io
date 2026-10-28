---
layout: page
title: From Input to Identity
description: "M.S. thesis: player identification from Counter-Strike 2 gameplay telemetry"
dates: Sep 2024 – Apr 2025
importance: 2
category: research
---

**From Input to Identity: An AI-Driven Approach to Player Identification in Counter-Strike 2**<br>
M.S. thesis, Northeastern University, April 2025. Advised by [Prof. Rajagopal Venkat](https://rajagopalvenkat.com/).

I've played Counter-Strike since a cousin visiting from Poland let me try his copy of Counter-Strike: Source. When someone is caught cheating, Valve bans their account, but nothing stops them from making a new one, and IP bans and hardware fingerprints can be dodged with a proxy or a virtual machine. Professional players who cheat are banned as people, not as accounts, because a referee can check who is sitting at the computer. My thesis asked whether a model could do something similar online: recognize the person behind the screen from how they move the mouse and press keys.

## Two ways to look at a player

I scraped replays of professional matches from HLTV and parsed the mouse and keyboard input out of them. I tried two ways of representing a player.

The first was recoil control. In Counter-Strike, an automatic rifle's recoil follows a fixed pattern, and players spend hours learning to pull against it. Every player makes this same motion many times a game, so I hoped the small differences in how they made it would work like a signature. I extracted about 103,000 AK-47 sprays from 1,411 players across 1,003 matches.

Sprays only capture the mouse, so the second representation added the keyboard. I cut each round into 5-second segments and summarized each one with 108 features: how long keys were held, the time between presses, the entropy of key sequences, and the speed, acceleration, and path of the mouse. This dataset covered about 520 hours of play from 154 players, all on one map (Dust II) to keep the noise down.

## The model

Sequences of segments went through a Siamese LSTM trained with triplet loss, which learns an embedding where samples from the same player land close together. A classifier on top of the embeddings was meant to answer the real question: were these two samples played by the same person?

## What happened

The results fell short of what I was hoping for. The training loss went down, but the embeddings didn't separate cleanly by player. There was some signal: among 31 held-out players, a nearest-neighbor classifier picked the right player 22% of the time, where random guessing gets about 3%. But the classifiers that decided whether two samples came from the same person did no better than a coin flip on players they hadn't seen.

Looking back, I think the biggest thing holding it back was relying on hand-crafted features. I boiled every 5 seconds of play down to 108 statistics that I picked, which threw away most of the detail in the raw input and limited the model to whatever I thought mattered. A learned feature extractor, like a convolutional network over the raw tick-by-tick input, could pick up patterns I wouldn't have thought to measure. I listed this as future work in the thesis, and it's something I fixed in [PlayID](/projects/1_playid/).
I also wrote about the risks of deploying something like this. A false match can get an innocent player banned, and a model that has seen few players with motor impairments could link them to each other. I recommended tuning the system to miss some evaders rather than accuse innocent players, and having moderators review its flags instead of banning automatically.

## What came next

[PlayID](/projects/1_playid/) is my second attempt at the question, with roughly 300× more accounts, a different game, and a stricter open-set evaluation.
