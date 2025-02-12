---
title: 🟡 Port Scanning
date: 1002-01-01
layout: post
---

### Open Ports

#### TCP

##### Nmap

```bash
nmap -n -Pn -sS -p- --min-rate="5000" 192.168.1.2                    # ipv4
nmap -n -Pn -sS -p- --min-rate="5000" -6 fe80::a00:27ff:fe7b:77f7    # ipv6
proxychains nmap -n -Pn -sT -p- --min-rate="5000" 192.168.1.2        # tunnel / proxy (proxychains)
```

##### Xargs

```bash
seq 1 65535 | xargs -P 50 -I {} bash -c 'echo "" > /dev/tcp/192.168.1.2/{} &>/dev/null && echo -e "[+] Port: {} OPEN"' 2>/dev/null
```

##### Netcat

```bash
nc -zvw 1 192.168.1.2 1-65535
```

##### PowerShell

```bash
1..65535 | % {echo ((New-Object Net.Sockets.TcpClient).Connect("192.168.1.2", $_)) "TCP port $_ is open"} 2>$null
```

##### Bash

**Host**

```bash
#!/bin/bash

echo -e "\n[!] Start Discovery:"

for port in $(seq 1 65535); do
  timeout 1 bash -c "echo '' &>/dev/null < /dev/tcp/192.168.1.2/$port" 2>/dev/null && echo -e "\t[+] $port" &
done; wait
```

**Hosts**

```bash
#!/bin/bash

hosts=("192.168.1.2" "192.168.1.3" "192.168.1.4" "192.168.1.5")

for host in ${hosts[@]}; do
  echo -e "\n[+] Host: $host"
  for port in $(seq 1 10000); do
    timeout 1 bash -c "echo '' > /dev/tcp/$host/$port" 2>/dev/null && echo "[*] Ports: $port - Active" &
  done; wait
done
```

##### Metasploit

```bash
use auxiliary/scanner/portscan/tcp
show options
set PORTS 1-65535
set RHOSTS 192.168.1.2
set THREADS 10
run
```

#### UDP

```bash
nmap -sU --top-ports="10" 192.168.1.2
nmap -sU --top-ports="20" 192.168.1.2
nmap -sU --top-ports="30" 192.168.1.2
nmap -sU --top-ports="40" 192.168.1.2
nmap -sU --top-ports="50" 192.168.1.2
nmap -sU --top-ports="100" 192.168.1.2

nmap -sUVC -p1-100 192.168.1.2
nmap -sUVC -p101-200 192.168.1.2
nmap -sUVC -p69,161 192.168.1.2
```

#### SCTP

```bash
nmap -n -Pn -sY -p- --min-rate="5000" 192.168.1.2
```
The **`SCTP`** port must be converted to **`TCP`**, this is necessary to be able to access the service normally since many services are not compatible with this protocol.

```bash
socat TCP-LISTEN:8081,fork SCTP:192.168.1.2:8080
```

---

### Services & Versions

#### TCP

```bash
nmap -n -Pn -sVC -p<PORTS> 192.168.1.2                      # ipv4
nmap -n -Pn -sVC -p<PORTS> -6 fe80::a00:27ff:fe7b:77f7      # ipv6
proxychains nmap -p<PORTS> -sTVC 127.0.0.1 2>/dev/null      # tunnel / proxy (proxychains)

nc -vn <IP> <PORT>                                          # banner grabbing
timeout 0.1 bash -c "nc -nv 192.168.1.2 <PORT>"             # banner grabbing
```

#### UDP

```bash
nmap -sU -sVC -p161,500 192.168.1.2
```

#### SCTP

```bash
nmap -p22,80,8080 -sYVC 192.168.1.2
```

---

### Output (Nmap)

The relevant output formats and parameters are:  

<div class="table-wrapper" markdown="block">

|Flag|Description|
|:-:|:-:|
|**`-oN`**|Normal|
|**`-oG`**|Grepable|
|**`-oX`**|XML|

</div>

<br>

The **`-oX (XML)`** format can be converted to **HTML** with **`xsltproc`**, we raise an **HTTP** server to display the new **HTML** file in an attractive way.

```bash
# apt install -y xsltproc
xsltproc nmap.xml >/var/www/html/index.html ; service apache2 start
```

---

### Function (copy-ports)

Function to add to **`.bashrc`** or **`.zshrc`** to **copy open ports** from an **nmap file**.

```bash
copy-ports () {
  if [ -n "$1" ]
  then
    grep -oP '\d{1,5}/tcp' $1 | cut -d '/' -f 1 | xargs | tr ' ' ',' | tr -d '\n' | xclip -sel clip
  else
    echo "[i] Usage: copy-ports <nmap file>"
  fi
}
```

---

<br><br>
<div style="display: flex; justify-content: center; align-items: center; width: 100%; margin-top: 20px;">
  <img src="/assets/gitbook/images/favicon.png" style="width: 30px; height: auto; margin-right: 6px;">
  <span>© VulNyx 2023-2025</span>
</div>
