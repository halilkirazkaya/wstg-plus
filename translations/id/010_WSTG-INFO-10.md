## WSTG-INFO-10 — Pemetaan Arsitektur Aplikasi

Pemetaan arsitektur aplikasi melibatkan identifikasi teknologi yang mendasari, komponen infrastruktur, dan jalur aliran data yang mendukung aplikasi web. Dengan menganalisis header respons HTTP, pesan kesalahan, format cookie, dan ekstensi berkas, penguji dapat menyusun kembali skema server web, server aplikasi, database, dan integrasi pihak ketiga yang digunakan. Pengetahuan ini sangat mendasar untuk menyesuaikan serangan berikutnya, karena memungkinkan pemilihan platform-specific exploits dan identifikasi potensi bottleneck atau perangkat perantara yang salah konfigurasi seperti Load Balancer dan WAF. Seorang penyerang memanfaatkan peta struktural ini untuk berpindah dari pemindaian generik ke eksploitasi terarah pada software stack tertentu dan komponen-komponennya yang saling terhubung.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INFO-10 |
| **CWE** | CWE-200 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Informasional / Rendah* |

> *Keparahan menjadi Menengah jika topologi jaringan internal atau alamat IP privat terungkap.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/10-Map_Application_Architecture  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Wappalyzer`, `BuiltWith`, `nmap`, `WhatWeb`, `Burp Suite`, `Netcat`, `curl`

### Dapatkah teknologi server web dan server aplikasi diidentifikasi secara akurat?
- [ ] Tidak — identifikasi tidak dimungkinkan karena hardening atau obfuskasi yang efektif  
- [ ] Ya — jenis teknologi diketahui tetapi versi spesifik tidak dapat ditentukan  
- [ ] Ya — teknologi dan versi spesifik diungkapkan sepenuhnya melalui header atau tanda tangan berkas *(Informasional)*  

### Apakah perangkat perantara (WAF, Load Balancer, Proxy) terdeteksi dalam jalur komunikasi?
- [ ] Tidak — tidak ada perangkat perantara yang terdeteksi atau mereka sepenuhnya transparan  
- [ ] Ya — keberadaan dicurigai melalui waktu atau perilaku, tetapi identitas tidak dikonfirmasi  
- [ ] Ya — produk spesifik (misalnya, Cloudflare, Nginx, F5) diidentifikasi melalui header unik atau perilaku  

### Apakah aplikasi membocorkan informasi tentang struktur direktori internal atau topologi jaringan?
- [ ] Tidak — jalur internal dan IP tidak diungkapkan  
- [ ] Ya — jalur internal bocor dalam pesan kesalahan atau stack trace tetapi IP internal tidak  
- [ ] Ya — baik struktur direktori internal maupun alamat IP privat diungkapkan  

### Dapatkah jenis dan versi database backend disimpulkan dari perilaku aplikasi?
- [ ] Tidak — rincian database tidak dapat ditentukan  
- [ ] Ya — jenis database (misalnya, PostgreSQL, MSSQL) disimpulkan melalui pesan kesalahan atau perilaku  
- [ ] Ya — jenis dan versi database diungkapkan secara eksplisit dalam bodi respons atau header  

### Apakah aplikasi di-host pada penyedia layanan cloud yang dapat diidentifikasi atau CDN pihak ketiga tertentu?
- [ ] Tidak — infrastruktur hosting tidak dapat diidentifikasi  
- [ ] Ya — penyedia cloud (AWS, Azure, GCP) diidentifikasi tetapi layanan spesifik tidak  
- [ ] Ya — layanan cloud spesifik (misalnya, S3 bucket, Lambda, Azure Functions) telah dipetakan dan dapat dijangkau  

---