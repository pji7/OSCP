# Linux - LaVita

---

### Host Info Gathering

- Given a target of `192.168.192.38`, run `nmap` scan over it.
    
    ```bash
    PORT   STATE SERVICE VERSION
    
    22/tcp open  ssh     OpenSSH 8.4p1 Debian 5+deb11u2 (protocol 2.0)
    | ssh-hostkey: 
    |   3072 c9:c3:da:15:28:3b:f1:f8:9a:36:df:4d:36:6b:a7:44 (RSA)
    |   256 26:03:2b:f6:da:90:1d:1b:ec:8d:8f:8d:1e:7e:3d:6b (ECDSA)
    |_  256 fb:43:b2:b0:19:2f:d3:f6:bc:aa:60:67:ab:c1:af:37 (ED25519)
    
    80/tcp open  http    Apache httpd 2.4.56 ((Debian))
    |_http-title: W3.CSS Template
    |_http-server-header: Apache/2.4.56 (Debian)
    ```
    

### Important Info

```bash
Laravel 8.4.0
```

### Initial Foothold

- We found port `80` open, by visiting the web page and doing some enumeration (URL discover by `dirsearch` `feroxbuster` , we can find the version info of `Laravel 8.4.0` . Google related exploit, we can find `CVE-2021-3129` , a RCE to Laravel.
    
    https://nvd.nist.gov/vuln/detail/cve-2021-3129
    
    https://www.exploit-db.com/exploits/49424
    
    [https://github.com/joshuavanderpoll/CVE-2021-3129](https://github.com/joshuavanderpoll/CVE-2021-3129)
    
    ![image.png](Linux%20-%20LaVita%201c5553bebf0f80cba860cbf3f96ef380/image.png)
    
- We can use any exploit from `ExploitDB` or `Github` , but such exploit requires debug mode to use. By URL discovering, we found a register url and register an account. Then login the account to enable the debug mode.
    
    I also find there’s image upload session on this page, so that I tried to upload some test files. However it seems not useful and give me no feedbacks.
    
    ![image.png](Linux%20-%20LaVita%201c5553bebf0f80cba860cbf3f96ef380/image%201.png)
    
- After enable the debug mode, run the exploit and we can get a reverse shell as `www-data` .
    
    ![image.png](Linux%20-%20LaVita%201c5553bebf0f80cba860cbf3f96ef380/image%202.png)
    
    ![image.png](Linux%20-%20LaVita%201c5553bebf0f80cba860cbf3f96ef380/image%203.png)
    
    ![image.png](Linux%20-%20LaVita%201c5553bebf0f80cba860cbf3f96ef380/image%204.png)
    

### Privilege Escalation

- After getting initial foothold, use `linpeas.sh` to scan over the target. We found that a user named `skunk` is belong to the sudo(27) group. This is an useful method.
    
    ![image.png](Linux%20-%20LaVita%201c5553bebf0f80cba860cbf3f96ef380/image%205.png)
    
- Enumerating cron jobs by using `pspy` , we found that `skunk` is constantly executing `/usr/bin/php  /var/www/html/lavita/artisan` , this is a php script and we are able to write content into it.
    
    ![image.png](Linux%20-%20LaVita%201c5553bebf0f80cba860cbf3f96ef380/image%206.png)
    
    Write a reverse shell into it and start listening on kali, we can get the shell as `skunk` .
    
    ![image.png](Linux%20-%20LaVita%201c5553bebf0f80cba860cbf3f96ef380/image%207.png)
    
    ![image.png](Linux%20-%20LaVita%201c5553bebf0f80cba860cbf3f96ef380/image%208.png)
    
    ![image.png](Linux%20-%20LaVita%201c5553bebf0f80cba860cbf3f96ef380/image%209.png)
    
    ![image.png](Linux%20-%20LaVita%201c5553bebf0f80cba860cbf3f96ef380/image%2010.png)
    

- By enumerating with `sudo -l` , we found we can execute `/usr/bin/composer` as root without password. Go to `GTFOBins` and we can find info about composer.
    
    Follow by instructions, we are able to get `root` privilege. 
    
    ![image.png](Linux%20-%20LaVita%201c5553bebf0f80cba860cbf3f96ef380/image%2011.png)
    
    ![image.png](Linux%20-%20LaVita%201c5553bebf0f80cba860cbf3f96ef380/image%2012.png)
    

### Reference

- https://nvd.nist.gov/vuln/detail/cve-2021-3129
- https://www.exploit-db.com/exploits/49424
- https://github.com/joshuavanderpoll/CVE-2021-3129?source=post_page-----12bfd272e9cf--------------------------------
- https://medium.com/@0xrave/ctf-200-07-offsec-proving-grounds-practice-labor-day-ctf-machine-walkthrough-12bfd272e9cf