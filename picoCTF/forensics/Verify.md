# Verify

## Platform
picoCTF

## Category
Forensics / Cryptography

## Difficulty
Easy

---

## Challenge Description

In this challenge, we were given a SHA-256 checksum and a directory containing multiple files. Most of them were fake, and only one file matched the provided checksum. The goal was to find the correct file and decrypt it using a provided script.

---

## Challenge Information

- Host: rhea.picoctf.net  
- Port: 49565  
- Username: ctf-player  
- Password: (provided in challenge)  

SHA-256 checksum:

```text id="sha1"
467a10447deb3d4e17634cacc2a68ba6c2bb62a6637dad9145ea673bf0be5e02
````

---

## Solution

### 1. SSH into the server

I started by connecting to the remote server via SSH:

```bash id="ssh1"
ssh -p 49565 ctf-player@rhea.picoctf.net
```

When prompted about the fingerprint, I accepted it:

```text id="ssh2"
yes
```

Then I entered the provided password.

---

### 2. Checking available files

After logging in, I listed the directory contents:

```bash id="ls1"
ls
```

Output:

```text id="ls2"
checksum.txt  decrypt.sh  files
```

I confirmed the expected checksum:

```bash id="cat1"
cat checksum.txt
```

Output:

```text id="cat2"
467a10447deb3d4e17634cacc2a68ba6c2bb62a6637dad9145ea673bf0be5e02
```

---

### 3. Finding the correct file

The `files/` directory contained many candidate files. I computed SHA-256 hashes for all of them:

```bash id="hash1"
sha256sum files/*
```

To filter the correct one, I used:

```bash id="grep1"
sha256sum files/* | grep 467a10447deb3d4e17634cacc2a68ba6c2bb62a6637dad9145ea673bf0be5e02
```

The matching result was:

```text id="match1"
467a10447deb3d4e17634cacc2a68ba6c2bb62a6637dad9145ea673bf0be5e02  files/c6c8b911
```

So the valid file was:

```text id="file1"
files/c6c8b911
```

---

### 4. Decrypting the file

I then used the provided script to decrypt the file:

```bash id="dec1"
./decrypt.sh files/c6c8b911
```

The script output revealed the flag.

---

## Flag

```text id="flag1"
picoCTF{REDACTED}
```

*(Replace with the actual flag output from the script.)*

---

## Key Concepts

* SHA-256 hashing for file integrity verification
* Identifying correct files among decoys using hash comparison
* Basic SSH interaction in remote CTF environments
* Using scripts provided in challenges for final extraction

---

## Summary

The core idea of this challenge was verifying file authenticity using SHA-256. Instead of manually inspecting content, the correct approach was to compute hashes for all candidates and match them against the given checksum. Once the correct file was identified, the provided decryption script revealed the final flag.

```
```
