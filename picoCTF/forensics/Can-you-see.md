# CanYouSee

## Platform
picoCTF

## Category
Forensics

## Difficulty
Easy

---

## Challenge Description

How about some hide and seek?

A JPEG image was provided. The goal is to find hidden information inside it.

Hints:
- How can you view the information about the picture?
- If something isn't in the expected form, maybe it deserves attention?

---

## Solution

### 1. Initial inspection

After downloading the file, I extracted it (if compressed) and inspected it using `exiftool`:

```bash
exiftool ukn_reality.jpg
````

---

### 2. Metadata analysis

The output showed standard image metadata, but one field stood out:

```text id="m1"
Attribution URL : cGljb0NURntNRTc0RDQ3QV9ISUREM05fYTZkZjhkYjh9Cg==
```

This value looked suspicious because it was not a normal URL format and appeared to be Base64 encoded.

---

### 3. Decoding hidden data

I decoded the Base64 string using CyberChef (or any Base64 decoder):

```bash id="m2"
echo "cGljb0NURntNRTc0RDQ3QV9ISUREM05fYTZkZjhkYjh9Cg==" | base64 -d
```

The decoded output revealed the flag.

---

## Flag

```text id="m3"
picoCTF{ME74D47A_HIDD3N_a6df8db8}
```

---

## Key Concepts

* Metadata analysis using `exiftool`
* Hidden data in non-standard EXIF fields
* Base64 decoding from image metadata
* Importance of checking unexpected metadata fields

---

## Summary

This challenge demonstrates that hidden data is often stored in image metadata fields that look normal at first glance. In this case, the `Attribution URL` field contained a Base64-encoded flag, which could be easily decoded once identified.

```
```
