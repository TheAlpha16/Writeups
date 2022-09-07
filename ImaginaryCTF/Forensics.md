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


