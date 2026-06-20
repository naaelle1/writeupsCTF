# PicoCTF - General Skills: PW Crack 4

## Description

This challenge provides a Python password checker, an encrypted flag file, and a hash file. Unlike the previous challenge, there are 100 possible passwords and only one of them is correct.

## Analysis

I began by inspecting the source code:

```bash
cat level4.py
```

The script loads the encrypted flag and a hash file:

```python
flag_enc = open('level4.flag.txt.enc', 'rb').read()
correct_pw_hash = open('level4.hash.bin', 'rb').read()
```

The password verification function uses MD5:

```python
def hash_pw(pw_str):
    pw_bytes = bytearray()
    pw_bytes.extend(pw_str.encode())
    m = hashlib.md5()
    m.update(pw_bytes)
    return m.digest()
```

At the bottom of the script, I found a list of 100 candidate passwords:

```python
pos_pw_list = [...]
```

Since the challenge states that only one password is correct, the goal is to find which candidate produces the same MD5 hash as the value stored in `level4.hash.bin`.

## Examining the Hash

Using `bvi`, I viewed the contents of the hash file:

```bash
bvi level4.hash.bin
```

The file contained:

```text
D3 D5 8C 47 86 A6 A2 29 42 73 51 50 0A C7 AB D7
```

Converting this to hexadecimal gives:

```text
d3d58c4786a6a229427351500ac7abd7
```

## Finding the Password

To calculate the MD5 hash of every candidate password, I created a helper script:

```python
import hashlib

candidates = [ ... ]

for pw in candidates:
    print(pw, hashlib.md5(pw.encode()).hexdigest())
```

After running the script, I compared the generated hashes against the target hash.

The matching entry was:

```text
8b95 -> d3d58c4786a6a229427351500ac7abd7
```

Therefore, the correct password was:

```text
8b95
```

## Exploitation

I executed the password checker:

```bash
python3 level4.py
```

and entered:

```text
8b95
```

The program successfully decrypted the flag and displayed:

```text
picoCTF{fl45h_5pr1ng1ng_cf341ff1}
```

## Flag

```text
picoCTF{fl45h_5pr1ng1ng_cf341ff1}
```
