
# Gallery

## Description

Our gallery is not very well secured.



## Enumeration

We are provided with the IP of the machine.

###Port Scan

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