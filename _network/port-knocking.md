---
title: 🟡 Port Knocking
date: 1004-01-01
layout: post
mermaid: true
---

Port Knocking

```mermaid
sequenceDiagram
    participant kali
    participant www-data
    participant root
    
    kali->>www-data: LFI (Local File Inclusion)
    www-data->>root: Find (Sudo)
```
