---
title: "EXPLOITING OS COMMAND INJECTION VULNERABILITIES"
layout: page
date: 2022-10-09
image: https://miro.medium.com/max/786/0*Wr3Tm8LvmG57ES_2.webp
headerImage: false
tag:
- markdown
- elements
star: true
category: blog
author: Hashar Mujahid
description: Hi This Blog Explains What Os Command Injections are.
---

Hi! My name is Hashar Mujahid. And today we are going to learn what **OS command injections** are and how we can exploit them.

![](https://miro.medium.com/max/770/0*fsDcLw9CU5hxtTs-.png)


## What is OS Command Injection?

An OS command injection is a vulnerability that allows an attacker to execute arbitrary commands directly on the server. If you haven't already realized, if an attacker is able to execute malicious code on the server, he could easily get a reverse shell or a backdoor into the server.

So finding Os command injection during bug-bounty and penetration is marked as a critical vulnerability.

## Example:

Like in this simple lab, instead of only whitelisted commands server allows the user to execute all commands.

![](https://miro.medium.com/max/770/1*u4Pktd7Fg-F5ll_3Ibenbg.png)

## Use Full Commands:

An attacker can run some commands to get related information to the user and the target’s operating system.

> Name of the current user`whoami`
> 
> Operating system`uname -a`
> 
> Network configuration`ifconfig`
> 
> Network connections`netstat -an`
> 
> Running processes`ps -ef`

## OS Command Injections In Post Request Parameters:

We can see in this lab that the post request is made when we enter the value.

![](https://miro.medium.com/max/770/1*uErjgnrjaDtK1mV91RIlVQ.png)

We can determine that because in the GET request the parameter is listed in the URL and in the POST request the parameter is not listed in the URL.

```bash
GET

http://124.221.152.247:5001/CMD-1/index.php?

POST

http://124.221.152.247:5001/CMD-1/index.php?cmd=ls
```

Now the question is

## How To Detect Command Injection Vulnerability:

There is no specific answer to this question. try to understand the logic of the application if there is suspected suspicion that some functionality is using vulnerable functions like

> eval()
> 
> system()
> 
> passthru()

You can try to inject commands and test those parameters.

## Breaking Out Of Existing Commands:

Sometimes, if the code execution is very important in the exposed functionality, applications set a pre-made path of the command and get the query from the user.

![](https://miro.medium.com/max/770/1*O7TbSjXwfhuwfzVeaIw4eQ.png)

Whois

![](https://miro.medium.com/max/770/1*4jE0X4KfKzhWnSH-LS1eEA.png)

Google Whois

Now to get command execution, we need to use some bypassing techniques so we can execute code.

![](https://miro.medium.com/max/770/1*x0f1ZKXzzNSrswIewKvxKw.png)

We can use `;` symbol to end the execution of one command and add ours.

![](https://miro.medium.com/max/770/1*mYDK_NPaaLdRbIp3MhNOYg.png)

We can see our code executed.

## Blind OS command Injection Vulnerabilities:

Many cases of OS command injection are blind vulnerabilities. This indicates that the application does not include the command’s output in its HTTP response. Blind vulnerabilities can still be exploited, but it takes a different approach.

## Detecting Blind OS Injection Vulnerabilities:

Now you should be asking yourself how can we detect the blind OS command injection vulnerabilities.

The simple answer is to inject the commands that can cause some delays.

## for example :

You can try injecting the sleep command like this

`vulnerableweb.com/command?cmd=;sleep(5)`

This will cause the server to respond after 5 seconds.

Or you can try to inject a ping command to interact with the public IP to see if you got the connection back from the web app.

`vulnerableweb.com/command?cmd=;& ping -c 10 IP`

![](https://miro.medium.com/max/770/1*xYLt9GNNASe8ameMzEed1g.png)

We can see our 10 pings caused approx 10 sec of delay on the server.

## Payload:

`email=test@mail.com||ping+-c+10+127.0.0.1||`

## Ways of injecting OS commands:

Sometimes applications block the ";" symbol to prevent attackers from injecting commands.

We can try to inject.

-   `&`
-   `&&`
-   `|`
-   `||`

## How to prevent OS command injection attacks:

Try not to call out OS commands, but if it is inevitable developers can try some techniques.

-   Validating against a whitelist of permitted values.
-   Validating that the input is a number.
-   Validating that the input contains only alphanumeric characters, no other syntax or whitespace.

I will be back with new techniques to exploit web vulnerabilities.

Till then, Happy Hacking ❤.

![](https://media.giphy.com/media/FnGJfc18tDDHy/giphy.gif)