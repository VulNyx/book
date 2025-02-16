---
title: 🟢 23 - Telnet (TCP)
date: 0023-01-01
layout: post
---

### Overview

[**Telnet**](https://en.wikipedia.org/wiki/Telnet) is the name of a network protocol that allows us to access another machine to manage it remotely as if we were sitting in front of it. It is also the name of the computer program that implements the client.

Default Port: **`23`**  
```bash
PORT   STATE SERVICE
23/tcp open  telnet
```
```bash
PORT   STATE SERVICE VERSION
23/tcp open  telnet  Netkit telnet-ssl telnetd
```

When [**Nmap**](https://nmap.org) does not get the service header, it uses the [**IANA**](https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml) db to determine the name of the service associated with that port.

---

### Enumeration

#### Active

```bash
nc -vn 192.168.1.2 23
timeout 0.1 bash -c "nc -nv 192.168.1.2 23"
nmap -p23 -sS 192.168.1.2
nmap -p23 -sVC 192.168.1.2

nmap -p23 --script="telnet-encryption" 192.168.1.2
nmap -p23 --script="telnet-ntlm-info" 192.168.1.2
```

#### Pasive

##### [**Shodan**](https://shodan.io)

```bash
port:23
port:23 telnetd
port:23 country:"US"
```

---

### Usage & Commands

#### Connect

```bash
# default port (23)
telnet 192.168.1.2

# other port
telnet 192.168.1.2 1234
```

---

### Brute Force

([**Telnet**](https://en.wikipedia.org/wiki/Telnet) is an excessively slow protocol, to avoid false negatives do **not implement threads**.)

#### Password

If you got a **`username`** and need the **`password`** this is the way.

```bash
hydra -t 1 -l user -P rockyou.txt telnet://192.168.1.2 -V -F -I
nmap -p23 --script="telnet-brute" --script-args="user=john,passdb=rockyou.txt,telnet-brute.timeout=8s" 192.168.1.2
```
```bash
msf6 > use auxiliary/scanner/telnet/telnet_login 
msf6 auxiliary(scanner/telnet/telnet_login) > set RHOSTS 192.168.1.2
msf6 auxiliary(scanner/telnet/telnet_login) > set USERNAME peter
msf6 auxiliary(scanner/telnet/telnet_login) > set PASS_FILE rockyou.txt
msf6 auxiliary(scanner/telnet/telnet_login) > set THREADS 1
msf6 auxiliary(scanner/telnet/telnet_login) > set STOP_ON_SUCCESS true
msf6 auxiliary(scanner/telnet/telnet_login) > run
```

#### Username

If you have a **`password`** and need the **`username`** this is the way.

```bash
hydra -t 1 -L users.dic -p P@ssword1 telnet://192.168.1.2 -V -F -I
```

---

### Hardening & Configuration

#### Interesting Files

- **`/etc/inetd.conf`**
- **`/etc/xinetd.d/telnet`**
- **`/etc/xinetd.d/stelnet`**

#### Install

```bash
# client
apt install -y telnet
apt install -y telnet-ssl

# server
apt install -y telnetd
apt install -y telnetd-ssl
```

#### Change Port

```bash
nano /etc/services
```

```bash
# default port
telnet    23/tcp

# other port
telnet    1234/tcp
```

---

<div style="display: flex; justify-content: center; align-items: center; width: 100%; margin-top: 20px;">
  <img src="/assets/gitbook/images/favicon.png" style="width: 30px; height: auto; margin-right: 6px;">
  <span style="color: #ffffffa4;">© VulNyx 2023-2025</span>
</div>
