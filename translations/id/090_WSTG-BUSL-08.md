## WSTG-BUSL-08 — Uji Unggah Jenis File yang Tidak Terduga

Mengunggah jenis file yang tidak terduga memungkinkan penyerang untuk melewati batasan logika bisnis dengan mengirimkan file yang tidak dimaksudkan untuk diproses oleh aplikasi, yang berpotensi menyebabkan eksekusi kode jarak jauh (Remote Code Execution), Cross-Site Scripting (XSS), atau Denial of Service (DoS). Penyerang menguji kerentanan ini dengan memodifikasi ekstensi file, MIME types, dan magic bytes untuk menipu logika validasi sisi server (server-side) agar menerima konten berbahaya. Hal ini biasanya terjadi pada unggahan foto profil, sistem manajemen dokumen, atau lampiran tiket dukungan di mana validasi yang tidak memadai tersedia pada bagian back-end. Eksploitasi yang berhasil dapat membahayakan server host jika file yang diunggah dieksekusi, atau menyebabkan serangan sisi klien (client-side) jika file tersebut disajikan kembali kepada pengguna lain dengan Content-Type yang salah.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-BUSL-08 |
| **CWE** | CWE-434 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi / Kritis |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/08-Test_Upload_of_Unexpected_File_Types  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**Alat:** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`

### Apakah aplikasi membatasi unggahan file ke set ekstensi tertentu?
- [ ] Ya — whitelist ekstensi yang ketat diterapkan dan bypass **tidak dimungkinkan**  
- [ ] Ya — whitelist ekstensi tersedia tetapi bypass **dimungkinkan** melalui ekstensi ganda atau null bytes  
- [ ] Tidak — ekstensi file apa pun **dapat** diunggah  

### Apakah konten file divalidasi lebih dari sekadar ekstensi file?
- [ ] Ya — logika sisi server memverifikasi Magic Bytes dan melakukan inspeksi konten secara mendalam  
- [ ] Ya — logika sisi server memverifikasi MIME type hanya melalui header `Content-Type`  
- [ ] Tidak — tidak ada validasi konten yang **diterapkan** setelah pemeriksaan ekstensi  

### Dapatkah penyerang mengeksekusi kode dengan mengunggah web shell atau skrip?
- [ ] Tidak — file yang diunggah disimpan dalam direktori non-executable atau bucket penyimpanan (S3/Azure Blob)  
- [ ] Ya — file disimpan dalam direktori yang dapat diakses web tetapi eksekusi **dinonaktifkan** melalui konfigurasi server  
- [ ] Ya — skrip berbahaya yang diunggah **dapat** dieksekusi langsung melalui jalur URL yang dapat diprediksi *(Kritis)*  

### Apakah serangan sisi klien seperti XSS dimungkinkan melalui jenis file yang diunggah?
- [ ] Tidak — file disajikan dengan `Content-Disposition: attachment` dan `X-Content-Type-Options: nosniff`  
- [ ] Ya — file SVG atau HTML **dapat** diunggah dan dirender di browser, sehingga menyebabkan XSS  
- [ ] Ya — metadata gambar (EXIF) **diproses** dan direfleksikan dalam UI tanpa sanitasi (sanitization)  

---