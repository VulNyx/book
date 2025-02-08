---
title: 🐧 Linux
date: 0002-01-01
layout: post
---

### Enumeration

```ruby
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

---

### Network

```ruby
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

---

### Interesting Files

```ruby
/etc/passwd
/etc/shadow
/etc/hosts
/etc/hotname
/etc/issue
/etc/motd
/etc/sudoers
/etc/crontab

/home/user/.ssh/id_rsa
/root/.ssh/id_rsa

/etc/ssh/sshd_config
/var/log/auth.log

/var/log/apache2/access.log
/var/log/apache2/error.log
/etc/apache2/sites-available/000-default.conf
/etc/apache2/apache2.conf
```

---

### Restricted Bash (rbash) [Escape]

```ruby
ssh low@192.168.1.2 -t 'bash --noprofile'
ssh low@192.168.1.2 bash
ssh -i id_rsa low@192.168.1.2 -t 'bash --noprofile'
```

---

### Compilate Binary

**Windows**

```ruby
# apt-get install mingw-w64
i686-w64-mingw32-gcc main.c -o binary.exe        # x86 - 32 bits
x86_64-w64-mingw32-gcc main.c -o binary.exe      # x64 - 64 bits
```

**Linux**

```ruby
gcc -m32 main.c -o binary      # x86 - 32 bits
gcc main.c -o binary           # x64 - 64 bits

gcc main.c -o binary -static   # fix errors
```

**Go**

```ruby
go build .                     # default (no compress)

go build  -ldflags '-s -w' .   # compress
upx binary
```

---
