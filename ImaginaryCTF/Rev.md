# xorrot

## Description

I'm rather tired, but I whipped up this xor scheme. See if you can crack it?

## Attachments

server.py

```
#!/usr/bin/env python3

flag = open('flag.txt', 'rb').read()
key = open('/dev/urandom','rb').read(1)[0]
out = []

for c in flag:
    out.append(c^key)
    key = c

print(f'{bytes(out).hex() = }')
```

output

`970a17121d121d2b28181a19083b2f021d0d03030e1526370d091c2f360f392b1c0d3a340e1c263e070003061711013b32021d173a2b1c090f31351f06072b2b1c0d3a390f1b01072b3c0b09132d33030311`

## Solution

The implementation uses 8-bit key which is so small that we can bruteforce the flag.

script

```
hex_out = '970a17121d121d2b2...'
ecn = bytes.fromhex(hex_out)


def dec(key):
    out = ''
    for c in ecn:
        s = c ^ key
        out += chr(s)
        key = s
    if 'ictf{' in out:
        print(out)
        print(key)


for i in range(256):
    dec(i)
```

## flag 

`ictf{it_would_probably_help_if_the_key_affected_more_than_just_the_first_char_lol}`

<br>

# Reflection

## Description

Reflection is both the question and the answer.

## Attachments

`https://imaginaryctf.org/f/n639O#reflection`

## Solution

Analyzing the given binary in ghidra shows that our input is being xored by the first 65 non zero bytes and being compared to the corresponding byte in flag list.

script

```
with open('reflection', 'rb') as f:
    data = f.read()

check_lis = [0x16,
    0x26, 0x38, 0x20, 0x79, 0x65, 0x68, 0x67, 0x61, 0x78, 0x0f,
    0x65, 0x1f, 0x9b, 0x54, 0x2f, 0x4f, 0x52, 0x34, 0x77, 0x7f,
    0x72, 0x5b, 0x26, 0x35, 0x2e, 0xbb, 0x76, 0xb1, 0x6d, 0x66,
    0x5c, 0x6a, 0x79, 0x6e, 0x7d, 0x70, 0x47, 0x62, 0x6e, 0x79,
    0x5e, 0x6b, 0x71, 0x03, 0x72, 0x2f, 0x76, 0x7f, 0x68, 0x6b,
    0x64, 0x75, 0x62, 0xee, 0x5d, 0xe9, 0x6d, 0x4f, 0x62, 0x6b,
    0x44, 0x45, 0x1f, 0x25
]

final = data.replace(b'\x00', b'')

for i in range(len(check_lis)):
    print(chr(check_lis[i] ^ final[i]), end="")
```

## flag

`ictf{did_you_know_that_function_names_are_just_pointers_to_code?}`

<br>

# I like to MOV it, MOV it!

## Description

We'll get there soon ... byte-by-byte.


## Attachments

`https://imaginaryctf.org/f/ACXRp`

## Solution

Analysing the given file in Ghidra shows the flag..🙃.

But the intended solution for this is to use [Side Channel Attack](https://en.wikipedia.org/wiki/Side-channel_attack)

## flag

`ictf{mov_S1d3_ChanN3l_Att4ck}`

<br>

