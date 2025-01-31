---
title: 🟡 Host Discovery
date: 1001-01-01
layout: post
---

### Linux

##### Nmap

```ruby
# apt instal -y nmap
nmap -sn 192.168.1.0/24
```

##### Fping

```ruby
# apt install -y fping
❯ fping -aqg 192.168.1.0/24
```

##### Netdiscover

```ruby
# apt install -y netdiscover
netdiscover -i eth0 -r 192.168.1.0/24
```

### arp-scan

```ruby
# apt install -y arp-scan
arp-scan -I eth0 -l
```

### Ping

```ruby
for i in $(seq 1 254); do (ping -c 1 192.168.1.${i} | grep "bytes from" | awk '{print $4}' | tr -d ':' &); done;
```

### Bash

##### Network segments: `1`

```ruby
#!/bin/bash

echo -e "\n[!] Start Discovery:\n"

for i in $(seq 1 254); do
  timeout 1 bash -c "ping -c 1 192.168.1.$i" &> /dev/null && echo -e "  [+] 192.168.1.$i" &
done; wait
```

##### Network segments: `2`

```ruby
#!/bin/bash

hosts=("192.168.1" "10.10.10")

echo -e "\n[!] Start Discovery:\n"

for host in ${hosts[@]}; do
  echo -e "[*] Range: $host.0/24"
  for i in $(seq 1 254); do
    timeout 1 bash -c "ping -c 1 $host.$i" &>/dev/null && echo -e "  [+] $host.$i" &
  done; wait
done
```

---

### Windows

##### CMD

```ruby
for /l %i in (1,1,254) do @ping -4 -n 1 -w 100 192.168.1.%i | findstr TTL
for /L %a IN (1,1,254) DO @(ping -n 1 -w 1 192.168.1.%a | findstr "TTL=" > nul && echo 192.168.1.%a)
```

##### PowerShell

```ruby
1..254 | % {ping -4 -n 1 -w 100 X.X.X.$_} | Select-String TTL
1..254 | % {ping -4 -n 1 -w 100 X.X.X.$_} | Select-String TTL | % {$regex = [regex] '\b\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}\b'; $regex.Matches($_)} | % {$_.value}
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

