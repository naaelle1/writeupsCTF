## Disk, disk, sleuth! – Write-up

### Problem Description

In this challenge, you are given a compressed disk image (`dds1-alpine.flag.img.gz`). The objective is to analyze the disk using Sleuth Kit utilities, specifically `srch_strings`, combined with basic Linux command-line filtering (“terminal-fu”), to locate a hidden flag inside the image.

---

### Solution Steps

First, the disk image is extracted:

```bash id="a1"
gzip -d dds1-alpine.flag.img.gz
```

After extraction, the file type is checked using `file`:

```bash id="a2"
file dds1-alpine.flag.img
```

Output:

```
DOS/MBR boot sector; partition 1 : ID=0x83, active, startsector 2048, 260096 sectors
```

This confirms it is a Linux disk image with a valid partition table.

---

### Partition Analysis

To inspect the partition layout, `mmls` is used:

```bash id="a3"
mmls dds1-alpine.flag.img
```

Output:

```
Slot      Start        End          Length       Description
001: -------   0000002047   0000002047   0000002048   Unallocated
002: 000:000   0000002048   0000262143   0000260096   Linux (0x83)
```

The disk contains a Linux partition, but this challenge does not require manual offset analysis. Instead, the flag is embedded somewhere in the raw disk data.

---

### Extracting Strings

The next step is to scan the disk image for readable strings using `srch_strings`:

```bash id="a4"
srch_strings dds1-alpine.flag.img | grep -i pico
```

The `grep -i pico` filter is used to search case-insensitively for any PicoCTF-related strings.

---

### Finding the Flag

The output reveals several matches, including the flag:

```
SAY picoCTF{f0r3ns1c4t0r_n30phyt3_5e56e786}
```

---

### Flag

```
picoCTF{f0r3ns1c4t0r_n30phyt3_5e56e786}
```

---

### Conclusion

This challenge demonstrates a common forensics technique: using `srch_strings` to quickly extract readable content from a raw disk image. Instead of manually mounting or carving files, the flag can be discovered efficiently by scanning for known patterns and filtering output with `grep`.

The key insight is that not all forensic challenges require deep filesystem traversal—sometimes a simple string scan is enough.
