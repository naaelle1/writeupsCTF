
# Flag in Flame

## Platform
picoCTF

## Category
Forensics

## Difficulty
Easy

---

## Challenge Description

The SOC team discovered a suspiciously large log file after a recent breach. When they opened it, they found an enormous block of encoded text instead of typical logs. The goal is to inspect the file and uncover any hidden information within it.

Hint: Use base64 to decode the data and generate an image file.

---

## Solution

I started by downloading the provided log file. After opening it, I noticed that it contained a large block of Base64-encoded data.

Based on the hint, I decided to decode it using CyberChef.

### Step 1: Base64 Decoding

I used the following recipe in CyberChef:

- From Base64

After decoding, the output was an image containing a hidden message.

![CyberChef output](assets/flaginflame.png)
---

## Hidden Image

The decoded result produced an image that appeared to contain a hacker-style graphic. Inside the image, there were encoded characters embedded.

---

## Step 2: Extract Text from Image

To extract the hidden text from the image, I used OCR with `tesseract`.

```bash id="t1o2xv"
tesseract download.png output
````

Then I checked the extracted text:

```bash id="v9kq2m"
cat output.txt
```

The result contained a mix of noise and a hexadecimal string:

```text id="h3p7ld"
7069636F4354467B666F72656E736963735F616E616C797369735F69735F616D617A696E675F62396163346362397D
```

(Note: some spacing appeared during extraction and should be removed before decoding.)

---

## Step 3: Hex Decoding

The extracted string was hexadecimal, so I decoded it using CyberChef or a CLI tool.

### Example (CyberChef)

* From Hex

### Or CLI:

```bash id="x8zq1a"
echo "7069636F4354467B666F72656E736963735F616E616C797369735F69735F616D617A696E675F62396163346362397D" | xxd -r -p
```

---

## Flag

```text id="pico1f"
picoCTF{forensics_analysis_is_amazing_b9ac4cb9}
```

---

## Tools Used

* CyberChef (Base64 decoding, Hex decoding)
* Tesseract OCR
* xxd / hex decoder

---

## Key Takeaways

* Large Base64 blobs often represent files or images
* Decoded outputs may still contain hidden information inside images
* OCR tools like `tesseract` are useful for extracting embedded text
* Hex encoding is commonly used as a second-layer obfuscation
* CTF forensics challenges often require chaining multiple tools

---

## Notes

This challenge combined multiple forensics techniques: Base64 decoding → image analysis → OCR extraction → hex decoding. Each stage revealed the next clue in the chain.

```
```
