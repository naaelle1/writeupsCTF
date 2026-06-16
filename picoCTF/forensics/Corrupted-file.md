# Corrupted File

## Platform
picoCTF

## Category
Forensics

## Difficulty
Easy

---

## Challenge Description

We were given a file that appeared to be corrupted. The hint suggested that only a small number of bytes were incorrect and that the file header should be inspected. Since JPEG was mentioned, the goal was likely to repair a corrupted JPEG by fixing its magic bytes.

---

## Initial Inspection

After downloading the file, I first inspected its raw binary structure using `xxd`:

```bash
xxd Corrupted > Corrupted.hex
````

This converted the file into a hexadecimal representation so the header could be analyzed more easily.

I then opened the output file:

```bash
nano Corrupted.hex
```

At the beginning of the file, the header looked suspicious and did not match a valid JPEG signature.

---

## Identifying the Problem

A valid JPEG file should start with the following magic bytes:

```text
FF D8 FF E0 00 10 4A 46 49 46
```

However, the corrupted file began with something like:

```text
5c 78 ff e0 00 10 4a 46 49 46
```

This indicated that:

* The JPEG Start of Image (SOI) marker was corrupted
* The correct header `FF D8` was replaced with invalid bytes (`5C 78`)

---

## Fixing the File

To repair the file, I manually corrected the first two bytes in the hex representation.

### Before:

```text
5c 78
```

### After:

```text
ff d8
```

After fixing the header, I converted the hex file back into a binary JPEG:

```bash
xxd -r Corrupted.hex fixed.jpg
```

---

## Result

The reconstructed file opened successfully as an image. The issue was resolved because restoring the correct JPEG magic bytes allowed the system to properly recognize and decode the file.

---

## Conclusion

This challenge demonstrates a common forensics technique: file header repair.

Key takeaway: in many CTF forensics challenges, files that appear “broken” are often just slightly corrupted at the byte level. Fixing the correct magic number can be enough to fully recover the file.

---

## Tools Used

* xxd
* nano
* basic hex editing knowledge

---

## Key Takeaways

* File signatures (magic bytes) are critical for file identification
* JPEG files must start with `FF D8`
* Small corruption in headers can make a file appear broken
* Hex editing is a powerful forensic technique for file recovery

```
```
