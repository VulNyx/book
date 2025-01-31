---
title: 🟡 Port Scanning
date: 1002-01-01
layout: post
---

### Open Ports

##### TCP

**Nmap**

```ruby
nmap -n -Pn -sS -p- --min-rate="5000" 192.168.1.2                    # ipv4
nmap -n -Pn -sS -p- --min-rate="5000" -6 fe80::a00:27ff:fe7b:77f7    # ipv6
proxychains nmap -n -Pn -sT -p- --min-rate="5000" 192.168.1.2        # tunnel / proxy (proxychains)
```

**Xargs**

```ruby
seq 1 65535 | xargs -P 50 -I {} bash -c 'echo "" > /dev/tcp/192.168.1.2/{} &>/dev/null && echo -e "[+] Port: {} OPEN"' 2>/dev/null
```

**Netcat**

```ruby
nc -zvw 1 192.168.1.2 1-65535
```

**PowerShell**

```ruby
1..65535 | % {echo ((New-Object Net.Sockets.TcpClient).Connect("192.168.1.2", $_)) "TCP port $_ is open"} 2>$null
```

**Bash**

Host

```ruby
#!/bin/bash

echo -e "\n[!] Start Discovery:"

for port in $(seq 1 65535); do
  timeout 1 bash -c "echo '' &>/dev/null < /dev/tcp/192.168.1.2/$port" 2>/dev/null && echo -e "\t[+] $port" &
done; wait
```

Hosts

```ruby
#!/bin/bash

hosts=("192.168.1.2" "192.168.1.3" "192.168.1.4" "192.168.1.5")

for host in ${hosts[@]}; do
  echo -e "\n[+] Host: $host"
  for port in $(seq 1 10000); do
    timeout 1 bash -c "echo '' > /dev/tcp/$host/$port" 2>/dev/null && echo "[*] Ports: $port - Active" &
  done; wait
done
```

**Metasploit**

```ruby
use auxiliary/scanner/portscan/tcp
show options
set PORTS 1-65535
set RHOSTS 192.168.1.2
set THREADS 10
run
```

##### UDP

```ruby
nmap -sUVC -p1-100 192.168.1.2
nmap -sUVC -p101-200 192.168.1.2
nmap -sUVC -p69,161 192.168.1.2
nmap -sU --top-ports="10" 192.168.1.2
nmap -sU --top-ports="20" 192.168.1.2
nmap -sU --top-ports="30" 192.168.1.2
nmap -sU --top-ports="40" 192.168.1.2
nmap -sU --top-ports="50" 192.168.1.2
nmap -sU --top-ports="100" 192.168.1.2
```

##### SCTP

```ruby
nmap -n -Pn -sY -p- --min-rate="5000" 192.168.1.2
```

It is necessary to convert the `SCTP` port to `TCP` in order to reach the service normally since many are not compatible with this protocol.

```ruby
socat TCP-LISTEN:8081,fork SCTP:192.168.1.2:8080
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

