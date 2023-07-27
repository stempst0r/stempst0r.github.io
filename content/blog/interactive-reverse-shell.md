---
title: "Interactive Reverse Shell"
date: 2023-07-27T08:49:15Z
draft: false
---

Had some issues getting a fully functional reverse shell on Kali 2023.2. 
I think the main issue was caused by the standard shell zsh (not good old bash).

Maybe this post will help me again, or even others :)

# Workaround 1
---
Simply switch from zsh to bash:
```
exec bash --login               #switch to bash shell
ps -p $$                        #check used shell
```
and then use the *standards* from [HackTricks - Full TTYs](https://book.hacktricks.xyz/generic-methodologies-and-resources/shells/full-ttys).

# Workaround 2
---
With some tweaking it was possible to get it running with zsh:

On attacker:
```
rlwrap nc -lvnp {your_port}
```
- `rlwrap`: provides readline's line edition, persistent history and completion

on rev shell:
```
python3 -c 'import pty; pty.spawn("/bin/bash")'
ctrl-z
```
- stabilize shell
- put it to background

on attacker:
```
stty size;stty raw -echo;fg
```
- important to send this commands as an oneliner
