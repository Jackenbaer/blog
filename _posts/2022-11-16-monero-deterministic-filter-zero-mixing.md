---
layout: post
title:  "Monero Deterministic Filter: Zero Mixing"
date:   2022-11-16 16:16:23 +0200
categories: monero 
---

# Zero Mixing


| Case |
|:--|
| ![](/assets/images/monero_filter0_1.jpg) |
| Typical situation without any filters applied yet |

| Step 1 |
|:--|
| ![](/assets/images/monero_filter0_2.jpg) |
| We spotted a ring signature over a single output. This has to be the real output. |


| Step 2 |
|:--|
| ![](/assets/images/monero_filter0_3.jpg) |
| Outputs can only be spend a single time. Remove the other edge. |


| Repeat |
|:--|
| ![](/assets/images/monero_filter0_4.jpg) |
| After removing the edge we created another zero mixing transaction. Repeat everything as until there are no zero mixing transactions left. |

