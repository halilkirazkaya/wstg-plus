## WSTG-BUSL-09 — Test Upload of Malicious Files

Fungsionalitas unggah berkas memungkinkan pengguna untuk mengirimkan data ke server, yang memberikan vektor langsung untuk memasukkan konten berbahaya ke dalam lingkungan aplikasi. Penyerang mengeksploitasi logika validasi yang lemah untuk mengunggah Web Shell, Malware, atau berkas yang dirancang khusus—seperti SVG atau HTML—untuk mencapai Remote Code Execution (RCE) atau Cross-Site Scripting (XSS). Kerentanan ini biasanya ditemukan pada fitur-fitur seperti pengaturan profil, sistem manajemen dokumen, dan penanganan lampiran di mana server gagal memverifikasi tipe berkas, konten, dan izin eksekusi dengan benar. Eksploitasi yang berhasil sering kali menyebabkan kompromi sistem secara penuh, eksfiltrasi data, atau penggunaan server sebagai titik tumpu (pivot point) untuk serangan jaringan internal.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-BUSL-09 |
| **CWE** | CWE-434 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Kerentanan** | Tinggi / Kritis |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/09-Test_Upload_of_Malicious_Files  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**Alat:** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`, `Weevely`

### Apakah aplikasi menyediakan fungsionalitas untuk mengunggah berkas?
- [ ] Tidak — tidak ada fungsionalitas unggah berkas
- [ ] Ya — fungsionalitas tersedia untuk pengguna terautentikasi
- [ ] Ya — fungsionalitas tersedia untuk pengguna **tanpa autentikasi** *(Kritis)*

### Apakah validasi ekstensi berkas ditegakkan di sisi server?
- [ ] Ya — allowlist ekstensi yang ketat **diterapkan** dan bypass **tidak dimungkinkan**
- [ ] Ya — allowlist digunakan tetapi bypass **dimungkinkan** melalui kepekaan huruf (case sensitivity) atau ekstensi ganda
- [ ] Ya — blocklist digunakan, yang **dapat** dilompati dengan ekstensi alternatif (contoh: `.phtml`, `.asa`)
- [ ] Tidak — tidak ada validasi ekstensi yang **diterapkan** di sisi server

### Apakah konten berkas divalidasi selain dari ekstensi atau tipe MIME?
- [ ] Ya — aplikasi memverifikasi magic bytes dan melakukan inspeksi konten yang mendalam
- [ ] Ya — aplikasi hanya memeriksa header `Content-Type`, yang sangat mudah **dipalsukan** (spoofed)
- [ ] Tidak — aplikasi sepenuhnya bergantung pada ekstensi berkas untuk validasi

### Apakah berkas yang diunggah disimpan dalam direktori dengan izin eksekusi?
- [ ] Tidak — berkas disimpan pada layanan penyimpanan khusus (seperti S3) atau direktori non-executable
- [ ] Ya — berkas disimpan di server web tetapi eksekusi **dinonaktifkan** melalui konfigurasi
- [ ] Ya — berkas disimpan dalam direktori yang dapat diakses melalui web dan eksekusi **diaktifkan** *(Kritis)*

### Apakah filter keamanan dapat dilompati melalui manipulasi nama berkas?
- [ ] Tidak — nama berkas disanitasi atau dihasilkan ulang secara acak oleh server
- [ ] Ya — filter **dapat** dilompati menggunakan null bytes (contoh: `shell.php%00.jpg`)
- [ ] Ya — karakter Path Traversal (contoh: `../../`) **dapat** digunakan untuk mengunggah berkas ke direktori sembarang

---