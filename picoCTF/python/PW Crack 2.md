# PicoCTF - General Skills: PW Crack 2

## Description

The challenge provides a Python password checker and an encrypted flag file. The objective is to determine the correct password and use it to decrypt the flag.

## Analysis

I began by inspecting the provided Python script:

```bash
nano level2.py
```

The challenge hints suggested that the encoding should look familiar and that the `str_xor` function did not need to be reverse engineered. Therefore, I focused on finding how the password was stored in the source code.

Inside the script, I found the following password check:

```python
user_pw == chr(0x33) + chr(0x39) + chr(0x63) + chr(0x65)
```

The password was represented using hexadecimal ASCII values. Converting each value gives:

| Hex Value | ASCII Character |
| --------- | --------------- |
| 0x33      | 3               |
| 0x39      | 9               |
| 0x63      | c               |
| 0x65      | e               |

Combining the characters produces the password:

```text
39ce
```

## Exploitation

After identifying the password, I executed the script:

```bash
python3 level2.py
```

When prompted, I entered:

```text
39ce
```

The program accepted the password and decrypted the encrypted flag file.

Output:

```text
Please enter correct password for flag: 39ce
Welcome back... your flag, user:
picoCTF{tr45h_51ng1ng_502ec42e}
```

## Flag

```text
picoCTF{tr45h_51ng1ng_502ec42e}
```
