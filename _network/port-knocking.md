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
    
    Note right of www-data: Buscando archivos sensibles...
    
    www-data-->>root: Escalada exitosa
```
