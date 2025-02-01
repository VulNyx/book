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
    Attacker--> Protocol ---IPv4;
    IPv4-- SYN ---Open_Ports;
    Open_Ports-- Banner_Grabbing ---Victim;
```
