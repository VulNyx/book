---
title: 🟢 23 Telnet (TCP)
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
nmap -p22 -sS 192.168.1.2
nmap -p22 -sVC 192.168.1.2


nmap -p21 --script="ftp-anon" 192.168.1.2
nmap -p21 --script="ftp-*" 192.168.1.2
```

#### Pasive

##### [**Shodan**](https://shodan.io)

```bash
port:21
port:21 vsftpd
port:21 proftpd 1.3.5
port:21 os:"Linux"
port:21 os:"Windows"
port:21 country:"US"
port:21 country:"US" os:"Linux"
port:21 hostname:"ftp.domain.tld"
```

---

### Usage & Commands

```bash
# upload file
put cmd.php
# upload all files
mput *
# download
get config.php
# download all files
prompt off
mget *
# binary mode enable
binary
# exit
bye
# rename file
ftp> rename cmd.php
(to-name) cmd.php5
# read files from ftp
ftp> less .htaccess
ftp> more .htaccess
```

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
hydra -t 1 -l user -P /opt/techyou.txt telnet://192.168.1.2 -V -F -I
```

#### Username

If you have a **`password`** and need the **`username`** this is the way.

```bash
hydra -t 1 -l users.dic -p P@ssword1 telnet://192.168.1.2 -V -F -I
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
