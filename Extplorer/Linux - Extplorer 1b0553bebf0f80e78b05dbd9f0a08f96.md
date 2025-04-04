# Linux - Extplorer

---

### Host Info Gathering

- We are given an intermediate machine called Extplorer. Perform `nmap` scan over it.
    
    ```bash
    > nmap -sCV -A -Pn -O -p- 192.168.241.16 -oN tcpnmap.md
    
    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   3072 98:4e:5d:e1:e6:97:29:6f:d9:e0:d4:82:a8:f6:4f:3f (RSA)
    |   256 57:23:57:1f:fd:77:06:be:25:66:61:14:6d:ae:5e:98 (ECDSA)
    |_  256 c7:9b:aa:d5:a6:33:35:91:34:1e:ef:cf:61:a8:30:1c (ED25519)
    80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
    |_http-server-header: Apache/2.4.41 (Ubuntu)
    Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
    Device type: general purpose|router
    Running (JUST GUESSING): Linux 4.X|5.X|2.6.X|3.X (97%), MikroTik RouterOS 7.X (97%)
    OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5 cpe:/o:mikrotik:routeros:7 cpe:/o:linux:linux_kernel:5.6.3 cpe:/o:linux:linux_kernel:2.6 cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:6.0
    Aggressive OS guesses: Linux 4.15 - 5.19 (97%), Linux 5.0 - 5.14 (97%), MikroTik RouterOS 7.2 - 7.5 (Linux 5.6.3) (97%), Linux 2.6.32 - 3.13 (91%), Linux 3.10 - 4.11 (91%), Linux 3.2 - 4.14 (91%), Linux 3.4 - 3.10 (91%), Linux 4.15 (91%), Linux 2.6.32 - 3.10 (91%), Linux 4.19 - 5.15 (91%)
    No exact OS matches for host (test conditions non-ideal).
    Network Distance: 4 hops
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
    ```
    

### Important Info

```bash
extplorer v 2.1.15. # File upload -> reverse_shell.php

/filemanager/config/.htusers.php # User credentials

admin : admin
dora  : doraemon

root : explorer
```

### Initial Foothold

- `port 80` is open with http server. We can visit and it will redirect us to a `WordPress` page.
    
    Use `diresearch` , we can get some interesting directory. The `/filemanager/` path will display a login page for extplorer.
    
    ![image.png](Linux%20-%20Extplorer%201b0553bebf0f80e78b05dbd9f0a08f96/image.png)
    
    ![image.png](Linux%20-%20Extplorer%201b0553bebf0f80e78b05dbd9f0a08f96/image%201.png)
    
    ![image.png](Linux%20-%20Extplorer%201b0553bebf0f80e78b05dbd9f0a08f96/image%202.png)
    

- By trying weak password `admin : admin` , we are able to login to the file manager.
    
    ![image.png](Linux%20-%20Extplorer%201b0553bebf0f80e78b05dbd9f0a08f96/image%203.png)
    
    From `filemanager/CHANGELOG.txt` , we can see the `extplorer version 2.1.15` . Google related information, we find info says “eXtplorer 2.1.15 is vulnerable to file upload” and get CVE-2023-29657.
    
    - https://github.com/advisories/GHSA-9337-wvr6-wx8x
    
    ![image.png](Linux%20-%20Extplorer%201b0553bebf0f80e78b05dbd9f0a08f96/image%204.png)
    
- Also, with more enumeration, we found some user credentials under `filemanager/config/.htusers.php` . We can use `hash-identifier` and `hashid` to find their hash types.
    
    ```bash
    admin : 21232f297a57a5a743894a0e4a801fc3
    dora  : $2a$08$zyiNvVoP/UuSMgO2rKDtLuox.vYj.3hZPVYq3i4oG3/CtgET7CjjS
    ```
    
    ![image.png](Linux%20-%20Extplorer%201b0553bebf0f80e78b05dbd9f0a08f96/image%205.png)
    
    ![image.png](Linux%20-%20Extplorer%201b0553bebf0f80e78b05dbd9f0a08f96/image%206.png)
    
    ![image.png](Linux%20-%20Extplorer%201b0553bebf0f80e78b05dbd9f0a08f96/image%207.png)
    

