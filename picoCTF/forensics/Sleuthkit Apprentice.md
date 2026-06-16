# PicoCTF Challenge Writeup: Sleuthkit Apprentice

## Challenge Information

* **Name:** Sleuthkit Apprentice
* **Category:** Forensics
* **Difficulty:** Medium
* **Author:** LT 'syreal' Jones
* **Description:** Download this disk image and find the flag.

---

## Technical Overview

The challenge provides a compressed Linux disk image (`disk.flag.img.gz`). The objective is to analyze the partitions within the disk image, locate where the operating system files are stored, and find the flag hidden inside the file system using **The Sleuth Kit (TSK)** command-line utilities.

---

## Step-by-Step Solution

### Step 1: Extracting and Identifying the Disk Image

First, extract the compressed disk image file using `gzip`:

```bash
gzip -d disk.flag.img.gz

```

Next, use the `file` command to look at the partition layout of the extracted image (`disk.flag.img`):

```bash
file disk.flag.img

```

**Output Analysis:**
The output shows a DOS/MBR boot sector layout with three separate partitions:

1. **Partition 1 (Linux):** Starts at sector `2048`.
2. **Partition 2 (Linux Swap):** Starts at sector `206848`.
3. **Partition 3 (Linux):** Starts at sector `360448`.

---

### Step 2: Analyzing the Partition Layout with `mmls`

To confirm the exact sector offsets cleanly, use `mmls` (Media Management List) from The Sleuth Kit:

```bash
mmls disk.flag.img

```

**Output Result:**

```text
      Slot      Start        End          Length       Description
002:  000:000   0000002048   0000206847   0000204800   Linux (0x83)
003:  000:001   0000206848   0000360447   0000153600   Linux Swap / Solaris x86 (0x82)
004:  000:002   0000360448   0000614399   0000253952   Linux (0x83)

```

We have two potential Linux data targets: the partition at offset `2048` and the one at offset `360448`.

---

### Step 3: File System Examination

Checking the first Linux partition (`-o 2048`) with `fls` reveals it is simply the `/boot` partition hosting Linux kernel elements (`vmlinuz`, `initramfs`, `extlinux.conf`), which contains no relevant flag files.

Let's inspect the third partition at offset **`360448`**:

```bash
fls -o 360448 disk.flag.img

```

**Output Result:**

```text
d/d 451:	home
d/d 11:	lost+found
d/d 12:	boot
d/d 1985:	etc
...
d/d 1995:	root

```

This is clearly the Linux root directory (`/`).

---

### Step 4: Searching for the Flag File

Instead of manually clicking or stepping through every directory, perform a recursive directory listing (`-r`) on the root partition and filter the output using case-insensitive `grep` for terms like `flag` or `txt`:

```bash
fls -r -o 360448 disk.flag.img | grep -Ei "flag|txt"

```

**Output Result:**

```text
++ r/r * 2082(realloc):	flag.txt
++ r/r 2371:	flag.uni.txt

```

The tool discovered two interesting entries:

* Inode `2082` is a deleted/reallocated entry (`flag.txt`).
* Inode `2371` points directly to an active file named `flag.uni.txt`.

---

### Step 5: Extracting the Flag Content

Using the `icat` utility, read the content of Inode `2371` from the partition at offset `360448`:

```bash
icat -o 360448 disk.flag.img 2371

```

**Output:**

```text
picoCTF{by73_5urf3r_adac6cb4}

```

---

## Flag

`picoCTF{by73_5urf3r_adac6cb4}`