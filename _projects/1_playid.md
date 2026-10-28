---
layout: page
title: PlayID
description: Open-set watchlist identification from player input in Deadlock
dates: Oct 2026 – present
importance: 1
category: research
---

**PlayID: Open-Set Watchlist Identification from Player Input in Deadlock**<br>
Independent research, Oct 2026 – present. Paper in preparation.

Players banned from an online game often come back on a new account, because games ban accounts, not people. The usual ways of linking accounts, like IP addresses, hardware IDs, and phone numbers, identify a connection or a device rather than a person, and they can all be swapped or spoofed.

PlayID asks whether a signal that comes from the player instead, the way they aim and press keys, is distinctive enough to tell players apart and stable enough to recognize them from one match to the next. I study this in Valve's _Deadlock_, whose public match replays record every player's camera and held keys throughout the match.

- **Framing.** I treat it as open-set watchlist identification: given an account's matches, either find it on a list of known accounts, or conclude that it belongs to none of them.
- **Model.** An ECAPA-TDNN encoder trained with an ArcFace loss turns a player's input into an embedding, so the same player's matches land close together.
- **Results.** So far on a validation watchlist of about 2,000 accounts, with false alarms held to 1% of searches by players not on the list, PlayID misses 2.5% of searches by players on it, against 61.9% for a model trained the same way on hand-crafted features. Test results are pending.

More details once the paper is out.

PlayID builds on my [M.S. thesis](/projects/2_thesis/), which studied the same question in _Counter-Strike 2_ at a much smaller scale.
