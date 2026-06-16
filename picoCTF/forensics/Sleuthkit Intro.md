## Sleuthkit Intro – Write-up

### Problem Description

You are given a compressed disk image (`disk.img.gz`). The task is to analyze the disk using Sleuth Kit, specifically the `mmls` command, in order to determine the size of the Linux partition. After finding the correct value, you must submit it to a remote checker service to retrieve the flag.

---

### Solution Steps

First, the disk image is extracted using gzip:

```bash
gzip -d disk.img.gz
```

After extraction, the partition layout is examined using `mmls`:

```bash
mmls disk.img
```

The output shows the partition table:

```
DOS Partition Table
Offset Sector: 0
Units are in 512-byte sectors

Slot      Start        End          Length       Description
000: Meta      0000000000   0000000000   0000000001   Primary Table (#0)
001: -------   0000000000   0000002047   0000002048   Unallocated
002: 000:000   0000002048   0000204799   0000202752   Linux (0x83)
```

---

### Analysis

From the `mmls` output, the Linux partition is:

* Start sector: 2048
* End sector: 204799
* Length: **202752 sectors**

The important value here is the **Length column**, which represents the size of the partition in sectors.

---

### Submitting the Answer

Next, connect to the remote service:

```bash
nc saturn.picoctf.net 58122
```

When prompted:

```
What is the size of the Linux partition in the given disk image?
Length in sectors:
```

The correct answer is:

```
202752
```

---

### Flag

After submitting the correct value, the service returns the flag:

```
picoCTF{mm15_f7w!}
```

---

### Conclusion

This challenge demonstrates the basic use of Sleuth Kit’s `mmls` tool to analyze disk partition tables. The key takeaway is that the partition size is directly given in the **Length** field, so no additional calculation is required.
