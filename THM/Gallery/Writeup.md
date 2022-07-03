
# Gallery


## Description

Our gallery is not very well secured.


## Enumeration

We are provided with the IP of the machine.


### Port Scan

Basic port scan shows that there are two open ports on the machine.

```
Not shown: 998 closed tcp ports (conn-refused)
PORT     STATE SERVICE VERSION
80/tcp   open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
8080/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
| http-open-proxy: Potentially OPEN proxy.
|_Methods supported:CONNECTION
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-title: Simple Image Gallery System
```

#### Port 80

Going over to the port 80 shows default Apache webpage.


#### Port 8080

Going over to the port 8080 redirects to /gallery/login.php

Searching the title *Simple Image Gallery System* on google shows that it is an open source image manager that works on **php** and **SQL**.


### Login

Since the webpage is running SQL, we can use the creds to login.

>**Username**: ' OR 1=1 -- -
>**Password**: ' OR 1=1 -- -

This gives us the access as *Admin*


## Getting Shell

Then we can upload a reverse shell in the **Album** and include it to get the shell.

Here I have used the PHP reverse shell from [PentestMonkey](https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php)

