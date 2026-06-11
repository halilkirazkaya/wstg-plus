## WSTG-INFO-06 — Mengidentifikasi Titik Masuk Aplikasi (Identify Application Entry Points)

Mengidentifikasi titik masuk aplikasi melibatkan pemetaan seluruh permukaan serangan (attack surface) dengan mengenumerasi semua kemungkinan cara pengguna atau sistem eksternal dapat berinteraksi dengan aplikasi. Proses ini mencakup penemuan semua URL, endpoint API, parameter (GET, POST, berbasis cookie), header HTTP, dan protokol khusus seperti WebSockets atau gRPC. Bagi seorang penetration tester, tahapan ini sangat krusial karena kerentanan sering ditemukan pada API "bayangan" (shadow API) yang tidak terdokumentasi, endpoint legacy, atau struktur parameter kompleks yang terlewatkan selama pengembangan. Enumerasi yang menyeluruh memastikan tidak ada komponen aplikasi yang tetap menjadi black box yang tidak teruji, sehingga mencegah penyerang menemukan rute yang tidak terpantau ke dalam sistem.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INFO-06 |
| **CWE** | CWE-1059 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Informasional |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/06-Identify_Application_Entry_Points  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite (Target/Proxy)`, `OWASP ZAP`, `ffuf`, `Arjun`, `LinkFinder`, `GoSpider`, `KiteRunner`

### Apakah semua parameter GET dan POST telah dienumerasi di seluruh aplikasi?
- [ ] Ya — enumerasi komprehensif melalui crawling otomatis dan manual **telah selesai**  
- [ ] Ya — hanya parameter umum yang telah diidentifikasi melalui crawling standar  
- [ ] Tidak — parameter sebagian besar **belum diidentifikasi** atau belum diuji  

### Apakah parameter yang tidak terdokumentasi atau tersembunyi (misalnya, debug, admin) dicari menggunakan fuzzing?
- [ ] Tidak — penemuan parameter tersembunyi **tidak diperlukan** untuk cakupan (scope) spesifik ini  
- [ ] Ya — fuzzing parameter (misalnya, menggunakan `Arjun`) **telah dilakukan** tanpa temuan  
- [ ] Ya — parameter tersembunyi **berhasil ditemukan** dan dipetakan untuk pengujian lebih lanjut  
- [ ] Tidak — penemuan parameter tersembunyi **tidak dilakukan**  

### Apakah semua metode HTTP yang didukung (PUT, DELETE, PATCH, dll.) telah diidentifikasi untuk setiap endpoint?
- [ ] Ya — penemuan metode **diterapkan** di seluruh endpoint yang ditemukan  
- [ ] Ya — penemuan metode **diterapkan** hanya pada endpoint yang berisiko tinggi atau sensitif  
- [ ] Tidak — hanya metode standar GET dan POST yang **diidentifikasi**  

### Apakah titik masuk non-standar seperti WebSockets, gRPC, atau header kustom telah dipetakan?
- [ ] Tidak — aplikasi **tidak** menggunakan protokol non-standar atau header kustom  
- [ ] Ya — semua titik masuk spesifik protokol dan header kustom **telah diidentifikasi**  
- [ ] Tidak — titik masuk non-standar **tersedia** namun belum dipetakan  

### Apakah seluruh permukaan API (termasuk endpoint versi seperti /v1/ atau /v2/) telah dipetakan sepenuhnya?
- [ ] Tidak — aplikasi **tidak** mengekspos API  
- [ ] Ya — semua versi dan endpoint API **telah diidentifikasi** dan didokumentasikan  
- [ ] Ya — hanya versi API produksi saat ini yang **diidentifikasi**  
- [ ] Tidak — endpoint API **tetap tidak terpetakan** atau hanya ditemukan sebagian  

---