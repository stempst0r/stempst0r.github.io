---
title: "HTB Weak RSA"
date: 2023-08-07T06:47:27Z
draft: false
---

# Info
---
This is my writeup for the HTB challenge [Weak RSA](https://app.hackthebox.com/challenges/6).

I'll use Kali 2023.2 running in a VM for this.

# Gaining the private key
---
First we download, compare the checksum and unzip the files.

```
$ sha256sum Weak\ RSA.zip 
1cbf890e7a0fe8b404597b565da96c388e5653937631e2dc8710ede9d15bdb7d  Weak RSA.zip

$ unzip -P hackthebox Weak\ RSA.zip 
Archive:  Weak RSA.zip
  inflating: flag.enc                
  inflating: key.pub 
```
The checksum is correct and there are two files inside the zip. 
`flag.enc` seems to contain a  encrypted message and `key.pub` seems to contain a  public key from a asymetric encryption method.

After some search I found this very useful script called [RsaCtfTool](https://github.com/RsaCtfTool/RsaCtfTool). 
We will install it:
```
git clone https://github.com/RsaCtfTool/RsaCtfTool.git
sudo apt-get install libgmp3-dev libmpc-dev
cd RsaCtfTool
pip3 install -r "requirements.txt"
./RsaCtfTool.py
```

Now we let run the script against the private key:
```
$ ./RsaCtfTool.py --publickey ~/Downloads/key.pub --private
['/home/kali/Downloads/key.pub']

[*] Testing key /home/kali/Downloads/key.pub.
attack initialized...
attack initialized...
[*] Performing factordb attack on /home/kali/Downloads/key.pub.
[*] Attack success with factordb method !

Results for /home/kali/Downloads/key.pub:

Private key :
-----BEGIN RSA PRIVATE KEY-----
MIICOQIBAAKBgQMwO3kPsUnaNAbUlaubn7ip4pNEXjvUOxjvLwUhtybr6Ng4undL
tSQPCPf7ygoUKh1KYeqXMpTmhKjRos3xioTy23CZuOl3WIsLiRKSVYyqBc9d8rxj
NMXuUIOiNO38ealcR4p44zfHI66INPuKmTG3RQP/6p5hv1PYcWmErEeDewKBgGEX
xgRIsTlFGrW2C2JXoSvakMCWD60eAH0W2PpDqlqqOFD8JA5UFK0roQkOjhLWSVu8
c6DLpWJQQlXHPqP702qIg/gx2o0bm4EzrCEJ4gYo6Ax+U7q6TOWhQpiBHnC0ojE8
kUoqMhfALpUaruTJ6zmj8IA1e1M6bMqVF8srlb/NAiBhwngxi+Cbie3YBogNzGJV
h10vAgw+i7cQqiiwEiPFNQJBAYXzr5r2KkHVjGcZNCLRAoXrzJjVhb7knZE5oEYo
nEI+h2gQSt1bavv3YVxhcisTVuNrlgQo58eGb4c9dtY2blMCQQIX2W9IbtJ26KzZ
C/5HPsVqgxWtuP5hN8OLf3ohhojr1NigJwc6o68dtKScaEQ5A33vmNpuWqKucecT
0HEVxuE5AiBhwngxi+Cbie3YBogNzGJVh10vAgw+i7cQqiiwEiPFNQIgYcJ4MYvg
m4nt2AaIDcxiVYddLwIMPou3EKoosBIjxTUCQQCnqbJMPEQHpg5lI6MQi8ixFRqo
+KwoBrwYfZlGEwZxdK2Ms0jgeta5jFFS11Fwk5+GyimnRzVcEbADJno/8BKe
-----END RSA PRIVATE KEY-----
```

Jackpot! :) We got the private key. Let's store it into a file:

```
$ echo "-----BEGIN RSA PRIVATE KEY-----                           
MIICOQIBAAKBgQMwO3kPsUnaNAbUlaubn7ip4pNEXjvUOxjvLwUhtybr6Ng4undL
tSQPCPf7ygoUKh1KYeqXMpTmhKjRos3xioTy23CZuOl3WIsLiRKSVYyqBc9d8rxj
NMXuUIOiNO38ealcR4p44zfHI66INPuKmTG3RQP/6p5hv1PYcWmErEeDewKBgGEX
xgRIsTlFGrW2C2JXoSvakMCWD60eAH0W2PpDqlqqOFD8JA5UFK0roQkOjhLWSVu8
c6DLpWJQQlXHPqP702qIg/gx2o0bm4EzrCEJ4gYo6Ax+U7q6TOWhQpiBHnC0ojE8
kUoqMhfALpUaruTJ6zmj8IA1e1M6bMqVF8srlb/NAiBhwngxi+Cbie3YBogNzGJV
h10vAgw+i7cQqiiwEiPFNQJBAYXzr5r2KkHVjGcZNCLRAoXrzJjVhb7knZE5oEYo
nEI+h2gQSt1bavv3YVxhcisTVuNrlgQo58eGb4c9dtY2blMCQQIX2W9IbtJ26KzZ
C/5HPsVqgxWtuP5hN8OLf3ohhojr1NigJwc6o68dtKScaEQ5A33vmNpuWqKucecT
0HEVxuE5AiBhwngxi+Cbie3YBogNzGJVh10vAgw+i7cQqiiwEiPFNQIgYcJ4MYvg
m4nt2AaIDcxiVYddLwIMPou3EKoosBIjxTUCQQCnqbJMPEQHpg5lI6MQi8ixFRqo
+KwoBrwYfZlGEwZxdK2Ms0jgeta5jFFS11Fwk5+GyimnRzVcEbADJno/8BKe
-----END RSA PRIVATE KEY-----" > priv.key

```

# Decrypting the file
---
## Option 1: Using openssl
After some trying, it was possible to decrypt `flag.enc` with the following command:

```
$ openssl pkeyutl -in flag.enc -out flag.txt -decrypt -inkey priv.key
$ cat flag.txt 
HTB{s1mpl3_Wi3n3rs_4tt4ck}
```

## Option 2: Using RsaCtfTool
It is also possible to simply use the RsaCtfTool from before:
```
$ ./RsaCtfTool.py --publickey ~/Downloads/key.pub --uncipherfile ~/Downloads/flag.enc 
private argument is not set, the private key will not be displayed, even if recovered.
['/home/kali/Downloads/key.pub']

[*] Testing key /home/kali/Downloads/key.pub.
attack initialized...
attack initialized...
[*] Performing factordb attack on /home/kali/Downloads/key.pub.
[*] Attack success with factordb method !

Results for /home/kali/Downloads/key.pub:

Unciphered data :
HEX : 0x000221cfb29883b06f409a679a58a4e97b446e28b244bbcd0687d178a8ab8722bf86da06a62e042c892d2921b336571e9ff7ac9d89ba90512bac4cfb8d7e4a3901bbccf5dfac01b27bddd35f1ca55344a75943df9a18eadb344cf7cf55fa0baa7005bfe32f41004854427b73316d706c335f5769336e3372735f34747434636b7d
INT (big endian) : 1497194306832430076266314478305730170974165912795150306640063107539292495904192020114449824357438113183764256783752233913408135242464239912689425668318419718061442061010640167802145162377597484106658670422900749326253337728846324798012274989739031662527650589811318528908253458824763561374522387177140349821
INT (little endian) : 22546574266225300968123857704721191858671593287972919965619572675918636257464402082642870677657579044805501825719744981953609630743396909394906721219496019830622451770590549653716476856077849644487076110495020954617170743371827481017047908786316114794508942268154434710618690751442928771926238749045133355844096
STR : b'\x00\x02!\xcf\xb2\x98\x83\xb0o@\x9ag\x9aX\xa4\xe9{Dn(\xb2D\xbb\xcd\x06\x87\xd1x\xa8\xab\x87"\xbf\x86\xda\x06\xa6.\x04,\x89-)!\xb36W\x1e\x9f\xf7\xac\x9d\x89\xba\x90Q+\xacL\xfb\x8d~J9\x01\xbb\xcc\xf5\xdf\xac\x01\xb2{\xdd\xd3_\x1c\xa5SD\xa7YC\xdf\x9a\x18\xea\xdb4L\xf7\xcfU\xfa\x0b\xaap\x05\xbf\xe3/A\x00HTB{s1mpl3_Wi3n3rs_4tt4ck}'

PKCS#1.5 padding decoded!
HEX : 0x004854427b73316d706c335f5769336e3372735f34747434636b7d
INT (big endian) : 116228445871869252378692588205079217110932931184359462733572989
INT (little endian) : 51594582506285564025554597946778804341308607376857173453085886464
utf-8 : HTB{s1mpl3_Wi3n3rs_4tt4ck}
STR : b'\x00HTB{s1mpl3_Wi3n3rs_4tt4ck}'
```
Happy hacking :)
