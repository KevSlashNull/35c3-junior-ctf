# Logged In
## Challenge Details
> **Phew, we totally did not set up our mail server yet. This is bad news since nobody can get into their accounts at the moment... It'll be in our next sprint. Until then, since you cannot login: enjoy our totally finished software without account.**
> 
> http://35.207.132.47/
> 
> Difficulty Estimate: Easy
> 
> ---
> Good coders should learn one new language every year.
>
> InfoSec folks are even used to learn one new language for every new problem they face (YMMV).
> 
> If you have not picked up a new challenge in 2018, you're in for a treat.
> 
> We took the new and upcoming Wee programming language from paperbots.io. Big shout-out to Mario Zechner (@badlogicgames) at this point.
> 
> Some cool Projects can be created in Wee, like: this, this and that.
> 
> Since we already know Java, though, we ported the server (Server.java and Paperbots.java) to Python (WIP) and constantly add awesome functionality. Get the new open-sourced server at /pyserver/server.py.
> 
> Anything unrelated to the new server is left unchanged from commit dd059961cbc2b551f81afce6a6177fcf61133292 at badlogics paperbot github (mirrored up to this commit here).
> 
> We even added new features to this better server, like server-side Wee evaluation!
> 
> To make server-side Wee the language of the future, we already implemented awesome runtime functions. To make sure our VM is 100% safe and secure, there are also assertion functions in server-side Wee that you don't have to be concerned about.

## Breakdown

On the website logging in and signing in is not possible by default. The site will ask you for a verification code sent by email. But going through the server.py ([source](https://github.com/KevSlashNull/35c3-junior-ctf/blob/master/_paperbots/server.py)), there is a function called `login()` that processes POST requests to `/api/login`. It returns `get_code(name)` which generates the verification code for the user. And as it returns it, it will be sent to the browser. Using the browser‘s webtools (or a software like Postman or Burp), we can obtain the verification code and successfully log in.

## Solution

When logged in (with `admin` as username preferably), the `logged_in` cookie contains the flag.

```
35C3_LOG_ME_IN_LIKE_ONE_OF_YOUR_FRENCH_GIRLS
```

## Comment

It did not took me long to find a way to be able to log in. “Phew, we totally did not set up our mail server yet” helped me to spot the “bug”. Because I solved the challenge in the browser, I was wondering why I was not given the flag once I logged in. I really tend to overlook simple things.
