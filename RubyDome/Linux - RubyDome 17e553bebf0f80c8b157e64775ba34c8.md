# Linux - RubyDome

---

### Host

```bash
192.168.136.22
```

### Scan

```bash
Starting Nmap 7.95 ( https://nmap.org ) at 2025-01-16 22:45 EST
Nmap scan report for 192.168.136.22
Host is up (0.035s latency).
Not shown: 65533 closed tcp ports (reset)
PORT     STATE SERVICE VERSION

22/tcp   open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 b9:bc:8f:01:3f:85:5d:f9:5c:d9:fb:b6:15:a0:1e:74 (ECDSA)
|_  256 53:d9:7f:3d:22:8a:fd:57:98:fe:6b:1a:4c:ac:79:67 (ED25519)

3000/tcp open  http    WEBrick httpd 1.7.0 (Ruby 3.0.2 (2021-07-07))
|_http-title: RubyDome HTML to PDF
|_http-server-header: WEBrick/1.7.0 (Ruby/3.0.2/2021-07-07)

Device type: general purpose|router
Running: Linux 5.X, MikroTik RouterOS 7.X
OS CPE: cpe:/o:linux:linux_kernel:5 cpe:/o:mikrotik:routeros:7 cpe:/o:linux:linux_kernel:5.6.3
OS details: Linux 5.0 - 5.14, MikroTik RouterOS 7.2 - 7.5 (Linux 5.6.3)
Network Distance: 4 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 85.16 seconds
```

---

## Walkthrough

![image.png](Linux%20-%20RubyDome%2017e553bebf0f80c8b157e64775ba34c8/image.png)

- try [http://127.0.0.1](http://127.0.0.1)

![image.png](Linux%20-%20RubyDome%2017e553bebf0f80c8b157e64775ba34c8/image%201.png)

[https://github.com/UNICORDev/exploit-CVE-2022-25765](https://github.com/UNICORDev/exploit-CVE-2022-25765)

- attempt #1

![image.png](Linux%20-%20RubyDome%2017e553bebf0f80c8b157e64775ba34c8/image%202.png)

![image.png](Linux%20-%20RubyDome%2017e553bebf0f80c8b157e64775ba34c8/image%203.png)

![image.png](Linux%20-%20RubyDome%2017e553bebf0f80c8b157e64775ba34c8/image%204.png)

![image.png](Linux%20-%20RubyDome%2017e553bebf0f80c8b157e64775ba34c8/image%205.png)

4dea23a15927da71235b997462d7c4be

![image.png](Linux%20-%20RubyDome%2017e553bebf0f80c8b157e64775ba34c8/image%206.png)

![image.png](Linux%20-%20RubyDome%2017e553bebf0f80c8b157e64775ba34c8/image%207.png)

101c3cbd842effe9dcd1e87d77dee222