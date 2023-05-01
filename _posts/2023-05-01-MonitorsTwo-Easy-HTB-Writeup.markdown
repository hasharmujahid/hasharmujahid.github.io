---
title: "MOnitors Two HTB EASY"
layout: page
date: 2023-05-01
image: https://miro.medium.com/max/786/0*Wr3Tm8LvmG57ES_2.webp
headerImage: true
tag:
- MonitorsTwo Writeup
- MonitorsTwo Walkthrough
- MonitorsTwo HTB Writeup
- MonitorsTwo HTB
- htb monitorstwo
- htb 
- hasharmujahid
star: true
category: blog
author: Hashar Mujahid
description: Hi This Blog is the writeup of MonitorsTwo Machine from Hack The Box.
---

![](https://i.imgur.com/99iJZVs.png)


# NETWORK ENUMERATION

## PORT SCAN

```

 ╭─hax13@ZARB in ~/Documents/ctfs/htb/easy/monitor2 took 13ms
 ╰─λ sudo nmap -sC -sV  -p- --min-rate 1500 10.10.11.211 -oN tcp-scan.txt 
[sudo] password for hax13:       
Starting Nmap 7.93 ( https://nmap.org ) at 2023-04-30 12:58 PKT
Nmap scan report for 10.10.11.211
Host is up (0.44s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 48add5b83a9fbcbef7e8201ef6bfdeae (RSA)
|   256 b7896c0b20ed49b2c1867c2992741c1f (ECDSA)
|_  256 18cd9d08a621a8b8b6f79f8d405154fb (ED25519)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
|_http-title: Login to Cacti
|_http-server-header: nginx/1.18.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 77.52 seconds

```


# WEB ENUMERATION

> Landing Page

![](https://i.imgur.com/R0iQWDY.png)

Let's search google:


![](https://i.imgur.com/fTyBLnc.png)

Now let's search for version exploits.

![](https://i.imgur.com/huPCD6M.png)

[LINK](https://www.rapid7.com/db/modules/exploit/linux/http/cacti_unauthenticated_cmd_injection/)

![](https://i.imgur.com/5Zo73ZE.png)


Found the amazing blog from [sonar](https://www.sonarsource.com/blog/cacti-unauthenticated-remote-code-execution/) That explains how to exploit this vulnerability. 

Found another POC:
[GITHUB REPO](https://github.com/sAsPeCt488/CVE-2022-46169)


![](https://i.imgur.com/npekisf.png)



So from the blog we know "The `poller` table contains a default entry with the hostname of the server running Cacti. Because of this, attackers can bypass the `remote_client_authorized` check by, e.g., providing the HTTP header `X-Forwarded: <TARGET-IP>`. This way, the function `get_client_addr` returns the IP address of the server running Cacti. The call to `gethostbyaddr` resolves this IP address to the hostname of the server, which will pass the poller hostname check because of the default entry."

# FOOTHOLD [www-data]:

So we have to change the XFORWARDED IP so it wll be present in the poller table.

![](https://i.imgur.com/CNyuLM3.png)


Now let's start the listner and run the exploit.

![](https://i.imgur.com/1xEHiRN.png)

On the listner side:

![](https://i.imgur.com/vilVMTW.png)


We are in a container if we run hostname can see our container id.
![](https://i.imgur.com/Tn1iAXe.png)

> Paswords in confg

![](https://i.imgur.com/4BqaXOP.png)

# PRIVESC [ Docker-www-data to Docker-Root]:

After running linpeas found that:

![](https://i.imgur.com/l9tDfJd.png)

Let's run   
`capsh --gid=0 --uid=0 --`
to become root inside the container.

![](https://i.imgur.com/9uTE8jI.png)



# PRIVESC [Docker Root - Marcus]:

We also saw that mysql is running and we have password for the databases so let's try to connect to the database.
![](https://i.imgur.com/0RDijEy.png)

We also had a entrypoint.sh which can reveal more information:

```bash
#!/bin/bash
set -ex

wait-for-it db:3306 -t 300 -- echo "database is connected"
if [[ ! $(mysql --host=db --user=root --password=root cacti -e "show tables") =~ "automation_devices" ]]; then
    mysql --host=db --user=root --password=root cacti < /var/www/html/cacti.sql
    mysql --host=db --user=root --password=root cacti -e "UPDATE user_auth SET must_change_password='' WHERE username = 'admin'"
    mysql --host=db --user=root --password=root cacti -e "SET GLOBAL time_zone = 'UTC'"
fi

chown www-data:www-data -R /var/www/html
# first arg is `-f` or `--some-option`
if [ "${1#-}" != "$1" ]; then
        set -- apache2-foreground "$@"
fi

exec "$@"
```


> Databases

![](https://i.imgur.com/rWH5NLk.png)


> Tables

![](https://i.imgur.com/fHlRKNC.png)


Let's dump the user_auth tabel:

> User_auth

![](https://i.imgur.com/1HRxDm6.png)

We got some hashes:
`marcus:$2y$10$vcrYth5YcCLlZaPDj6PwqOYTw68W1.3WeKlBn70JonsdW/MhFYK4C`
`admin:$2y$10$IhEA.Og8vrvwueM7VEDkUes3pwc3zaBbQ/iuqMft/llx8utpR1hjC`

Let's try to crack them.
The hash looks like bcrypt so let's crackem.
![](https://i.imgur.com/lC24p4q.png)

After some time we got the password `$2y$10$vcrYth5YcCLlZaPDj6PwqOYTw68W1.3WeKlBn70JonsdW/MhFYK4C:funkymonkey`

Let's login using ssh.

![](https://i.imgur.com/m0y1eE2.png)


# PRIVESC [Macus to Root]:

Let's run the linpeas and see if we find anything.

![](https://i.imgur.com/lDtTjJ3.png)

![](https://i.imgur.com/4jPUx4p.png)



So according to this we can run the programs of the container inside the host machines b directly finding the path of the program.

The path can be found using findmnt command.
![](https://i.imgur.com/5vD6gXN.png)


Let's create a suid bit bash in our container.
![](https://i.imgur.com/fN2tJn8.png)

Now let's run it by path.
![](https://i.imgur.com/oARZl7N.png)

Let's run it.
![](https://i.imgur.com/yyK2SpN.png)

We got the root.

![](https://media.giphy.com/media/ieBWQkIVEELhbizGAp/giphy.gif)

---
