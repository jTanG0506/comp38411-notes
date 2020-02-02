---
title: Comparison of Transport Mode and Tunnel Mode
markup: "mmark"
weight: 9
---

## Comparison of Transport Mode and Tunnel Mode

|  | Transport Mode | Tunnel Mode |
| --- | -------------- | ----------- |
| AH | Authenticates IP payload and selected portions of IP header | Authenticates entire inner IP packet and selected portions of outer IP |
| ESP | Encrypts IP payload | Encrypts entire inner IP packet |
| ESP with authentication | Encrypts IP payload. Authenticates IP payload but not IP header | Encrypts and authenticates entire inner IP packet |
