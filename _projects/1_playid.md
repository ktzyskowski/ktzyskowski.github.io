---
layout: page
title: PlayID
description: Open-set watchlist identification from player input in Deadlock
img: assets/img/publication_preview/playid.png
dates: Oct 2026 – present
importance: 1
category: research
---

**PlayID: Open-Set Watchlist Identification from Player Input in Deadlock**<br>
Independent research, Oct 2026 – present. Paper in preparation.

<div class="row justify-content-sm-center">
    <div class="col-sm-5 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/publication_preview/playid.png" alt="An unknown player's input is matched within a search radius to enrolled player B" class="img-fluid" %}
    </div>
</div>

## The problem

Players banned from an online game often come back on a new account, because games ban accounts rather than people. A signal that belongs to the _player_, such as the way they aim and press keys, could link those accounts. That only works if the signal is distinctive enough to tell players apart, and stable enough to recognize the same player from one match to the next.

## Framing

I treat this as **open-set watchlist identification**, following the biometrics evaluation standard ISO/IEC 19795. Given an account's matches, the system must either find that account on a list of known accounts, or conclude that the matches belong to none of them. The evaluation is built to match how the system would be used:

- **Account-disjoint splits**, so the model is never tested on accounts it trained on.
- **Time-ordered enrollment**, so accounts are enrolled from earlier matches and searched with later ones.
- **Held-out accounts as unknown probes**, so the system is measured on how often it raises a false alarm for players who aren't on the list.

## Data and model

- Decoded **82K public match replays** from Valve's _Deadlock_, covering **412K accounts**, into each player's camera angles and held keys at every tick.
- Trained an **ECAPA-TDNN** encoder with an **ArcFace** loss and modality dropout. It maps a player's input to an embedding, so the same player's matches land close together.
- Search uses **cohort-normalized** scores. Each probe's threshold adapts to how crowded its neighborhood of enrolled accounts is, which cuts false alarms where many accounts look alike.

## Results

On validation, with a watchlist of about 2K accounts and the alarm threshold set so that 1% of searches by players _not_ on the list raise a false alarm:

| Model                                                  | Miss rate (FNIR) at 1% FPIR | Rank-1 |
| ------------------------------------------------------ | --------------------------- | ------ |
| PlayID                                                 | **2.5%**                    | 98.9%  |
| Same training, hand-crafted features of the same input | 61.9%                       |        |

The paper also checks that this accuracy doesn't just come from skill level or game settings. Players searched only among others of the same skill tier are missed about as often as in random groups of the same size, and the traces that mouse settings and key bindings leave in the input identify few players.

## Limits

These results show that a player's input works as an identity _across their own matches_. Whether it carries over to a brand-new account is still untested. If it does, it could help detect ban evasion. It could also link the accounts of players who keep them separate for legitimate reasons, which any deployment would have to weigh.

PlayID builds on my [M.S. thesis](/projects/2_thesis/), which studied the same question in _Counter-Strike 2_ at a much smaller scale.
