# Linux - pyLoader

---

### Host

```bash
192.168.132.26
```

### Scan

```bash
Starting Nmap 7.95 ( https://nmap.org ) at 2025-01-18 16:19 EST
Nmap scan report for 192.168.132.26
Host is up (0.024s latency).
Not shown: 65533 closed tcp ports (reset)
PORT     STATE SERVICE VERSION

22/tcp   open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 b9:bc:8f:01:3f:85:5d:f9:5c:d9:fb:b6:15:a0:1e:74 (ECDSA)
|_  256 53:d9:7f:3d:22:8a:fd:57:98:fe:6b:1a:4c:ac:79:67 (ED25519)

9666/tcp open  http    CherryPy wsgiserver
| http-title: Login - pyLoad 
|_Requested resource was /login?next=http://192.168.132.26:9666/
| http-robots.txt: 1 disallowed entry 
|_/
|_http-server-header: Cheroot/8.6.0

Device type: general purpose|router
Running: Linux 5.X, MikroTik RouterOS 7.X
OS CPE: cpe:/o:linux:linux_kernel:5 cpe:/o:mikrotik:routeros:7 cpe:/o:linux:linux_kernel:5.6.3
OS details: Linux 5.0 - 5.14, MikroTik RouterOS 7.2 - 7.5 (Linux 5.6.3)
Network Distance: 4 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 111/tcp)
HOP RTT      ADDRESS
1   21.95 ms 192.168.45.1
2   21.92 ms 192.168.45.254
3   26.04 ms 192.168.251.1
4   26.14 ms 192.168.132.26

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 57.96 seconds

```

---

## Walkthrough

![image.png](Linux%20-%20pyLoader%2017f553bebf0f806b9dabd6e98b10d8db/image.png)

![image.png](Linux%20-%20pyLoader%2017f553bebf0f806b9dabd6e98b10d8db/image%201.png)

![image.png](Linux%20-%20pyLoader%2017f553bebf0f806b9dabd6e98b10d8db/image%202.png)

![image.png](Linux%20-%20pyLoader%2017f553bebf0f806b9dabd6e98b10d8db/image%203.png)

![image.png](Linux%20-%20pyLoader%2017f553bebf0f806b9dabd6e98b10d8db/image%204.png)

- attempt #1 - searchsploit, exploit db, seems not working

https://www.exploit-db.com/exploits/51532

![image.png](Linux%20-%20pyLoader%2017f553bebf0f806b9dabd6e98b10d8db/image%205.png)

- attempt #2 - search CVE # on github, get new exploit.

[https://github.com/JacobEbben/CVE-2023-0297](https://github.com/JacobEbben/CVE-2023-0297)

![image.png](Linux%20-%20pyLoader%2017f553bebf0f806b9dabd6e98b10d8db/image%206.png)

![image.png](Linux%20-%20pyLoader%2017f553bebf0f806b9dabd6e98b10d8db/image%207.png)

b0de9722ccbb9462dc3c58c693184a69