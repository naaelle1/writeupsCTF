# DISKO 1

## Platform
picoCTF

## Category
Forensics

## Difficulty
Easy

---

## Challenge Description

Can you find the flag in this disk image?

A disk image file was provided for analysis.

Hint: Maybe strings could help.

---

## Solution

I started by extracting the compressed file since it was provided in `.gz` format.

```bash
gzip -d file.gz
````

This produced the disk image file `disko-1.dd`.

---

## Initial Inspection

At first, I tried searching directly using `grep`:

```bash
cat disko-1.dd | grep pico
```

This resulted in:

```text
grep: (standard input): binary file matches
```

So I switched to a more appropriate tool for binary inspection.

---

## Extracting Readable Strings

I used `strings` to extract human-readable content from the disk image:

```bash
strings disko-1.dd | grep "picoCTF{"
```

This immediately revealed the flag:

```text
picoCTF{1t5_ju5t_4_5tr1n9_be6031da}
```

---

## Flag

```text
picoCTF{1t5_ju5t_4_5tr1n9_be6031da}
```

---

## Tools Used

* gzip
* strings
* grep

---

## Key Takeaways

* Disk images often contain embedded readable strings
* `strings` is one of the fastest ways to extract hidden data
* `grep` alone is not ideal for binary files
* Simple tools are often enough for basic forensic challenges

```
```
