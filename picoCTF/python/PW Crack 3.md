# PicoCTF - General Skills: PW Crack 3

## Description

This challenge provides a password checker script, an encrypted flag file, and a hash file. The objective is to determine the correct password and use it to decrypt the flag.

## Analysis

I first inspected the Python source code:

```bash
nano level3.py
```

The script reads a hash value from `level3.hash.bin` and compares it against the MD5 hash of the user-provided password.

Relevant code:

```python
correct_pw_hash = open('level3.hash.bin', 'rb').read()

def hash_pw(pw_str):
    pw_bytes = bytearray()
    pw_bytes.extend(pw_str.encode())
    m = hashlib.md5()
    m.update(pw_bytes)
    return m.digest()
```

At the bottom of the script, I found a list of possible passwords:

```python
pos_pw_list = ["6997", "3ac8", "f0ac", "4b17", "ec27", "4e66", "865e"]
```

The challenge description stated that only one of these passwords was correct.

## Examining the Hash

The contents of `level3.hash.bin` were:

```text
1B 18 E1 31 6F 92 18 CC 5B 05 3E 1C EA 28 E0 2E
```

This is a 16-byte value, which matches the length of an MD5 digest.

## Finding the Correct Password

I calculated the MD5 hash of each candidate password:

```python
import hashlib

candidates = ["6997", "3ac8", "f0ac", "4b17", "ec27", "4e66", "865e"]

for pw in candidates:
    print(pw, hashlib.md5(pw.encode()).hexdigest())
```

Output:

```text
6997 c9049d2a46feb0ae2de6b0636f32ea0d
3ac8 9648ae234bd327d686f9d684342070cd
f0ac ee7751a05dead01a36378729fd0c204e
4b17 03078179ea3e83075620b5ed18f95894
ec27 f41719388902e51c111a5778b4669fcb
4e66 cd6925a9d53245b106a97e303f5e2ef7
865e 1b18e1316f9218cc5b053e1cea28e02e
```

The hash of `865e` matched the value stored in `level3.hash.bin`.

Therefore, the correct password was:

```text
865e
```

## Exploitation

I ran the password checker:

```bash
python3 level3.py
```

When prompted, I entered:

```text
865e
```

The program accepted the password and decrypted the encrypted flag file.

## Flag

```text
picoCTF{m45h_fl1ng1ng_2b072a90}
```
