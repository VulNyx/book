---
title: 🌐 Subdomains Enumeration
date: 0001-01-01
layout: post
---

### Fuzzing

#### Gobuster

```ruby
gobuster vhost -w vhost.dic -u 'http://domain.tld' --append-domain
```

#### Wfuzz

```ruby
wfuzz -c -w vhost.dic -H 'Host: FUZZ.domain.tld' -u 'http://domain.tld/' --hh=186
```

#### Ffuf

```ruby
ffuf -c -w vhost.dic -H 'Host: FUZZ.domain.tld' -u 'http://domain.tld/' -fs 186
```

---

### Wordlist

- [**subdomains-top1million-5000.txt**](https://raw.githubusercontent.com/danielmiessler/SecLists/refs/heads/master/Discovery/DNS/subdomains-top1million-5000.txt)
- [**subdomains-top1million-20000.txt**](https://raw.githubusercontent.com/danielmiessler/SecLists/refs/heads/master/Discovery/DNS/subdomains-top1million-20000.txt)
- [**subdomains-top1million-110000.txt**](https://raw.githubusercontent.com/danielmiessler/SecLists/refs/heads/master/Discovery/DNS/subdomains-top1million-110000.txt)

---

<div style="display: flex; justify-content: center; align-items: center; width: 100%; margin-top: 20px;">
  <img src="/assets/gitbook/images/favicon.png" style="width: 30px; height: auto; margin-right: 6px;">
  <span style="color: #ffffffa4;">© VulNyx 2023-2025</span>
</div>
