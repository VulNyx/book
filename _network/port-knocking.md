---
title: 🟡 Port Knocking
date: 1004-01-01
layout: post
mermaid: true
---

Port Knocking

```mermaid
graph LR;
    hacker-- Command Injection ---www-data;
    www-data-- Find (Sudo) ---root;
```



```mermaid
graph LR;
    attacker-- Protocol ---ipv4;
    ipv4-- SYN ---openports;
```