- Use `hashcat` to decrypt two hashes we found.
    
    ```bash
    > echo "21232f297a57a5a743894a0e4a801fc3" > admin.hash
    > hashcat -m 0 admin.hash /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule --force
    
    admin : admin
    
    > echo "$2a$08$zyiNvVoP/UuSMgO2rKDtLuox.vYj.3hZPVYq3i4oG3/CtgET7CjjS" > dora.hash
    > hashcat --help | grep -i "bcrypt"
    > hashcat -m 3200 dora.hash /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule --force
    
    dora : doraemon
    ```
    
    ![image.png](Linux%20-%20Extplorer%201b0553bebf0f80e78b05dbd9f0a08f96/image%208.png)
    
    ![image.png](Linux%20-%20Extplorer%201b0553bebf0f80e78b05dbd9f0a08f96/image%209.png)
    
    ![image.png](Linux%20-%20Extplorer%201b0553bebf0f80e78b05dbd9f0a08f96/image%2010.png)
    

- The CVE-related blog is no longer reachable. We can try to upload our reverse_shell.php to the target and execute it. Listen on Kali and we can get a reverse shell as `www-data` , a low-privilege shell.
    
    ![image.png](Linux%20-%20Extplorer%201b0553bebf0f80e78b05dbd9f0a08f96/image%2011.png)
    
    ![image.png](Linux%20-%20Extplorer%201b0553bebf0f80e78b05dbd9f0a08f96/image%2012.png)
    
    ![image.png](Linux%20-%20Extplorer%201b0553bebf0f80e78b05dbd9f0a08f96/image%2013.png)
    

- We can find `dora` is a user on target machine. With the password we found , we can switch to `dora` . Then find local.txt.
    
    ```bash
    > su dora
    	: doraemon
    	
    - local.txt : 02c994fb16ba84646461e76cea971031
    ```
    

### Privilege Escalation

- Running `linpeas.sh` on target machine, we can find useful information.
    
    ![image.png](Linux%20-%20Extplorer%201b0553bebf0f80e78b05dbd9f0a08f96/image%2014.png)
    

- Goole “disk group privilege escalation”, we can find this useful information. - https://www.hackingarticles.in/disk-group-privilege-escalation/
    
    Find the disk that we have root privilege, enter debug mode. We can find `root` credential.
    
    ![image.png](Linux%20-%20Extplorer%201b0553bebf0f80e78b05dbd9f0a08f96/image%2015.png)
    
    ![image.png](Linux%20-%20Extplorer%201b0553bebf0f80e78b05dbd9f0a08f96/image%2016.png)
    

- Crack `root` password. Then we can switch to `root` with high privilege.
    
    ```bash
    > echo "$6$AIWcIr8PEVxEWgv1$3mFpTQAc9Kzp4BGUQ2sPYYFE/dygqhDiv2Yw.XcU.Q8n1YO05.a/4.D/x4ojQAkPnv/v7Qrw7Ici7.hs0sZiC." > root.hash
    
    # method 1
    > john --wordlist=/usr/share/wordlists/rockyou.txt root.hash
    
    # method 2
    > hashid -> SHA-512 Crypt
    > hashcat --help | grep -i "crypt"
    > hashcat -m 1800 root.hash /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule --force
    
    root : explorer
    
    - proof.txt : 05baa89abd7a0f9a05e257df35b0bc5a
    ```
    
    ![image.png](Linux%20-%20Extplorer%201b0553bebf0f80e78b05dbd9f0a08f96/image%2017.png)
    
    ![image.png](Linux%20-%20Extplorer%201b0553bebf0f80e78b05dbd9f0a08f96/image%2018.png)
    
    ![image.png](Linux%20-%20Extplorer%201b0553bebf0f80e78b05dbd9f0a08f96/image%2019.png)
    

### Reference

- https://github.com/advisories/GHSA-9337-wvr6-wx8x
- https://nvd.nist.gov/vuln/detail/CVE-2023-29657
- https://www.hackingarticles.in/disk-group-privilege-escalation/