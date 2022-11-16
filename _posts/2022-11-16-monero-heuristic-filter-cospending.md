---
layout: post
title:  "Monero Heuristic Filter: Output Cospending"
date:   2022-11-16 16:16:23 +0200
categories: monero 
---

# Output Cospending

| Case |
|:--|
| ![](/assets/images/monero_filter1_1.jpg) |
| Typical situation without any filters applied yet |

| Identify cospending and remove edges |
|:--|
| ![](/assets/images/monero_filter1_2.jpg) |
| We spotted that both outputs originate from the same transaction. Since it is unlikely that both outputs are picket for a ring signature set, both outputs likely belong to the transaction and are cospended |

