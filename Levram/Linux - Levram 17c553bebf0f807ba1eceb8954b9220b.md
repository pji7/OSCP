# Linux - Levram

---

### Host Info

```bash
192.168.124.24
```

### Scan

```bash
# Nmap 7.95 scan initiated Wed Jan 15 02:29:16 2025 as: /usr/lib/nmap/nmap --privileged -sCV -O -A -p- -oN nmap.md 192.168.124.24
Nmap scan report for 192.168.124.24
Host is up (0.036s latency).
Not shown: 65533 closed tcp ports (reset)
PORT     STATE SERVICE VERSION

22/tcp   open  ssh     OpenSSH 8.9p1 Ubuntu 3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 b9:bc:8f:01:3f:85:5d:f9:5c:d9:fb:b6:15:a0:1e:74 (ECDSA)
|_  256 53:d9:7f:3d:22:8a:fd:57:98:fe:6b:1a:4c:ac:79:67 (ED25519)

8000/tcp open  http    WSGIServer 0.2 (Python 3.10.6)
|_http-title: Gerapy
|_http-cors: GET POST PUT DELETE OPTIONS PATCH
|_http-server-header: WSGIServer/0.2 CPython/3.10.6

Device type: general purpose|router
Running: Linux 5.X, MikroTik RouterOS 7.X
OS CPE: cpe:/o:linux:linux_kernel:5 cpe:/o:mikrotik:routeros:7 cpe:/o:linux:linux_kernel:5.6.3
OS details: Linux 5.0 - 5.14, MikroTik RouterOS 7.2 - 7.5 (Linux 5.6.3)
Network Distance: 4 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 256/tcp)
HOP RTT      ADDRESS
1   33.36 ms 192.168.45.1
2   33.43 ms 192.168.45.254
3   33.46 ms 192.168.251.1
4   25.90 ms 192.168.124.24

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Wed Jan 15 02:30:12 2025 -- 1 IP address (1 host up) scanned in 55.97 seconds

```

---

## Walkthrough

- weak password on port 8080
    - admin : admin

![image.png](Linux%20-%20Levram%2017c553bebf0f807ba1eceb8954b9220b/image.png)

![image.png](Linux%20-%20Levram%2017c553bebf0f807ba1eceb8954b9220b/image%201.png)

https://www.exploit-db.com/exploits/50640

[https://github.com/hadrian3689/gerapy_0.97_rce](https://github.com/hadrian3689/gerapy_0.97_rce)

![image.png](Linux%20-%20Levram%2017c553bebf0f807ba1eceb8954b9220b/image%202.png)

![image.png](Linux%20-%20Levram%2017c553bebf0f807ba1eceb8954b9220b/image%203.png)

![image.png](Linux%20-%20Levram%2017c553bebf0f807ba1eceb8954b9220b/image%204.png)

72ec03634d7736febe38ef2716868e6c

```bash
find / -perm -u+s 2>/dev/null
/snap/snapd/19361/usr/lib/snapd/snap-confine
/snap/core20/1518/usr/bin/chfn
/snap/core20/1518/usr/bin/chsh
/snap/core20/1518/usr/bin/gpasswd
/snap/core20/1518/usr/bin/mount
/snap/core20/1518/usr/bin/newgrp
/snap/core20/1518/usr/bin/passwd
/snap/core20/1518/usr/bin/su
/snap/core20/1518/usr/bin/sudo
/snap/core20/1518/usr/bin/umount
/snap/core20/1518/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/snap/core20/1518/usr/lib/openssh/ssh-keysign
/snap/core20/1891/usr/bin/chfn
/snap/core20/1891/usr/bin/chsh
/snap/core20/1891/usr/bin/gpasswd
/snap/core20/1891/usr/bin/mount
/snap/core20/1891/usr/bin/newgrp
/snap/core20/1891/usr/bin/passwd
/snap/core20/1891/usr/bin/su
/snap/core20/1891/usr/bin/sudo
/snap/core20/1891/usr/bin/umount
/snap/core20/1891/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/snap/core20/1891/usr/lib/openssh/ssh-keysign
/usr/libexec/polkit-agent-helper-1
/usr/lib/snapd/snap-confine
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/openssh/ssh-keysign
/usr/bin/su
/usr/bin/newgrp
/usr/bin/chsh
/usr/bin/chfn
/usr/bin/pkexec
/usr/bin/gpasswd
/usr/bin/fusermount3
/usr/bin/umount
/usr/bin/passwd
/usr/bin/mount
/usr/bin/sudo
```

- linpeas scan

![image.png](Linux%20-%20Levram%2017c553bebf0f807ba1eceb8954b9220b/image%205.png)

![image.png](Linux%20-%20Levram%2017c553bebf0f807ba1eceb8954b9220b/image%206.png)

![image.png](Linux%20-%20Levram%2017c553bebf0f807ba1eceb8954b9220b/image%207.png)

![image.png](Linux%20-%20Levram%2017c553bebf0f807ba1eceb8954b9220b/image%208.png)

ad881d639a11493aa4d4683af3b8b503