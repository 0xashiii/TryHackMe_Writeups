A room more focused on the misconfiguration of web servers, rather than sheer exploits


### TASK 1 : Intro


The purpose of this room, is to show the more subtle vulnerabilities. These vulns won't get you RCE, or LFI, but they will allow you to access sensitive information that a client would want to keep protected.

The topics that will be covered in this room are:

\- IDOR

\- Forced Browsing

\- API based Authentication Bypass

  

You can do this room with minimal experience, but I will be using some pentesting terms that if you don't know, you should research.


### TASK 2 : Section 1 - IDOR : Intro

IDOR, or Insecure Direct Object Reference, is the act of exploiting a misconfiguration in the way user input is handled, to access resources you wouldn't ordinarily be able to access.

For example, let's say we're logging into our bank account, and after correctly authenticating ourselves, we get taken to a URL like this [https://example.com/bank?account\_number=1234](https://example.com/bank?account_number=1234). On that page we can see all our important bank details, and a user would do whatever they needed to do and move along their way thinking nothing is wrong.

There is however a potentially huge problem here, a hacker may be able to change the account\_number parameter to something else like 1235, and if the site is incorrectly configured, then he would have access to someone else's bank information.


### TASK 3 : Section 1 - IDOR : Exploitation

We start off with an example page.  

![](https://imgur.com/q2kqkfT.png)

It makes enough sense, if you authenticate with the right user, you get to access that user's note.  


![](https://imgur.com/peFWimx.png)

If you have the wrong password, you get an incorrect password message. For our purposes the user is noot, and the password is test1234. Authenticating correctly, as expected takes us to our note.


![](https://imgur.com/2jSUDQ6.png)

However, as you may have picked up on, there seems to be an interesting part of the URL. It seems that the note that we can view is controlled by a URL parameter, let's check if we can access other notes, by increasing the number to 2.

![](https://imgur.com/hayJHKV.png)                                            

Woohoo! We can access other's notes. While this may seem dramatic, exploiting this is the real world can have drastic consequences. Let's say you found an IDOR vulnerability in a note keeping site, which allowed you to access the notes of others, you could find plenty of personal details, like passwords, usernames, even credit card information.

  

There is no way to automatically exploit this, as the pentester you need to examine the site, and find misconfigurations.


### TASk 4 : Section 1 - IDOR : Challenge

The credentials are:

noot:test1234

Try and exploit IDOR to get the flag!

Port: 80


1. What is the flag



### TASK 5 : Section 2 - Forced Browsing : Intro

Forced browsing is the art of using logic to find resources on the website that you would not normally be able to access. For example let's say we have a note taking site, that is structured like this. [http://example.com/user1/note.txt](http://example.com/user1/note.txt). It stands to reason that if we did [http://example.com/user2/note.txt](http://example.com/user2/note.txt) we may be able to access user2's note. 

Taking this a step further, if we ran wfuzz on that url, we could enumerate users we don't know about, as well as get their notes. This is quite devastating, because we can then run further attacks on the users we find, for example bruteforcing each user we find, to see if they have weak passwords.



### TASK 6 : Section 2 - Forced Browsing : Manual Exploitation

We start off with our basic note taking application.

![](https://imgur.com/zQPqKS8.png)

For our purposes the user and password are noot, and test1234. Properly authenticating shows us our note.

  

![](https://imgur.com/tSSJdfi.png)  

Let's look over what we have. The user we authenticated as is noot, and the path for the note is noot/note.txt. Therefore, it seems notes are stored in `<username>/note.txt` , meaning that if we find other usernames, we may be able to read other's notes! Often times on any application there is an admin user, so let's see if we can view his note.

  
![](https://imgur.com/COgxTaK.png)

Woohoo!

Forced browsing will often require some logic on the part of the hacker. To properly exploit this, you should keep notes on everything you find, building a picture of how the web application functions is essential.


### TASK 7 : Section 2 - Forced Browsing : Automatic Exploitation

Than sort of be exploited automatically. A tool such as wfuzz or dirsearch can find resources that normal users wouldn't be able to find. wfuzz will be the better tool in most cases, as it allows you better control over the path, so we'll go over basic wfuzz usage, and use it to exploit the our example site. wfuzz can be installed using `pip3 install wfuzz`.

  

  

wfuzz flags:  


\-c    			Shows the output in color  

\-z             Specifies what will replace FUZZ in the request. For example -z file,big.txt will read through all the lines of big.txt and replace FUZZ with  

\--hc          Don't show certain http response codes  

\--hl           Don't show a certain amount of lines in the response  

\--hh  		   Don't show a certain amount of words  

  

Let's make a request  

  

![](https://imgur.com/K5ykhIx.png)

We can see that when there isn't a note.txt it returns a 404, with 57 words. Let's hide 57 words by setting --hw to 57.

![](https://imgur.com/eYRJeyt.png)

Boom, we found the admin note!



1. What flag hides characters

2. What flag shows specific word amounts instead of hides them


### TASk 8 : Section 2 - Forced Browsing : Challenge

Use What you've learned to find the flag!

Port: 81

1. What is the flag


### TASK 9 : Section 3 - API bypassing : Intro

This is a bit of a unique one, as it can basically be anything. APIs are by definition incredibly versatile, and finding out how to exploit them, will require a lot of research and effort by the hacker. The following situation is only one possible scenario out of a near infinite number.


### TASk 10 : Section 3 - API bypassing : Exploitation

We start off with a basic login.

![](https://imgur.com/XR9Lz4N.png)

Logging in gives us an admin panel.

![](https://imgur.com/xQyefIs.png)

It seems we can run system commands here, so let's try running id.


![](https://imgur.com/Us7JEx1.png)

Woohoo! It seems we can run commands. But let's think for a second, we didn't really need to login at all, if we found the api.php page through dirsearching, and a cmd parameter through fuzz, we would never have needed to use the login panel.

If you find an api.php file, it's important to check it out on its own. It's probable that it won't be as easy as wfuzz to get command execution, but we may still be able to find useful information.


There is no way to automatically exploit this, we may be able to use tools we already know to get additional clues, but it will always take manual research


### TASk 11 : Section 3 - API bypassing : Challenge

You don't know any user, try and read the flag.txt

Port: 82

(Note: the admin.php page is useless, and will only mislead you. Remember the goal is to skip it :). Remember what I'm asking for, and remember what tools you can use and you will find it!)


1. What is the flag


