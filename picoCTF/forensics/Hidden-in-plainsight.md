````markdown
# Hidden in Plain Sight

## Platform
picoCTF

## Category
Forensics

## Difficulty
Easy

---

## Challenge Description

You’re given a seemingly ordinary JPG image. Something is tucked away out of sight inside the file. Your task is to discover the hidden payload and extract the flag.

Hint: Read the image metadata.

---

## Solution

I started by downloading the image and checking its metadata using `exiftool`, since the hint clearly suggested metadata inspection.

```bash
exiftool img.jpg
````

The output showed normal image information, but one field stood out:

```text id="m9qk3v"
Comment                         : c3RlZ2hpZGU6Y0VGNmVuZHZjbVE9
```

This looked like a Base64-encoded string, so I decoded it using CyberChef.

The result was:

```text id="p2xv7n"
steghide:cEF6endvcmQ=
```

This indicated another layer of encoding. I decoded the second Base64 string:

```text id="q8r1ld"
cEF6endvcmQ=
```

After decoding, I got:

```text
pAzzword
```

This strongly suggested a passphrase for a steganography tool.

---

## Extraction

Based on the hint, I used `steghide` to extract hidden data from the image.

```bash id="t6wzpa"
steghide extract -sf img.jpg
```

When prompted for a passphrase, I entered:

```text
pAzzword
```

The extraction succeeded and produced a file:

```bash
wrote extracted data to "flag.txt"
```

I then read the file:

```bash id="u1kq9m"
cat flag.txt
```

---

## Flag

```text id="z0v4nc"
picoCTF{h1dd3n_1n_1m4g3_f051f2e8}
```

---

## Tools Used

* exiftool
* CyberChef (Base64 decoding)
* steghide

---

## Key Takeaways

* Metadata fields like Comment can hide encoded data
* Base64 strings may be layered and require multiple decodings
* steghide is commonly used for image-based steganography
* Hidden clues often point directly to the next required tool
* CTF forensics challenges are often multi-layered rather than single-step

---

## Notes

This challenge demonstrated a typical multi-stage forensics flow: metadata inspection → decoding → steganography extraction. Each step revealed a hint for the next tool.

```
```
