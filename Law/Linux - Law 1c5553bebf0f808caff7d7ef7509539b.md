# Linux - Law

---

### Host Info Gathering

- Given a target of `192.168.192.190`, run `nmap` scan over it.
    
    ```bash
    > nmap -sCV -A -Pn -O -p- 192.168.192.190 -oN tcpnmap.md
    
    PORT   STATE SERVICE VERSION
    
    22/tcp open  ssh     OpenSSH 8.4p1 Debian 5+deb11u1 (protocol 2.0)
    | ssh-hostkey: 
    |   3072 c9:c3:da:15:28:3b:f1:f8:9a:36:df:4d:36:6b:a7:44 (RSA)
    |   256 26:03:2b:f6:da:90:1d:1b:ec:8d:8f:8d:1e:7e:3d:6b (ECDSA)
    |_  256 fb:43:b2:b0:19:2f:d3:f6:bc:aa:60:67:ab:c1:af:37 (ED25519)
    
    80/tcp open  http    Apache httpd 2.4.56 ((Debian))
    |_http-server-header: Apache/2.4.56 (Debian)
    |_http-title: htmLawed (1.2.5) test
    
    ```
    

### Important Info

```bash
HTMLAWED 1.2.5 TEST
```

### Initial Foothold

- We found port `80` open, by visiting the web page, we can find a page of `HTMLAWED 1.2.5 TEST` hosting. Google related exploit and searchsploit it, we can find an RCE to it.
    
    https://www.exploit-db.com/exploits/52023
    
    ![image.png](Linux%20-%20Law%201c5553bebf0f808caff7d7ef7509539b/image.png)
    
- On kali, execute the exploit and we can get reverse shell as `www-data` and get `local.txt` .
    
    ![image.png](Linux%20-%20Law%201c5553bebf0f808caff7d7ef7509539b/image%201.png)
    
    ![image.png](Linux%20-%20Law%201c5553bebf0f808caff7d7ef7509539b/image%202.png)
    
    ![image.png](Linux%20-%20Law%201c5553bebf0f808caff7d7ef7509539b/image%203.png)
    

### Privilege Escalation

- Enumerating cron jobs, we found `/var/www/cleanup.sh` run by root user, but we can edit its content. So we can try to add a reverse shell payload into the content and start listening on kali machine.
    
    We end up getting the `root` privilege.
    
    ![image.png](Linux%20-%20Law%201c5553bebf0f808caff7d7ef7509539b/image%204.png)
    
    ![image.png](Linux%20-%20Law%201c5553bebf0f808caff7d7ef7509539b/image%205.png)
    

### Reference

- https://www.exploit-db.com/exploits/52023