---
title: Anti-Replay
markup: "mmark"
weight: 6
---

## Anti-Replay
Recall that a replay attack is one in which an attacker obtains a copy of an authenticated packet and later transmits it to the intended destination. The Sequence Number field to designed to thwart such attacks.

-----

### Outbound Processing
Upon establishing an SA, the sender initialises a sequence number counter to 0. Each time a packet is sent on this SA, the sender increments the counter and populates the Sequence Number field with this value; thus, the first value to be used is 1. If the sequence number reaches the limit of $$2^{32} - 1$$, the sender should terminate this SA and negotiate a new SA with a new key.

-----

### Inbound Processing
As IP is a connectionless, unreliable service, we have no guarantee that packets will be delivered in the correct order, or at all. Therefore, the IPSec authentication document states that the receiver should implement a window of size $$W$$, which defaults to $$W = 64$$. 

Inbound processing proceeds as follows when a packet is received:
1. If the received packet falls within the window and is unseen, the MAC is checked and marked if the packet is authenticated.
1. If the received packet is to the right of the window and is unseen, the MAC is checked. If the packet is authenticated, the window is advanced to the right so that the sequence number of this packet is the right edge of the window, and the packet is marked.
1. If the received packet is to the left of the window or if authentication fails, the packet is discarded.

![Anti-Replay Mechanism](/docs/figures/anti-replay-mechanism.png)
