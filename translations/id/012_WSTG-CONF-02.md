## WSTG-CONF-02 — Pengujian Konfigurasi Platform Aplikasi

Pengujian konfigurasi platform aplikasi melibatkan audit terhadap pengaturan web server, application server, dan framework yang mendasarinya untuk memastikan semuanya telah diperkuat (hardened) terhadap eksploitasi umum. Penyerang secara aktif mencari kredensial default, kerentanan platform yang belum ditambal (unpatched), dan antarmuka administratif yang terekspos untuk mendapatkan akses tidak sah atau mengeksekusi kode. Kesalahan konfigurasi (misconfiguration) sering kali mengakibatkan pengungkapan variabel lingkungan yang sensitif, jalur sistem internal, atau keberadaan aplikasi contoh yang tidak perlu yang memperluas permukaan serangan (attack surface). Dari sudut pandang profesional, platform yang dikonfigurasi dengan buruk berfungsi sebagai titik masuk dengan probabilitas tinggi untuk mendapatkan akses awal ke lingkungan target.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CONF-02 |
| **CWE** | CWE-16 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Sedang / Tinggi* |

> *Tingkat keparahan menjadi Tinggi jika antarmuka administratif dapat diakses dengan kredensial default atau jika skrip contoh memungkinkan Remote Code Execution (RCE).

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/02-Test_Application_Platform_Configuration  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Nmap`, `Nikto`, `WhatWeb`, `Wappalyzer`, `Burp Suite`, `Curl`

### Apakah kredensial atau akun default aktif pada platform?
- [ ] Tidak — semua akun default **dinonaktifkan** atau kata sandi telah diubah  
- [ ] Ya — akun default ada tetapi **tidak dapat diakses** dari jaringan eksternal  
- [ ] Ya — akun default **aktif** dan dapat diakses melalui internet *(Kritis)*  

### Apakah platform mengungkapkan informasi versi yang sensitif melalui header atau halaman kesalahan?
- [ ] Tidak — string versi **disembunyikan** atau bersifat umum (generic)  
- [ ] Ya — informasi versi yang terperinci **diungkapkan** dalam header HTTP (misalnya, `Server`, `X-Powered-By`)  
- [ ] Ya — full stack traces dan jalur sistem internal **terekspos** dalam pesan kesalahan yang verbose  

### Apakah antarmuka administratif atau manajemen dibatasi dengan benar?
- [ ] Tidak — antarmuka administratif **tidak terekspos**  
- [ ] Ya — antarmuka tersedia tetapi **dibatasi** oleh allowlisting IP atau Multi-Factor Authentication (MFA)  
- [ ] Ya — antarmuka (misalnya, `/admin`, `/manager`, `/console`) **dapat diakses secara publik** dengan autentikasi faktor tunggal  

### Apakah file contoh, skrip, atau dokumentasi ada di server produksi?
- [ ] Tidak — semua file yang tidak penting telah **dihapus**  
- [ ] Ya — file contoh atau dokumentasi **ada** tetapi tidak membocorkan informasi sensitif  
- [ ] Ya — skrip contoh (misalnya, `info.php`, `examples/`) **ada** dan menyediakan vektor serangan langsung  

### Apakah server dikonfigurasi untuk mendukung metode HTTP yang tidak aman?
- [ ] Tidak — hanya `GET` dan `POST` yang **diaktifkan**  
- [ ] Ya — metode yang berpotensi berisiko seperti `PUT`, `DELETE`, atau `TRACE` **diaktifkan** tetapi dibatasi  
- [ ] Ya — metode berisiko **diaktifkan** dan memungkinkan modifikasi file yang tidak sah atau pencurian kredensial  

---