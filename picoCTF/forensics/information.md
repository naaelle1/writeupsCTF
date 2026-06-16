# information

## Platform
picoCTF

## Category
Forensics

## Difficulty
Easy

---

## Challenge Description

Files can always be changed in a secret way. Can you find the flag?

A file named `cat.jpg` was provided.

Hints:
- Look at the details of the file
- Make sure to submit the flag as picoCTF{XXXXX}

---

## Solution

### 1. Initial inspection

After downloading the image, I opened it normally and then checked its metadata using `exiftool`:

```bash
exiftool cat.jpg
````

---

### 2. Metadata analysis

Most fields looked normal, but one entry stood out:

```text id="m1"
License : cGljb0NURnt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9
```

This value clearly looked like a Base64-encoded string hidden inside image metadata.

---

### 3. Decoding hidden value

I decoded the string using CyberChef (or terminal):

```bash id="m2"
echo "cGljb0NURnt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9" | base64 -d
```

---

## Flag

```text id="m3"
picoCTF{the_m3tadata_1s_modified}
```

---

## Key Concepts

* Metadata inspection using `exiftool`
* Hidden data stored in non-obvious EXIF fields (e.g., License)
* Base64 decoding from image metadata
* Importance of checking all metadata fields, not just standard ones

---

## Summary

This challenge shows how image metadata can be intentionally modified to hide information. The flag was embedded in the `License` field and required Base64 decoding to recover.

```
```
