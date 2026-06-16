# RED

## Platform
picoCTF

## Category
Forensics / Steganography

## Difficulty
Easy

---

## Challenge Description

RED, RED, RED, RED

Download the image: `red.png`

Hints:
- The picture seems pure, but is it though?
- Red? Ged? Bed? Aed?
- Check whatever Facebook is called now.

---

## Solution

At first glance, the image looked completely normal, but the hints suggested hidden data inside the file, likely through steganography.

Based on prior experience, I used `zsteg` to analyze the PNG file.

---

## Initial Analysis

```bash
zsteg red.png
````

The output revealed multiple hidden layers. One of the most important findings was:

```text id="zst1"
b1,rgba,lsb,xy .. text: "cGljb0NURntyM2RfMXNfdGgzX3VsdDFtNHQzX2N1cjNfZjByXzU0ZG4zNTVffQ==cGljb0NURntyM2RfMXNfdGgzX3VsdDFtNHQzX2N1cjNfZjByXzU0ZG4zNTVffQ==..."
```

This was a repeated Base64-encoded string hidden in the least significant bits of the image.

---

## Extraction

I copied one of the Base64 strings and decoded it using CyberChef (or any Base64 decoder).

Decoded result:

```text id="zst2"
picoCTF{r3d_1s_th3_ult1m4t3_cur3_f0r_54dn355_}
```

---

## Flag

```text id="zst3"
picoCTF{r3d_1s_th3_ult1m4t3_cur3_f0r_54dn355_}
```

---

## Tools Used

* zsteg
* CyberChef (Base64 decoding)

---

## Key Takeaways

* PNG images often hide data in LSB (Least Significant Bit) layers
* `zsteg` is very effective for PNG steganography analysis
* Repeated encoded strings usually indicate valid hidden payloads
* Base64 is commonly used to encode hidden CTF flags
* Steganography often combines multiple techniques (LSB + encoding)

---

## Notes

This challenge demonstrates classic PNG steganography: hidden data embedded in pixel bits, extracted using specialized tools and then decoded into readable form.

```
```
