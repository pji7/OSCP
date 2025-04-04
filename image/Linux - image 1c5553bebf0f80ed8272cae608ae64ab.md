# Linux - image

---

### Host Info Gathering

- Given a target machine `192.168.120.178` and run `nmap` scan over it.
    
    ```bash
    > nmap -sCV -A -Pn -O -p- 192.168.120.178 -oN tcpnmap.md
    
    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.7 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   3072 62:36:1a:5c:d3:e3:7b:e1:70:f8:a3:b3:1c:4c:24:38 (RSA)
    |   256 ee:25:fc:23:66:05:c0:c1:ec:47:c6:bb:00:c7:4f:53 (ECDSA)
    |_  256 83:5c:51:ac:32:e5:3a:21:7c:f6:c2:cd:93:68:58:d8 (ED25519)
    80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
    |_http-title: ImageMagick Identifier
    |_http-server-header: Apache/2.4.41 (Ubuntu)
    ```
    

### Important Info

```bash
ImageMagick identifier version: 6.9.6-4
/usr/bin/strace -> SUID
```

### Initial Foothold

- We found port `80` open, by visiting the page, we find a file upload web page called `ImageMagick Identifier` . After uploading an image, we can get its version info.
    
    ![image.png](Linux%20-%20image%201c5553bebf0f80ed8272cae608ae64ab/image.png)
    
- Then google `ImageMagick Identifier exploit` , we are able to find some useful resources.
    
    [https://github.com/ImageMagick/ImageMagick/issues/6339](https://github.com/ImageMagick/ImageMagick/issues/6339)
    
    ![image.png](Linux%20-%20image%201c5553bebf0f80ed8272cae608ae64ab/image%201.png)
    

- Use any picture, write base64 encode payload to the new `.jpeg` file and upload it to the server. Start listening on kali machine. We are able to get reverse shell as `www-data` .
    
    ![image.png](Linux%20-%20image%201c5553bebf0f80ed8272cae608ae64ab/image%202.png)
    
    ![image.png](Linux%20-%20image%201c5553bebf0f80ed8272cae608ae64ab/image%202.png)
    

### Privilege Escalation

- After we get the initial foothold, use `linpeas.sh` to scan over the target machine. We can found highlighted `strace` under `/user/bin` path. Go to `GTFOBins` and search for related info, we found a `SUID` .
    
    ![image.png](Linux%20-%20image%201c5553bebf0f80ed8272cae608ae64ab/image%203.png)
    
    ![image.png](Linux%20-%20image%201c5553bebf0f80ed8272cae608ae64ab/image%204.png)
    
- Follow above command to get `root` privilege.
    
    ![image.png](Linux%20-%20image%201c5553bebf0f80ed8272cae608ae64ab/image%205.png)
    
    ![image.png](Linux%20-%20image%201c5553bebf0f80ed8272cae608ae64ab/image%206.png)
    
    ![image.png](Linux%20-%20image%201c5553bebf0f80ed8272cae608ae64ab/image%207.png)
    

### Reference

- https://github.com/ImageMagick/ImageMagick/issues/6339
- https://medium.com/@vineeth.dj/proving-grounds-image-walkthrough-cf7e0b09ba93