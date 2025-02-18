---
title: 🌐 Subdomains Enumeration
date: 0001-01-01
layout: post
---

### Active

#### Fuzzing (Brute Force)

##### Gobuster

```ruby
gobuster vhost -w vhost.dic -u 'http://domain.tld' --append-domain
```

##### Wfuzz

```ruby
wfuzz -c -w vhost.dic -H 'Host: FUZZ.domain.tld' -u 'http://domain.tld/' --hh=186
```

##### Ffuf

```ruby
ffuf -c -w vhost.dic -H 'Host: FUZZ.domain.tld' -u 'http://domain.tld/' -fs 186
```

#### Wordlist

##### [**SecLists**](https://github.com/danielmiessler/SecLists/tree/master/Discovery/DNS)

---

### Pasive (OSINT)

#### Search Engine Hacking

##### Google Dorks

```ruby
site:domain.tld
site:*.domain.tld
```

##### [**VirusTotal**](https://www.virustotal.com)

#### Certificate of Transparency

##### [**crt.sh**](https://crt.sh/)

---

<div style="display: flex; justify-content: center; align-items: center; width: 100%; margin-top: 20px;">
  <img src="/assets/gitbook/images/favicon.png" style="width: 30px; height: auto; margin-right: 6px;">
  <span style="color: #ffffffa4;">© VulNyx 2023-2025</span>
</div>
