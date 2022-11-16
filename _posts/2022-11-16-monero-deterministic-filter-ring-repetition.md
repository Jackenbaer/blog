---
layout: post
title:  "Monero Deterministic Filter: Ring Repetition"
date:   2022-11-16 16:16:23 +0200
categories: monero 
---

# Ring Repetition


| Case |
|:--|
| ![](/assets/images/monero_filter3_1.jpg) |
| Typical situation without any filters applied yet |

| Apply filter |
|:--|
| ![](/assets/images/monero_filter3_2.jpg) |
| The so-called ring repetition method uses multiple appearances of the same ring to identify spent outputs. This method simply identifies a collection of n separate n-rings containing the same outputs, where we can conclude that the ring is spent. This analysis was initially presented as semi-cooperative attack, where an adversary generates ring repetitions of controlled outputs to signal to other adversarial users that the ring is spent. See [this paper](https://www.getmonero.org/resources/research-lab/pubs/MRL-0007.pdf) |
