# CTF Writeup: Wireshark doo dooo do doo...

* **Category:** Forensics
* **Difficulty:** Medium
* **Author:** Dylan

## Challenge Description

> Can you find the flag?
> `shark1.pcapng`

---

## Tools Used

* **Wireshark**: Aplikasi *packet analyzer* untuk memeriksa berkas tangkapan jaringan (`.pcapng`).
* **CyberChef** (atau ROT13 Decoder): Alat berbasis web untuk melakukan dekripsi/dekode teks.

---

## Methodology

### Step 1: Membuka Berkas PCAPNG

Langkah pertama adalah membuka file berkas jaringan `shark1.pcapng` menggunakan Wireshark melalui terminal:

```bash
elok@elok:~/Downloads$ wireshark shark1.pcapng

```

### Step 2: Analisis Payload Jaringan (Follow TCP Stream)

Setelah lalu lintas jaringan terbuka di Wireshark, kita perlu mencari informasi yang mencurigakan atau pola teks yang menyerupai format flag.

1. Klik kanan pada salah satu paket HTTP/TCP.
2. Pilih menu **Follow** -> **TCP Stream**.
3. Sesuai dengan hasil analisis pada gambar `wireshark doo doo doo.png`, ubah nomor *stream* di pojok kanan bawah menjadi **Stream 5**.

Di dalam Stream 5, ditemukan sebuah respons `HTTP/1.1 200 OK` yang memuat teks tersamar (*ciphertext*) dengan format menyerupai struktur flag picoCTF:

```text
Gur synt vf cvbpPGS{c33xno00_1_f33_h_qrnqorrs}

```

### Step 3: Dekripsi Flag menggunakan ROT13

Teks `cvbpPGS` terlihat seperti hasil pergeseran karakter (cipher substitusi) dari kata `picoCTF`. Jarak pergeseran alfabet dari **c** ke **p** adalah 13, yang menandakan bahwa teks ini dienkripsi menggunakan metode **ROT13**.

Untuk mendapatkan bendera aslinya:

1. Salin teks `cvbpPGS{c33xno00_1_f33_h_qrnqorrs}`.
2. Masukkan teks tersebut ke dalam pembaca sandi ROT13 
!(seperti terlihat pada gambar `assets/decode rot13.png`).

Hasil dekode langsung memunculkan flag asli dalam format yang benar.

---

## Flag

```text
picoCTF{p33kab00_1_s33_u_deadbeef}

```