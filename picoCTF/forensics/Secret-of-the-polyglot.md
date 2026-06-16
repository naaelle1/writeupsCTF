````markdown id="poly1"
# Secret of the Polyglot

## Platform
picoCTF

## Category
Forensics

## Difficulty
Easy

---

## Challenge Description

We were given a file named:

```bash
flag2of2-final.pdf
````

When opened as a PDF, it only showed part of the flag, indicating that the file likely contained hidden data or multiple embedded formats.

---

## Solution

### 1. Opening the file as a PDF

I first opened the file normally using a PDF viewer:

```bash
xdg-open flag2of2-final.pdf
```

The visible content revealed a partial flag:

```text id="p1"
1n_pn9_&_pdf_53b741d6}
```

However, the flag was clearly incomplete.

---

### 2. Checking the actual file type

To verify the file structure, I used the `file` command:

```bash
file flag2of2-final.pdf
```

Output:

```text id="p2"
flag2of2-final.pdf: PNG image data, 50 x 50, 8-bit/color RGBA, non-interlaced
```

This revealed that the file was actually a PNG image despite its `.pdf` extension.

---

### 3. Analyzing file structure

To inspect embedded data, I used `binwalk`:

```bash
binwalk flag2of2-final.pdf
```

Output:

```text id="p3"
DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             PNG image, 50 x 50, 8-bit/color RGBA
914           0x392           PDF document, version: "1.4"
1149          0x47D           Zlib compressed data, default compression
```

This confirmed that the file is a **polyglot file**, containing both PNG and PDF data.

---

### 4. Extracting the hidden PDF

I searched for the PDF header inside the file:

```bash id="p4"
grep -abo "%PDF" flag2of2-final.pdf
```

Output:

```text id="p5"
914:%PDF
```

Then I extracted the PDF portion:

```bash id="p6"
dd if=flag2of2-final.pdf of=hidden.pdf bs=1 skip=914
```

Verification:

```bash id="p7"
file hidden.pdf
```

Output:

```text id="p8"
hidden.pdf: PDF document, version 1.4, 1 page(s)
```

---

### 5. Extracting the PNG content

The PNG portion contained another part of the flag:

```text id="p9"
picoCTF{f1u3n7_
```

---

### 6. Combining both parts

PNG output:

```text id="p10"
picoCTF{f1u3n7_
```

PDF output:

```text id="p11"
1n_pn9_&_pdf_53b741d6}
```

Final reconstructed flag:

```text id="p12"
picoCTF{f1u3n7_1n_pn9_&_pdf_53b741d6}
```

---

## Flag

```text id="p13"
picoCTF{f1u3n7_1n_pn9_&_pdf_53b741d6}
```

---

## Key Concepts

* Polyglot files (multiple valid file formats in one file)
* File signature analysis using `file`
* Embedded data detection using `binwalk`
* Header-based extraction using `grep` and `dd`

---

## Summary

This challenge demonstrates how a single file can simultaneously behave as both a PNG and a PDF. By analyzing file signatures and extracting embedded sections, both parts of the flag were recovered and combined.

```
```
