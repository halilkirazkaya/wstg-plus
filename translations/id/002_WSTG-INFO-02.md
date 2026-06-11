## WSTG-INFO-02 — Fingerprint Web Server

Fingerprinting web server adalah proses mengidentifikasi tipe perangkat lunak tertentu, versi, dan sistem operasi yang mendasari dari sebuah web server target. Penyerang melakukan pengintaian (reconnaissance) ini untuk mengidentifikasi kerentanan yang diketahui, seperti CVE yang belum ditambal atau kelemahan konfigurasi, yang spesifik untuk versi Apache, Nginx, IIS, atau teknologi server lainnya. Hal ini biasanya dicapai dengan menganalisis header respons HTTP (misalnya, `Server`, `X-Powered-By`), memeriksa perilaku protokol yang unik, atau mengamati informasi yang bocor melalui halaman kesalahan (error pages) default dan file sistem. Fingerprinting yang berhasil memungkinkan penyerang untuk menyesuaikan strategi eksploitasi mereka, berpindah dari pemindaian generik ke serangan dengan probabilitas tinggi terhadap celah arsitektur yang diketahui.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INFO-02 |
| **CWE** | CWE-200 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Informasional / Rendah* |

> *Tingkat keparahan menjadi Tinggi (High) jika fingerprinting mengungkapkan versi server yang usang atau sudah mencapai akhir masa pakai (end-of-life/EOL) dengan kerentanan kritis yang diketahui.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/02-Fingerprint_Web_Server  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**Alat:** `curl`, `Nmap`, `WhatWeb`, `Wappalyzer`, `Nikto`, `Netcat`

### Apakah terdapat string identifikasi dalam header respons HTTP?
- [ ] Tidak — header `Server` dan `X-Powered-By` **tidak ada** atau berisi nilai generik  
- [ ] Ya — header mengungkapkan perangkat lunak server tetapi **tidak** nomor versi spesifik  
- [ ] Ya — header mengungkapkan perangkat lunak server spesifik dan nomor versi **persis**  

### Apakah halaman kesalahan default atau respons buatan sistem membocorkan rincian server?
- [ ] Tidak — halaman kesalahan kustom digunakan dan **tidak** mengungkapkan informasi server  
- [ ] Ya — halaman kesalahan default membocorkan nama perangkat lunak server dan/atau versi  
- [ ] Ya — halaman kesalahan membocorkan rincian server, versi, dan **sistem operasi yang mendasari**  

### Apakah versi server diungkapkan melalui file default atau struktur direktori?
- [ ] Tidak — file instalasi default, panduan, dan skrip pengujian telah **dihapus**  
- [ ] Ya — file default (misalnya, `info.php`, `manual/`, `test.html`) **tersedia** dan mengungkapkan data versi  

### Dapatkah tipe server disimpulkan secara akurat melalui analisis perilaku yang unik?
- [ ] Tidak — respons server dinormalisasi dan fingerprinting berbasis perilaku **tidak dimungkinkan**  
- [ ] Ya — perilaku unik (misalnya, urutan header, kode kesalahan tertentu, atau keunikan stack TCP/IP) **memungkinkan** identifikasi server  

### Apakah versi server yang teridentifikasi saat ini didukung dan telah ditambal (patched)?
- [ ] Ya — versi yang teridentifikasi adalah versi terbaru dan **didukung** oleh vendor  
- [ ] Tidak — versi yang teridentifikasi sudah **usang** atau **end-of-life (EOL)** dengan kerentanan yang diketahui  

---