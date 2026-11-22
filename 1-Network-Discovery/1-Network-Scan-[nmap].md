# Network Scan using [nmap]
<img src="https://www.kali.org/tools/nmap/images/nmap-logo.svg" style="width:80px;"/>

Nmap is a network scanner created by Gordon Lyon. and used to discover hosts and services on a computer network by sending packets and analyzing the responses.

---

### 1- Discover Current network pereference
`ifconfig`
```
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.70.17  netmask 255.255.255.0  broadcast 192.168.70.255
        inet6 fe80::4854:f1ff:fe86:7716  prefixlen 64  scopeid 0x20<link>
        ether 4a:54:f1:86:77:16  txqueuelen 1000  (Ethernet)
        RX packets 126  bytes 17578 (17.1 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 98  bytes 16942 (16.5 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```
As we can see the ip is `192.168.70.17` and the subnet mask is `255.255.255.0` thats mean the nodes in the network are between `192.168.70.1` and `192.168.70.254`

---
### 2- scan the network to discover avaiable hosts
`nmap -sn 192.168.70.*` or `nmap -sn 192.168.70.1/24`
- `-sn` Ping Scan - disable port scan

We use the -sn option to rapidly scan the network
```
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-11-16 12:29 EST
Nmap scan report for 192.168.70.1
Host is up (0.033s latency).
MAC Address: 20:3D:79:B9:E5:3F (zte)
Nmap scan report for Mac (192.168.70.16)
Host is up (0.00055s latency).
MAC Address: 56:BD:25:FC:CF:B9 (Unknown)
Nmap scan report for Windows (192.168.70.21)
Host is up (0.12s latency).
MAC Address: 1E:10:17:DF:BA:9F (Intel Corporate)
Nmap scan report for kali (192.168.70.17)
Host is up.
Nmap done: 256 IP addresses (4 hosts up) scanned in 4.32 seconds
```
as we can see, we discovered 4 hosts on the network

---
### 3- Scan a targeted host
`nmap 192.168.70.21`

```
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-11-16 12:39 EST
Nmap scan report for Mac (192.168.70.21)
Host is up (0.00076s latency).
Not shown: 994 closed tcp ports (reset)
PORT     STATE SERVICE
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
5040/tcp  open  unknown
7680/tcp  open  pando-pub
MAC Address: 1E:10:17:DF:BA:9F (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 21.39 seconds
```

---
### 4- Discover Operating Systems
`nmap -O 192.168.70.21`
- `-O` Enable OS detection

Operating systems can be used to identify possible vulnerabilities.
```
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-11-16 12:44 EST
Nmap scan report for Windows7 (192.168.70.21)
Host is up (0.0051s latency).
Not shown: 994 closed tcp ports (reset)
PORT     STATE SERVICE
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
5040/tcp  open  unknown
7680/tcp  open  pando-pub
MAC Address: 1E:10:17:DF:BA:9F (Unknown)
Too many fingerprints match this host to give specific OS details
Network Distance: 1 hop

OS detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 24.31 seconds
```

---
### 5- Identify Services Versions
`nmap -sV 192.168.70.22`
- `-sV` identify services versions

Operating systems can be used to identify possible vulnerabilities.
```
cai@kali:~$ sudo nmap -sV 192.168.70.22
Starting Nmap 7.95 ( https://nmap.org ) at 2026-10-05 04:35 EDT
Nmap scan report for 192.168.70.22
Host is up (0.00035s latency).
Not shown: 977 closed tcp ports (reset)
PORT    STATE  SERVICE    VERSION
21/tcp  open  ftp        vsftpd 2.3.4
22/tcp  open  ssh        OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
23/tcp  open  telnet     Linux telnetd
25/tcp  open  smtp       Postfix smtpd
53/tcp  open  domain     ISC BIND 9.4.2
80/tcp  open  http       Apache httpd 2.2.8 ((Ubuntu) DAV/2)
111/tcp open  rpcbind    2 (RPC #100000)
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
512/tcp open  exec       netkit-rsh rexecd
513/tcp open  login?     
514/tcp open  shell      Netkit rshd
1099/tcp open  java-rmi   GNU Classpath grmiregistry
1524/tcp open  bindshell  Metasploitable root shell
2049/tcp open  nfs        2-4 (RPC #100003)
2121/tcp open  ftp        ProFTPD 1.3.1
3306/tcp open  mysql      MySQL 5.0.51a-3ubuntu5
5432/tcp open  postgresql PostgreSQL DB 8.3.0 - 8.3.7
5900/tcp open  vnc        VNC (protocol 3.3)
6000/tcp open  X11        (access denied)
6667/tcp open  irc        UnrealIRCD
8009/tcp open  ajp13      Apache Jserv (Protocol v1.3)
8180/tcp open  http       Apache Tomcat/Coyote JSP engine 1.1
MAC Address: 08:00:27:49:AE:4D (PCS Systemtechnik/Oracle VirtualBox virtual NIC)
Service Info: Hosts: metasploitable.localdomain, irc.Metasploitable.LAN; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 65.48 seconds
```
---
### 6- Scan a Host Blocking Ping (Bypassing Windows Firewall)
`nmap -Pn 192.168.70.21`
- `-Pn` Treat all hosts as online -- skip host discovery.

