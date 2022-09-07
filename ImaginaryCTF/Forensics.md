# Lost Flag

## Description

Quasar said he lost his flag but it seems to me like it's in this file

## Attachments

`https://imaginaryctf.org/f/fOweJ#flag.zip`

## Solution

The given zip file contains a `.DS_Store` which is a file that stores custom attributes of its containing folder, such as folder view options, icon positions, and other visual information created by finder in MACOSX.

The filename starts with . to hide the file in most unix utilities.

Simply opening the file shows you the flag.

## flag

`ictf{mac_is_better_than_templeos}`


# Geoguessr Sucks

## Description

I went on a crazy vacation to the middle of nowhere, and I took a real picture with real camera of this very important field. Where is it?

Flag format is ictf{lat_long}, both rounded up to 5 decimal places because this exact spot is very important to me. Example: ictf{1.23456_7.89101}

## Attachments

`https://imaginaryctf.org/f/1OD91`

## Solution 

The coordinates are literally given in the meatdata of the image provided.

I have used this [tool](https://www.latlong.net/degrees-minutes-seconds-to-decimal-degrees) to convert DMS coordinastes to decimal format.

## flag

`ictf{39.13370_-98.52834}`

<br>

# Shine

## Description

I've not slept enough recently.

## Attachments

`https://imaginaryctf.org/f/JsBk7#flag.txt`

## Solution

Each line is 64 characters long. The hex digit of index of the uppercase letter in each block of 16 bytes adds to the flag.

script

```
ct = open('flag.txt', 'rb').read().replace(b'\n', b'')

# print(len(ct))
final = []
s = ''

for i in range(len(ct)):
    if i % 16 == 0:
        final.append(s)
        s = chr(ct[i])
    else:
        s += chr(ct[i])
final.append(str(s))

hex_enc = ''

for line in final:
    for i, char in enumerate(line):
        if char.isupper():
            hex_enc += hex(i)[-1]
        elif char == '\x00':
            hex_enc += hex(i)[-1]
        elif char == '\x0e':
            hex_enc += hex(i)[-1]

print(bytes.fromhex(hex_enc))
```

## flag

`ictf{writing_challenges_is_sort_of_like_writing_a_novel_i_guess}`

<br>

# Shady Penguins

## Description
Roo has spotted some shady Penguins passing files to each other and finally got his paws on a pair. What are these Penguins up to?

## Attachments
`https://imaginaryctf.org/f/jJH2d#Spy`
`https://imaginaryctf.org/f/wooKe#Penguins`

## Solution

Checking the file headers shows that these two files are PNG images.
After checking different bit manipulations, finally doing AND on each pixel produces the imagae containing flag.

script

```
from PIL import Image
import numpy as np

penguin = Image.open('Penguins.png')
spy = Image.open('Spy.png')

pen_arr = np.array(penguin)
spy_arr = np.array(spy)

# pen_arr |= spy_arr
# pen_arr ^= spy_arr
pen_arr &= spy_arr

Image.fromarray(pen_arr).show()
```

## flag

`ictf{visual_crypto_is_neato}`

<br>



# Zippy

## Description

Yet another hash cracking challenge ... or is there another way to go faster than the competition?

## Attachments

`https://imaginaryctf.org/f/QmDPe#chall.zip`

gen.py

```
#!/usr/bin/env python3

from base64 import b64encode
from flag import flag
from string import ascii_letters, digits
from hashlib import sha1
from random import choice
from os import system

password = ''.join(choice(ascii_letters+digits) for i in range(80))
fname = b64encode(sha1(password.encode()).digest()).decode()

with open(fname, 'w') as f:
    f.write(flag)

print(password)
print(fname)

system(f'7z a chall.zip {fname} -mem=AES256 -p{password}')
```

## Solution

Since the password is long, it is hashed before using as a password.

Now we can see the password hash in the zip file.

```
jithendranadh@Jithendras-MacBook-Air Downloads % strings chall.zip
ng3pV1YIws4l91Ai04m3IMVa2kg=
Rp5tJ
ng3pV1YIws4l91Ai04m3IMVa2kg=
```

using this hash as password will give us the flag

script

```
import pyzipper
import base64

passwd = base64.b64decode('ng3pV1YIws4l91Ai04m3IMVa2kg='.encode())

with pyzipper.AESZipFile('chall.zip', 'r', compression=pyzipper.ZIP_DEFLATED, encryption=pyzipper.WZ_AES) as extracted_zip:
    extracted_zip.extractall(pwd=passwd)
```

## flag

`ictf{fastest_hash_cracking_gun_in_the_w3st}`

<br>

# mixup

## Description
Something went wrong here...

## Attachments

`https://imaginaryctf.org/f/CJVKB#flag.txt`

flag.txt

```
i‎c‮t‎f‮{‎‎un1c0‮de_m‮4g1c_na‎‮h‎s‮d‮fo‮‎asi‎h‮‮df‎asoh‮‎dfoi‎ashdf‎‮‎j‎‮k‎a‎‮‎d‎s‮‮hf‮‎lj‮ad‮‎‮s‮f‎hdskl‮a‎‮h‎fl‮‮‎k‮hj‎‎‮d‎‎‮‮‮a‎‎fs}
```

As we can see