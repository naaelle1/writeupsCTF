
# Riddle Registry

## Platform
picoCTF

## Category
Forensics

## Difficulty
Easy

---

## Challenge Description

Hi, intrepid investigator! 📄🔍 You've stumbled upon a peculiar PDF filled with what seems like nothing more than garbled nonsense. But beware! Not everything is as it appears. Amidst the chaos lies a hidden treasure—an elusive flag waiting to be uncovered.

Find the PDF file here: *Hidden Confidential Document*  
Goal: uncover the flag within the metadata.

---

## Solution

I started by analyzing the PDF file using `exiftool`, since the challenge explicitly hinted that the flag might be hidden in the metadata.

```bash
exiftool confidential.pdf
````

The output showed several metadata fields:

```text
ExifTool Version Number         : 13.50
File Name                       : confidential.pdf
Directory                       : .
File Size                       : 183 kB
File Modification Date/Time     : 2026:06:11 10:57:14+07:00
File Access Date/Time           : 2026:06:11 10:57:27+07:00
File Inode Change Date/Time     : 2026:06:11 10:57:27+07:00
File Permissions                : -rw-rw-r--
File Type                       : PDF
File Type Extension             : pdf
MIME Type                       : application/pdf
PDF Version                     : 1.7
Linearized                      : No
Page Count                      : 1
Producer                        : PyPDF2
Author                          : cGljb0NURntwdXp6bDNkX20zdGFkYXRhX2YwdW5kIV9lZTQ1NDk1MH0=
```

At first glance, everything looked normal except for the **Author** field, which contained a suspicious string.

---

## Analysis

The `Author` field was encoded in Base64:

```text
cGljb0NURntwdXp6bDNkX20zdGFkYXRhX2YwdW5kIV9lZTQ1NDk1MH0=
```

This strongly suggested hidden data inside metadata, which is a common CTF forensics technique.

---

## Solution

I decoded the Base64 string using any decoder (CyberChef or CLI):

```bash
echo "cGljb0NURntwdXp6bDNkX20zdGFkYXRhX2YwdW5kIV9lZTQ1NDk1MH0=" | base64 -d
```

---

## Flag

```text
picoCTF{puzzl3d_m3tadata_f0und!_ee454950}
```

---

## Tools Used

* exiftool
* base64 decoder (CyberChef / CLI)

---

## Key Takeaways

* Metadata often contains hidden information in CTF challenges
* `exiftool` is the first tool to use for file metadata inspection
* Suspicious strings in fields like Author, Comment, or Producer should always be checked
* Base64 encoding is commonly used to hide flags in metadata

---

## Notes

This challenge highlights the importance of inspecting metadata carefully. Even when a file looks empty or meaningless, valuable data may still be hidden inside its attributes.

```
```
