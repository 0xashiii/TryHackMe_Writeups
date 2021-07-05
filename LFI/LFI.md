**Understand and exploit a web server that is vulnerable to the Local File Inclusion (LFI) vulnerability**





### ***TASK1***

Local File Inclusion (LFI) is the vulnerability that is mostly found in web servers. This vulnerability is exploited when a user input contains a certain path to the file which might be present on the server and will be included in the output. This kind of vulnerability can be used to read files containing sensitive and confidential data from the vulnerable system.  
  
The main cause of this type of Vulnerability is improper sanitization of the user's input. Sanitization here means that whatever user input should be checked and it should be made sure that only the expected values are passed and nothing suspicious is given in input. It is a type of Vulnerability commonly found in PHP based websites but isn't restricted to them.

Deploy the VM and access its web server: [http://10.10.166.77](http://10.10.166.77/)



### ***TASK 2: Getting User Access via LFI***

To test for LFI what we need is a parameter on any URL or any other input fields like request body etc. For example, if the website is tryhackme.com then a parameter in the URL can look like `https://tryhackme.com/?file=robots.txt`. Here `file` is the name of the parameter and `robots.txt` is the value that we are passing (_include the file robots.txt)_.

**Importance of Arbitrary file reading**

A lot of the time LFI can lead to accessing (without the proper permissions) important and classified data. An attacker can use LFI to read files from your system which can give away sensitive information such as passwords/SSH keys; enumerated data can be further used to compromise the system.

In this task, we are going to find the parameter which is vulnerable to the Local File Inclusion attack. We will then will try to leverage information obtained to get access to the system.


 **Look around the website. What is the name of the parameter you found on the website?**

*HINT: Try clicking around the website. What parameter is including files?*

*ANSWER:* `page`


Once we find the vulnerable parameter we can try to include the passwd file on the Linux system i.e /etc/passwd. The most common technique is path traversal method meaning we can include files like **../../../../etc/passwd** what this does it get out of a directory like we usually do in Linux system by running **cd ../**

**../../etc/passwd** means to go out twice from the current working directory and then go to **/etc** directory and read the **passwd** file. Now the issue with this method is you need to be sure about the path of the file.

You can read the [interesting files to check out](https://github.com/cyberheartmi9/PayloadsAllTheThings/tree/master/File%20Inclusion%20-%20Path%20Traversal#basic-lfi-null-byte-double-encoding-and-other-tricks) while testing for LFI.

**No Answer Needed**


Once you include **/etc/passwd** then you should see entries something like:

root:x:0:0::/root:/bin/bash  
bin:x:1:1::/:/sbin/nologin  
daemon:x:2:2::/:/sbin/nologin

To understand all those entries read [[Understanding etcpasswd-file-formaat]]
[this](https://www.cyberciti.biz/faq/understanding-etcpasswd-file-format/) article.

This file can give information about the system like the name of all the existing users on the system.


**What is the name of the user on the system?**
*HINT:When including the file, it will appear at the bottom of the page.*
*ANSWER:falcon*

Once you find the name of the user it's important to see if you can include anything **common** and **important** in that user's directory, could be anything like theirs **.bashrc** etc

**Name of the file which can give you access to falcon's account on the system?**

*HINT:Try to read private key file in \`.ssh\` folder under falcon's directory.*
*ANSWER: id_rsa*

*ID_RSA*
`
      -----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEAy5ab5+v0aBYFL7dN4O69CqvZdjpSk4/BFMOEndp71fy4qlBi
ewnTYIyKS1DjNhLdbLJZV/8oKOIx/BVSxrw3hnjL/b5d3jUpX1edcX8hjTt10LCT
ubgjWekIpspImHtp/vf9n0IlL1GIWLsxKZXEqyoSvvG4LU6ajrMyHM9jyZ7yMAsS
lHtfNzX22taxJliHF3SFDjf6Z0otY98T273qy6uTchBDEIZx7uJGQF4h9bvSq+22
e7QFY/h/cLrVUPGcRgv2VH4958A5n+BA48ioVyjGRcJlPsa8fNuFkgA8VlJqx/zD
Erf3G97zCY5iZSSm6g+9aUBAoqJqQqnZ1JY/jQIDAQABAoIBAQCjN3qEY7GM5OKB
j6Z7B0s9S+rKkxVywdQczmb6mpefRb3SpSFe3NC+3c1ddlrCFju4kf94wdIzfKxw
GbREKc8mGqAILN9abypdCoPp4u9GJ/5bMcUtJogI4/+QoCm1PXQL+ks1q7TeC7KQ
2HogibajNtbSiD2M7TCR6O3rFQU+NKW3P8R5rTJBFxSqXcoL0aZoOdIISuGo4e2D
w5p8eLHiYmKmQ3KYW58fau1Qnr8t0YoHDlG2wezNBQSckt5xxjY4XWlfcMPAQIP9
VWxv77zIqfLFYj6oH0opPHX7SbdpFVj00h7Ee2C0QR7CKj4lxmE2Dn97LEXksfWM
PqmJwiRBAoGBAPRGPZtAT7TfTac7lXbOYNj/5HuK1Pc7+GtsT8V3AJwbhkeZG/B+
ERee3gKlXhglEup0duxBwy5zTy6vsegMo7sk2ii/GlboR44Yy7OYbjrmIw7JRx6i
IIjN6YeKM+dwLGL7+xG2EavrRBJbuYfp1R139gK5CWd6Yu6xdwtCPS8xAoGBANVc
ZhSQOWzkN0Yq4CsAnKrrtIeynX7wTppy2F3N8/CQHQIYjFx0SZxs/1MlTXEpFdC+
VHhQ+Cy5O+hndqXiG5sty9wC67lZNaxuBkMoxKgX42JAgxxE41lUadAdRHJ4cq7A
1DiJ1xq3XwP1ZaUEVE/Y3EusMhtr/A6z1dGJcpcdAoGAdb6d14XqZb71iVS5OOlF
2ZOPKNXEzd+EYRN2aDJygszprv1ocEX0KzSSwye+8Vh9g7Hb2Qnh8TP3yQM7eCUP
jxe2aMmlAps4UpA1MD6bc5yW7Xur4mI32HmYxZKibj6txpC7dtASOJJQ36CDD7Zw
2aGHXcyfcdeWdIPqY+zr3SECgYEAoi4SCh93ByaSPWvp6cYVUHbKSzuiLBNOLGiP
vv4GJx3kbutqBfz+10Ci8/iu3Q11365NVwd1HcnPl+DNd1pf0Z0GEL7Hn6QIAIHB
kNs0YPGHje+ruZlDl2tq4x7cIIcd5Wf96Nwd/djVCJVIJh8cV3VoPr0teVqjxik8
poHr8KECgYBGJ/FLmaloEl00YECXAy2uMlvGyaAr2tsq28JF3LvZOXNIhB71+11V
E/gv9PP0ruJJRBv5r2DjAcsWA+sIBf5O8+z5dTeuto5v+WTrpOy8i3FLEWl3hqRH
ANCTI4EhCynYtF8zi8so5zeomlncFZg7JD/dL0gHXu4FImx5sUO9gg==
-----END RSA PRIVATE KEY-----
`

save this id_rsa to a file and change the permission to 400 then run the command
`ssh -i id\_rsa falcon@MACHINE_IP`





**What is the user flag?**
*HINT:Make sure to change the permission of the file.*
*ANSWER:*


![](Pastedimage20210605104939.png)


### ***TASK 3: Escalating your privileges to root***

In this, we are going to focus on getting root-level access on the machine. This step is also known as Privilege escalation; we are going to escalate our privilege from a normal user to a root user (with the highest level of system privileges).

First, we'll have to find a vector that would be exploited to give us root access. A vector can be anything like a binary with some special permission or a cronjob that is not configured properly etc.

I've written a blog post about Linux privilege escalation, you can read it [here](https://mzfr.github.io/linux-priv-esc) to know more about it.

**What can falcon run as root?**
*HINT:sudo -l*
*ANSWER:*
![](Pastedimage20210605105424.png)


Search gtfobins via the [website](http://gtfobins.github.io/) or by using [gtfo tool](http://github.com/mzfr/gtfo), to see if you find any way to use that binary for privilege escalation.

**What is the root flag?**
*ANSWER*
![](Pastedimage20210605105916.png)

