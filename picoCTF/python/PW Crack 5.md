# PicoCTF - General Skills: PW Crack 5

## Description

The challenge provides a password checker script, an encrypted flag file, a hash file, and a dictionary containing possible passwords. The goal is to recover the correct password and decrypt the flag.

## Analysis

I first inspected the source code:

```bash
cat level5.py
```

From the script, I observed that the password verification uses the MD5 hashing algorithm:

```python
def hash_pw(pw_str):
    pw_bytes = bytearray()
    pw_bytes.extend(pw_str.encode())
    m = hashlib.md5()
    m.update(pw_bytes)
    return m.digest()
```

The script compares the MD5 digest of the user-supplied password with the contents of `level5.hash.bin`.

Unlike the previous PW Crack challenges, no list of candidate passwords was included in the source code. Instead, a dictionary file containing possible passwords was provided.

## Solution

To automate the search, I created a Python script that reads each password from the dictionary, computes its MD5 hash, and compares it against the target hash.

```python
import hashlib

target = "E8352E76E260A31EB266012F70DF9A10"

with open("dictionary.txt") as f:
    for line in f:
        pw = line.strip()

        if hashlib.md5(pw.encode()).hexdigest() == target:
            print("Password found:", pw)
            break
```

The `strip()` function removes the newline character from each dictionary entry before hashing.

I saved the script as `crack.py` and executed it:

```bash
python3 crack.py
```

Output:

```text
Password found: 7e5f
```

Therefore, the correct password was:

```text
7e5f
```

## Exploitation

I then ran the challenge script:

```bash
python3 level5.py
```

and entered the recovered password:

```text
Please enter correct password for flag: 7e5f
```

The program successfully decrypted the flag and displayed:

```text
Welcome back... your flag, user:
picoCTF{h45h_sl1ng1ng_40f26f81}
```

## Flag

```text
picoCTF{h45h_sl1ng1ng_40f26f81}
```