When a Windows machine (like Windows 7) is set to a "Public" network profile, the Windows Defender Firewall automatically blocks incoming ICMP Echo Requests (ping). By default, Nmap pings a host before scanning it. If the host drops the ping, Nmap assumes the machine is offline and aborts the port scan. Using `-Pn` forces Nmap to skip the ping phase and directly attack the ports, bypassing this basic firewall restriction.

```text
Starting Nmap 7.94SVN ( [https://nmap.org](https://nmap.org) ) at 2024-11-16 12:50 EST
Nmap scan report for 192.168.70.21
Host is up.
Not shown: 994 filtered tcp ports (no-response)
PORT     STATE SERVICE
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
5040/tcp  open  unknown
7680/tcp  open  pando-pub

Nmap done: 1 IP address (1 host up) scanned in 45.12 seconds
```

---
### 7- Perform an Aggressive Scan
`nmap -A 192.168.70.22`
- `-A` Enables OS detection, version detection, script scanning, and traceroute.

This is a noisy, comprehensive scan that gives you a massive amount of information by combining several Nmap features into a single command. It is highly detectable by Intrusion Detection Systems (IDS).

```
Starting Nmap 7.94SVN ( [https://nmap.org](https://nmap.org) ) at 2024-11-16 12:55 EST
Nmap scan report for metasploitable.localdomain (192.168.70.22)
Host is up (0.00041s latency).
Not shown: 977 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
21/tcp   open  ftp     vsftpd 2.3.4
|_ftp-anon: Anonymous FTP login allowed (FTP code 230)
22/tcp   open  ssh     OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
| ssh-hostkey: 
|   1024 60:0f:cf:e1:c0:5f:6a:74:d6:90:24:fa:c4:d5:6c:cd (DSA)
|_  2048 56:56:24:0f:21:1d:de:a7:2b:ae:61:b1:24:3d:e8:f3 (RSA)
...
OS and Service detection performed. Please report any incorrect results at [https://nmap.org/submit/](https://nmap.org/submit/) .
Nmap done: 1 IP address (1 host up) scanned in 72.15 seconds
```

---

### 8- Vulnerability Scanning using NSE (Nmap Scripting Engine)
`nmap --script vuln 192.168.70.21`

- `--script vuln` Loads a category of Nmap scripts designed specifically to check target services for known vulnerabilities and exploits.

Nmap is not just a port scanner; its scripting engine allows it to act as a lightweight vulnerability scanner.


```
Starting Nmap 7.94SVN ( [https://nmap.org](https://nmap.org) ) at 2024-11-16 13:00 EST
Nmap scan report for Windows7 (192.168.70.21)
Host is up (0.0051s latency).
PORT    STATE SERVICE
445/tcp open  microsoft-ds

Host script results:
| smb-vuln-ms17-010: 
|   VULNERABLE:
|   Remote Code Execution vulnerability in Microsoft SMBv1 servers (ms17-010)
|     State: VULNERABLE
|     IDs:  CVE:CVE-2017-0143
|     Risk factor: HIGH
|       A critical remote code execution vulnerability exists in Microsoft SMBv1...
```

---
### 9- Perform a Stealth Xmas Scan (Firewall Evasion)
`nmap -sX 192.168.70.22`
- `-sX` Sets the FIN, PSH, and URG flags in the TCP packet (lighting it up "like a Christmas tree").   

This is a classic stealth scanning technique used to bypass stateless firewalls and basic packet filters. If the target system follows RFC 793, closed ports will respond with an RST packet, while open or filtered ports will drop the packet and send no response. (Note: This does not work against modern Windows systems, which reply with RST to all Xmas scans regardless of port state).

```
Starting Nmap 7.94SVN ( [https://nmap.org](https://nmap.org) ) at 2024-11-16 13:05 EST
Nmap scan report for 192.168.70.22
Host is up (0.00035s latency).
Not shown: 977 closed tcp ports (reset)
PORT     STATE         SERVICE
21/tcp   open|filtered ftp
22/tcp   open|filtered ssh
80/tcp   open|filtered http

Nmap done: 1 IP address (1 host up) scanned in 2.34 seconds
```

---
> For more questions email: nagy@aast.edu