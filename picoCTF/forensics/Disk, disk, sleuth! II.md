## Challenge Information

**Challenge Name:** Disk, disk, sleuth! II
**Category:** Forensics
**Difficulty:** Medium
**Author:** syreal

### Description

> All we know is the file with the flag is named `down-at-the-bottom.txt`...

**File Provided:**

* `dds2-alpine.flag.img.gz`

---

# Objective

The goal of this challenge is to locate the file named `down-at-the-bottom.txt` inside the provided disk image and retrieve the flag contained within it using Sleuth Kit tools.

---

# Tools Used

* `file`
* `gunzip` / `gzip`
* `mmls`
* `fls`
* `icat`
* `grep`
* Linux Terminal (Ubuntu)

---

# Solution

## 1. Extract the Disk Image

The provided file was compressed using gzip, so the first step was to extract it.

### Command

```bash
gunzip dds2-alpine.flag.img.gz
```

Alternatively:

```bash
gzip -d dds2-alpine.flag.img.gz 
```

### Output

After extraction, the resulting file was:

```text
dds2-alpine.flag.img
```

---

## 2. Identify the File Type

Before analyzing the image, I determined its format using the `file` command.

### Command

```bash
file dds2-alpine.flag.img
```

### Output

```text
dds2-alpine.flag.img: DOS/MBR boot sector; partition 1 : ID=0x83, active, start-CHS (0x0,32,33), end-CHS (0x10,81,1), startsector 2048, 260096 sectors

```


## 3. Examine the Partition Layout

If the image contains partitions, Sleuth Kit's `mmls` can display them.

### Command

```bash
mmls dds2-alpine.flag.img
```

### Output

```text
DOS Partition Table
Offset Sector: 0
Units are in 512-byte sectors

      Slot      Start        End          Length       Description
000:  Meta      0000000000   0000000000   0000000001   Primary Table (#0)
001:  -------   0000000000   0000002047   0000002048   Unallocated
002:  000:000   0000002048   0000262143   0000260096   Linux (0x83)

```

### Analysis

The Linux partition started at sector:

```text
Offset: 2048
```

This offset would be used in subsequent Sleuth Kit commands.

---

## 4. Search for the Target File

The challenge description revealed that the flag was stored inside a file named:

```text
down-at-the-bottom.txt
```

To locate this file, I listed all files recursively.

### Command

If using an offset:

```bash
fls -r -o 2048 dds2-alpine.flag.img | grep "down-at-the-bottom"
```

Without an offset:

```bash
fls -r dds2-alpine.flag.img | grep "down-at-the-bottom"
```

### Output

```text
+ r/r 18291:	down-at-the-bottom.txt
```

### Analysis

The file was found with inode:

```text
Inode: 18291
```

---

## 5. Recover the File Contents

After obtaining the inode number, I extracted the file using `icat`.

### Command

If using an offset:

```bash
icat -o 2048 dds2-alpine.flag.img INODE
```

Without an offset:

```bash
icat dds2-alpine.flag.img INODE
```

Example:

```bash
icat -o ______ dds2-alpine.flag.img ______
```

### Output

```text
   _     _     _     _     _     _     _     _     _     _     _     _     _  
  / \   / \   / \   / \   / \   / \   / \   / \   / \   / \   / \   / \   / \ 
 ( p ) ( i ) ( c ) ( o ) ( C ) ( T ) ( F ) ( { ) ( f ) ( 0 ) ( r ) ( 3 ) ( n )
  \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/ 
   _     _     _     _     _     _     _     _     _     _     _     _     _  
  / \   / \   / \   / \   / \   / \   / \   / \   / \   / \   / \   / \   / \ 
 ( s ) ( 1 ) ( c ) ( 4 ) ( t ) ( 0 ) ( r ) ( _ ) ( n ) ( 0 ) ( v ) ( 1 ) ( c )
  \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/ 
   _     _     _     _     _     _     _     _     _     _     _  
  / \   / \   / \   / \   / \   / \   / \   / \   / \   / \   / \ 
 ( 3 ) ( _ ) ( 4 ) ( b ) ( d ) ( 7 ) ( 2 ) ( 1 ) ( f ) ( 2 ) ( } )
  \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/   \_/ 

```

---

# Flag

```text
picoCTF{f0r3ns1c4t0r_n0v1c3_4bd721f2}
```

---

# Conclusion

This challenge demonstrates how Sleuth Kit can be used to investigate disk images efficiently. By identifying the filesystem structure, locating a specific filename with `fls`, and recovering its contents using `icat`, the hidden flag could be extracted successfully.

Key concepts learned from this challenge include:

* Extracting compressed forensic images.
* Identifying disk image structures.
* Enumerating files recursively with `fls`.
* Understanding inode-based recovery.
* Recovering files from disk images using `icat`.

---

# Commands Summary

```bash
gunzip dds2-alpine.flag.img.gz

file dds2-alpine.flag.img

mmls dds2-alpine.flag.img

fls -r -o OFFSET dds2-alpine.flag.img | grep "down-at-the-bottom"

icat -o OFFSET dds2-alpine.flag.img INODE
```

---

## Lessons Learned

* Always identify the image structure before beginning analysis.
* Challenge descriptions often provide valuable clues.
* Knowing the filename can significantly simplify forensic investigations.
* Sleuth Kit tools complement each other in a structured workflow.
