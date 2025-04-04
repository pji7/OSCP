# Win - Algernon

---

### Host

```bash
192.168.238.65
```

### Scan

```bash
# Nmap 7.95 scan initiated Fri Jan 31 14:33:11 2025 as: /usr/lib/nmap/nmap --privileged -sCV -A -Pn -O -p- -oN nmap.md 192.168.238.65
PORT      STATE SERVICE       VERSION

21/tcp    open  ftp           Microsoft ftpd
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| 04-29-20  09:31PM       <DIR>          ImapRetrieval
| 01-31-25  11:18AM       <DIR>          Logs
| 04-29-20  09:31PM       <DIR>          PopRetrieval
|_04-29-20  09:32PM       <DIR>          Spool

80/tcp    open  http          Microsoft IIS httpd 10.0

135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds?
5040/tcp  open  unknown

9998/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-IIS/10.0
| http-title: Site doesn't have a title (text/html; charset=utf-8).
|_Requested resource was /interface/root
| uptime-agent-info: HTTP/1.1 400 Bad Request\x0D
| Content-Type: text/html; charset=us-ascii\x0D
| Server: Microsoft-HTTPAPI/2.0\x0D
| Date: Fri, 31 Jan 2025 19:36:33 GMT\x0D
| Connection: close\x0D
| Content-Length: 326\x0D
| \x0D
| <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN""http://www.w3.org/TR/html4/strict.dtd">\x0D
| <HTML><HEAD><TITLE>Bad Request</TITLE>\x0D
| <META HTTP-EQUIV="Content-Type" Content="text/html; charset=us-ascii"></HEAD>\x0D
| <BODY><h2>Bad Request - Invalid Verb</h2>\x0D
| <hr><p>HTTP Error 400. The request verb is invalid.</p>\x0D
|_</BODY></HTML>\x0D

17001/tcp open  remoting      MS .NET Remoting services
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
49669/tcp open  msrpc         Microsoft Windows RPC
```

### Important Info

```bash

```

---

## Walkthrough

- First scan the target. Check open ports.

### 21 - FTP

- Try `21 - ftp` , nmap result shows it allows anonymous connection.
- We found 4 dir, but seems like we can only get into `Logs` .
- There are too many files in logs. We download it to kali, combine them, and check together.

![image.png](Win%20-%20Algernon%2018c553bebf0f8089a5d1dcd9cf192fa1/image.png)

```bash
ftp> mget *.log //下载所有log文件
> cat *.log > all_logs.txt //合并
> rm -rf *.log //删除log文件
```

- 检查所有合并log文件，并没有太多信息。

### 80

- Try port 80 → a simple welcome webpage, click on icons, not showing any useful info.

![image.png](Win%20-%20Algernon%2018c553bebf0f8089a5d1dcd9cf192fa1/image%201.png)

- Try `diresearch` over the URL, nothing interesting found.

### 9988

- SmarterMail login page → maybe a CMS?

![image.png](Win%20-%20Algernon%2018c553bebf0f8089a5d1dcd9cf192fa1/image%202.png)

- Try `dirsearch` over URL:9998, found something but not useful.
- Search for `SmarterMail` possible exploit.
- On exploitDB → we found a RCE exploit - https://www.exploit-db.com/exploits/49216
- Review the code, we need to modify the “HOST” “LHOST” “LPORT”. Confirm that .NET is hosting on `port 17001` , we can confirm it from our scan result.

![image.png](Win%20-%20Algernon%2018c553bebf0f8089a5d1dcd9cf192fa1/image%203.png)

- Run the script, we can get reverse shell.  We found that we are high privilege as `nt authroity\system` user.

![image.png](Win%20-%20Algernon%2018c553bebf0f8089a5d1dcd9cf192fa1/image%204.png)

![image.png](Win%20-%20Algernon%2018c553bebf0f8089a5d1dcd9cf192fa1/image%205.png)

- FLAG

```bash

- proof.txt : 19ada9e477f860ce824b2ce1aafe5961
```

```bash
where /R C:\ local.txt
dir C:\ /s /b | find "local.txt"
Get-ChildItem -Path C:\Users -Recurse -Filter "local.txt" -ErrorAction SilentlyContinue
```