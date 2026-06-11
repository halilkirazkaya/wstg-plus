## WSTG-CONF-04 — Meninjau Cadangan Lama dan Berkas Tidak Terreferensi untuk Informasi Sensitif

Meninjau berkas cadangan (backup) lama dan berkas yang tidak terreferensi melibatkan identifikasi berkas yang terlupakan, bersifat sementara, atau tersembunyi pada server web yang tidak ditujukan untuk akses publik. Berkas-berkas ini sering kali mencakup cadangan kode sumber (`.zip`, `.bak`), berkas swap editor teks (`.swp`, `~`), atau metadata kontrol sumber (`.git`, `.svn`) yang dapat membocorkan informasi sensitif. Penyerang menggunakan daftar kata (wordlist) otomatis dan alat penemuan untuk melakukan Brute Force pada konvensi penamaan umum, dengan tujuan mengeksfiltrasi kredensial basis data, API keys yang tertanam (hardcoded), atau logika yang memfasilitasi eksploitasi lebih lanjut. Kerentanan ini biasanya berasal dari pemeliharaan server secara manual atau alur kerja CI/CD yang cacat sehingga gagal melakukan sanitasi pada direktori utama (web root) produksi.

| Kolom | Nilai |
|---|---|
| **WSTG ID** | WSTG-CONF-04 |
| **CWE** | CWE-530 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Sedang / Tinggi* |

> *Keparahan menjadi Tinggi jika berkas konfigurasi, arsip kode sumber, atau kredensial ditemukan.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/04-Review_Old_Backup_and_Unreferenced_Files_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `ffuf`, `dirsearch`, `gobuster`, `GitTools`, `Burp Suite (Engagement Tools)`, `Wfuzz`

### Apakah daftar direktori (directory listing) diaktifkan pada server web?
- [ ] Tidak — daftar direktori **dinonaktifkan** dan mengembalikan kesalahan 403 Forbidden atau kesalahan khusus  
- [ ] Ya — daftar direktori **diaktifkan** pada direktori non-sensitif  
- [ ] Ya — daftar direktori **diaktifkan** pada direktori yang berisi kode sumber atau berkas sensitif *(Kritis)*  

### Apakah berkas cadangan dengan ekstensi umum (misalnya, .bak, .old, .save) dapat ditemukan?
- [ ] Tidak — ekstensi cadangan umum **tidak ditemukan** atau diblokir oleh konfigurasi server  
- [ ] Ya — berkas cadangan ada tetapi **tidak** berisi informasi sensitif  
- [ ] Ya — berkas cadangan untuk konfigurasi atau kode sumber **dapat** diakses *(Tinggi)*  

### Apakah direktori kontrol sumber (misalnya, .git, .svn) atau metadata terpapar?
- [ ] Tidak — metadata kontrol sumber **tidak ada** atau diblokir dengan benar  
- [ ] Ya — metadata ada tetapi repositori lengkap **tidak dapat** dikonstruksi ulang  
- [ ] Ya — seluruh repositori kode sumber **dapat** dieksfiltrasi melalui metadata yang terpapar  

### Apakah arsip terkompresi (misalnya, .zip, .tar.gz, .rar) dari aplikasi tersedia di web root?
- [ ] Tidak — tidak ada berkas arsip sensitif yang ditemukan melalui brute-force atau enumerasi  
- [ ] Ya — arsip ditemukan tetapi dilindungi kata sandi atau berisi aset publik  
- [ ] Ya — arsip yang tidak terenkripsi yang berisi kode sumber atau data aplikasi **dapat** diakses secara publik  

### Apakah aplikasi membocorkan berkas sementara yang dibuat oleh editor teks atau IDE?
- [ ] Tidak — berkas sementara seperti `.swp`, `~`, atau `.DS_Store` **tidak ada**  
- [ ] Ya — berkas sementara yang tidak sensitif **ditemukan**  
- [ ] Ya — berkas sementara mengungkapkan potongan kode sumber atau jalur (path) internal  

---