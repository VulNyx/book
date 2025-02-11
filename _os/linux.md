---
title: 🐧 Linux
date: 0002-01-01
layout: post
---

### Enumeration

```bash
# username
whoami
# user groups
id
# connected users
w
# machine name
hostname
# os & kernel
uname -a
hostnamectl
lsb_release -a
# list files and folders
ls
ls -l
ls -la
ls -laR
tree
tree -fas
# file size
du -hc filename
du -hc *

env
set
history
alias

ps aux
ps -faux
ps -eo command
ps -eo command "root"
```

### Network

```bash
ip a
ip -4 a
ip -6 a
ip a show eth0

ifconfig

cat /proc/net/arp

route
route -n
routel
ip route show

hostname -I

iptables -L

ss -ltun
ss -ltun | grep "127"
ss -nltp
pwdx 8505
netstat -nat
netstat -nat | grep "127"

lsof -i :22
lsof | grep "1234"
fuser 65000/tcp
fuser -k 3306/tcp
```

### Interesting Files

```bash
/etc/passwd
/etc/shadow
/etc/hosts
/etc/hostname
/etc/issue
/etc/motd
/etc/sudoers
/etc/crontab
/etc/group

/home/user/.ssh/id_rsa
/root/.ssh/id_rsa

/etc/ssh/sshd_config
/var/log/auth.log

/var/log/apache2/access.log
/var/log/apache2/error.log
/etc/apache2/sites-available/000-default.conf
/etc/apache2/apache2.conf
```

### Restricted Bash (rbash) [Escape]

```bash
ssh low@192.168.1.2 -t 'bash --noprofile'
ssh low@192.168.1.2 bash
ssh -i id_rsa low@192.168.1.2 -t 'bash --noprofile'
```

### Compilate Binary

**Windows**

```bash
# apt-get install mingw-w64
i686-w64-mingw32-gcc main.c -o binary.exe        # x86 - 32 bits
x86_64-w64-mingw32-gcc main.c -o binary.exe      # x64 - 64 bits
```

**Linux**

```bash
gcc -m32 main.c -o binary      # x86 - 32 bits
gcc main.c -o binary           # x64 - 64 bits

gcc main.c -o binary -static   # fix errors
```

**Go**

```bash
go build .                     # default (no compress)

go build  -ldflags '-s -w' .   # compress
upx binary
```

<div style="display: flex; justify-content: center; align-items: center; width: 100%; margin-top: 20px;">
  <img src="/assets/gitbook/images/favicon.png" style="width: 30px; height: auto; margin-right: 6px;">
  <span style="color: #ffffffa4;">© VulNyx 2023-2025</span>
</div>
