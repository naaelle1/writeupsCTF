# Binary Digits

**CTF:** picoCTF
**Category:** Forensics
**Difficulty:** Easy
**Points:** 100

## Challenge Description

> This file doesn't look like much... just a bunch of 1s and 0s. But maybe it's not just random noise. Can you recover anything meaningful from this?

## Solution

I downloaded the challenge file and, to be honest, I didn't know what I was supposed to do at first. 

Since it was a Forensics challenge, I decided to inspect the file to see what I was dealing with.

First, I checked the file type:

```bash
file digits.bin
```
digits.bin: ASCII text, with very long lines (65536), with no line terminators


Then, I tried extracting readable strings from the file:

```bash
strings digits.bin
```

This time, I got a huge wall of `1`s and `0`s.

At that point, I looked back at the challenge title: **Binary Digits**.

That made me think that the binary output wasn't random and probably needed to be decoded.

I copied the binary text and pasted it into CyberChef. Then I used the following recipe:

* **From Binary**

  * Delimiter: `Space`
  * Byte Length: `8`
* **Render Image**

After running the recipe, an image appeared in the output. Hidden inside the image was the flag.

![CyberChef output](picoCTF/forensics/assets/binary-digits.png)

## Flag

```text
picoCTF{h1dd3n_1n_th3_b1n4ry_cc2099d3}
```

## What I Learned

* The `file` command is useful for identifying what kind of file you're working with.
* `strings` can reveal readable data hidden inside a file.
* Challenge titles often contain hints that point toward the intended solution.
* CyberChef is an incredibly useful tool for decoding and transforming data.
* Sometimes the solution is simpler than I expect, so I shouldn't overcomplicate things.

## Final Thoughts

This challenge was actually fun because it reminded me to pay attention to small hints. I was confused at first, but seeing the image appear in CyberChef felt really satisfying. It also taught me that trying basic forensic techniques before jumping to complicated methods can save a lot of time.