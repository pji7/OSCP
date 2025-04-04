# Linux - Ochima

---

### Host

```bash
192.168.245.32
```

### Scan

```bash
Starting Nmap 7.95 ( https://nmap.org ) at 2025-01-17 21:55 EST
Nmap scan report for 192.168.245.32
Host is up (0.036s latency).
Not shown: 65532 filtered tcp ports (no-response)
PORT     STATE SERVICE VERSION

22/tcp   open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 b9:bc:8f:01:3f:85:5d:f9:5c:d9:fb:b6:15:a0:1e:74 (ECDSA)
|_  256 53:d9:7f:3d:22:8a:fd:57:98:fe:6b:1a:4c:ac:79:67 (ED25519)

80/tcp   open  http    Apache httpd 2.4.52 ((Ubuntu))
|_http-server-header: Apache/2.4.52 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works

8338/tcp open  http    Python http.server 3.5 - 3.10
|_http-title: Maltrail
|_http-server-header: Maltrail/0.52
| http-robots.txt: 1 disallowed entry 
|_/

Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose|router
Running (JUST GUESSING): Linux 4.X|5.X|2.6.X|3.X (97%), MikroTik RouterOS 7.X (97%)
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5 cpe:/o:mikrotik:routeros:7 cpe:/o:linux:linux_kernel:5.6.3 cpe:/o:linux:linux_kernel:2.6 cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:6.0
Aggressive OS guesses: Linux 4.15 - 5.19 (97%), Linux 5.0 - 5.14 (97%), MikroTik RouterOS 7.2 - 7.5 (Linux 5.6.3) (97%), Linux 2.6.32 - 3.13 (91%), Linux 3.10 - 4.11 (91%), Linux 3.2 - 4.14 (91%), Linux 3.4 - 3.10 (91%), Linux 4.15 (91%), Linux 2.6.32 - 3.10 (91%), Linux 4.19 - 5.15 (91%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 4 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 80/tcp)
HOP RTT      ADDRESS
1   33.44 ms 192.168.45.1
2   33.49 ms 192.168.45.254
3   34.83 ms 192.168.251.1
4   35.42 ms 192.168.245.32

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 176.96 seconds

```

---

## Walkthrough

![image.png](Linux%20-%20Ochima%2017f553bebf0f80b5a71bfee6274cb8c5/image.png)

![image.png](Linux%20-%20Ochima%2017f553bebf0f80b5a71bfee6274cb8c5/image%201.png)

![image.png](Linux%20-%20Ochima%2017f553bebf0f80b5a71bfee6274cb8c5/image%202.png)

- search related information

- attempt #1

[https://github.com/spookier/Maltrail-v0.53-Exploit](https://github.com/spookier/Maltrail-v0.53-Exploit)

![image.png](Linux%20-%20Ochima%2017f553bebf0f80b5a71bfee6274cb8c5/image%203.png)

![image.png](Linux%20-%20Ochima%2017f553bebf0f80b5a71bfee6274cb8c5/image%204.png)

- we have the reverse shell

![image.png](Linux%20-%20Ochima%2017f553bebf0f80b5a71bfee6274cb8c5/image%205.png)

e179705369aa00c31bc1bf542d894184

![image.png](Linux%20-%20Ochima%2017f553bebf0f80b5a71bfee6274cb8c5/image%206.png)

![image.png](Linux%20-%20Ochima%2017f553bebf0f80b5a71bfee6274cb8c5/image%207.png)

![image.png](Linux%20-%20Ochima%2017f553bebf0f80b5a71bfee6274cb8c5/image%208.png)

![image.png](Linux%20-%20Ochima%2017f553bebf0f80b5a71bfee6274cb8c5/image%209.png)

172a434e55323c60963ffd3201f543f1