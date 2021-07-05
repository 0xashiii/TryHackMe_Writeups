A ctf for beginners, can you root me?


**Enumeration**

**NAMP**

`
ashik@whitedevil:~$ nmap -sC -sV 10.10.122.219  
Starting Nmap 7.80 ( https://nmap.org ) at 2021-06-15 12:58 IST  
Nmap scan report for 10.10.122.219  
Host is up (0.35s latency).  
Not shown: 998 closed ports  
PORT   STATE SERVICE VERSION  
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)  
| ssh-hostkey:    
|   2048 4a:b9:16:08:84:c2:54:48:ba:5c:fd:3f:22:5f:22:14 (RSA)  
|   256 a9:a6:86:e8:ec:96:c3:f0:03:cd:16:d5:49:73:d0:82 (ECDSA)  
|\_  256 22:f6:b5:a6:54:d9:78:7c:26:03:5a:95:f3:f9:df:cd (ED25519)  
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))  
| http-cookie-flags:    
|   /:    
|     PHPSESSID:    
|\_      httponly flag not set  
|\_http-server-header: Apache/2.4.29 (Ubuntu)  
|\_http-title: HackIT - Home  
Service Info: OS: Linux; CPE: cpe:/o:linux:linux\_kernel  
  
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .  
Nmap done: 1 IP address (1 host up) scanned in 50.81 seconds
`

