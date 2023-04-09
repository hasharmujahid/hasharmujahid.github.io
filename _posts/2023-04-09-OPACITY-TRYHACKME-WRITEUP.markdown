---
title: "Opacity TryHackMe EASY"
layout: page
date: 2023-04-09
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

# NETWORK ENUMERATION:

```bash
Starting Nmap 7.93 ( https://nmap.org ) at 2023-04-09 10:23 PKT                                                                                                     [0/4]
Stats: 0:00:02 elapsed; 0 hosts completed (1 up), 1 undergoing SYN Stealth Scan                                                                                          
SYN Stealth Scan Timing: About 1.33% done; ETC: 10:23 (0:00:00 remaining)                                                                                                
Nmap scan report for 10.10.8.133          
Host is up (0.18s latency).               
Not shown: 65531 closed tcp ports (reset)                                           
PORT    STATE SERVICE     VERSION                                                   
22/tcp  open  ssh         OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)                                                                                   
| ssh-hostkey:                            
|   3072 0fee2910d98e8c53e64de3670c6ebee3 (RSA)                                     
|   256 9542cdfc712799392d0049ad1be4cf0e (ECDSA)                                    
|_  256 edfe9c94ca9c086ff25ca6cf4d3c8e5b (ED25519)                                  
80/tcp  open  http        Apache httpd 2.4.41 ((Ubuntu))                            
|_http-server-header: Apache/2.4.41 (Ubuntu)                                        
| http-title: Login                       
|_Requested resource was login.php                                                  
| http-cookie-flags:                      
|   /:                                    
|     PHPSESSID:                          
|_      httponly flag not set             
139/tcp open  netbios-ssn Samba smbd 4.6.2                                          
445/tcp open  netbios-ssn Samba smbd 4.6.2                                          
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel                             

Host script results:                      
|_nbstat: NetBIOS name: OPACITY, NetBIOS user: <unknown>, NetBIOS MAC: 000000000000 (Xerox)                                                                              
| smb2-security-mode:                     
|   311:                                  
|_    Message signing enabled but not required                                      
| smb2-time:                              
|   date: 2023-04-09T05:24:45             
|_  start_date: N/A                       

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .                                                                           
Nmap done: 1 IP address (1 host up) scanned in 74.52 seconds                        


```






# WEB ENUMERATION:


![](https://i.imgur.com/VoHvaT8.png)


> Directory Scan

```bash
 ╭─hax13@ZARB in ~/Documents/ctfs/thm/Opcity-Easy took 5ms
[🧱] × feroxbuster -u http://10.10.8.133/

 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 2.7.1
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.10.8.133/
 🚀  Threads               │ 50
 📖  Wordlist              │ /usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt
 👌  Status Codes          │ [200, 204, 301, 302, 307, 308, 401, 403, 405, 500]
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/2.7.1
 🏁  HTTP methods          │ [GET]
 🔃  Recursion Depth       │ 4
 🎉  New Version Available │ https://github.com/epi052/feroxbuster/releases/latest
───────────────────────────┴──────────────────────
 🏁  Press [ENTER] to use the Scan Management Menu™
──────────────────────────────────────────────────
302      GET        0l        0w        0c http://10.10.8.133/ => login.php
301      GET        9l       28w      308c http://10.10.8.133/css => http://10.10.8.133/css/
403      GET        9l       28w      276c http://10.10.8.133/server-status
301      GET        9l       28w      310c http://10.10.8.133/cloud => http://10.10.8.133/cloud/
301      GET        9l       28w      317c http://10.10.8.133/cloud/images => http://10.10.8.133/cloud/images/
[####################] - 3m    120000/120000  0s      found:5       errors:638    
[####################] - 3m     30000/30000   148/s   http://10.10.8.133/ 
[####################] - 3m     30000/30000   149/s   http://10.10.8.133/css 
[####################] - 3m     30000/30000   162/s   http://10.10.8.133/cloud 
[####################] - 3m     30000/30000   161/s   http://10.10.8.133/cloud/images 

```

> CLOUD

![](https://i.imgur.com/kDrFEq1.png)

We need to bypass the filter. We can try adding the nullbyte or `#`  to bypass this filter.

Let's make a shell.php on our attacker. and start a server.
![](https://i.imgur.com/9Dk8oXj.png)

![](https://i.imgur.com/CHdyQnP.png)

# PRIVESC [SYSADMIN]

In the /opt directorywe found something intresting.

![](https://i.imgur.com/qgwu6Za.png)

On attacker side `nc -lnvp 9898 > dataset.kbdx`

On victim side run
![](https://i.imgur.com/lrhSkSo.png)


Let's crack this file [Article](https://www.thedutchhacker.com/how-to-crack-a-keepass-database-file/)
> Cracking hash

`keepass2john dataset.kdbx > hash`

![](https://i.imgur.com/LgIgBJG.png)

Let's open this database.
![](https://i.imgur.com/q4wlGoi.png)

```
	sysadmin:Cl0udP4ss40p4city#8700
```

![](https://i.imgur.com/elNErg9.png)

# PRIVESC [ROOT]

Let's transfer pspy64 to review running processes.
![](https://i.imgur.com/w6NiIGZ.png)

```php
sysadmin@opacity:~$ cat  /home/sysadmin/scripts/script.php 
<?php

//Backup of scripts sysadmin folder
require_once('lib/backup.inc.php');
zipData('/home/sysadmin/scripts', '/var/backups/backup.zip');
echo 'Successful', PHP_EOL;

//Files scheduled removal
$dir = "/var/www/html/cloud/images";
if(file_exists($dir)){
    $di = new RecursiveDirectoryIterator($dir, FilesystemIterator::SKIP_DOTS);
    $ri = new RecursiveIteratorIterator($di, RecursiveIteratorIterator::CHILD_FIRST);
    foreach ( $ri as $file ) {
        $file->isDir() ?  rmdir($file) : unlink($file);
    }
}
?>

```

This is a script to tke the backup of the scripts reside in sysadmin.
We need to temper the `backup.inc.php` to trigger a revershell.
![](https://i.imgur.com/rc2VVHS.png)

Let's create the `backup.inc.php`.
![](https://i.imgur.com/M01OGU0.png)
![](https://i.imgur.com/khQBn4k.png)

