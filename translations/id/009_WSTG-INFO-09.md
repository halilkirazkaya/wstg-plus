## WSTG-INFO-09 — Fingerprint Web Application

Fingerprinting sebuah aplikasi web adalah proses mengidentifikasi framework tertentu, Content Management System (CMS), atau tumpukan teknologi (technology stack) yang mendasarinya beserta versi terkaitnya. Fase pengintaian (reconnaissance) ini sangat penting karena memungkinkan penyerang untuk memetakan komponen yang teridentifikasi terhadap kerentanan yang diketahui (CVE) dan exploit publik, sehingga mempersempit permukaan serangan (attack surface) secara signifikan. Informasi biasanya dikumpulkan dari header respons HTTP, jalur file (file path) yang unik, nama cookie, metadata kode sumber HTML, dan hash kriptografis (cryptographic hashes) dari aset statis. Dengan menentukan technology stack secara akurat, penyerang dapat beralih dari pemindaian otomatis generik ke eksploitasi (exploitation) yang sangat tertarget pada kelemahan spesifik versi.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INFO-09 |
| **CWE** | CWE-200 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Rendah / Informasional |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/09-Fingerprint_Web_Application  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Wappalyzer`, `WhatWeb`, `BuiltWith`, `CMSmap`, `nmap`, `Nikto`, `Burp Suite`

### Apakah header respons HTTP mengungkapkan technology stack atau nomor versi?
- [ ] Tidak — header sensitif telah dihapus atau berisi nilai generik  
- [ ] Ya — header default seperti `X-Powered-By`, `Server`, atau `X-AspNet-Version` ditemukan  
- [ ] Ya — header mengungkapkan versi spesifik dan usang (outdated) dari web server atau framework aplikasi  

### Apakah framework aplikasi atau CMS dapat diidentifikasi melalui jalur file (file path) unik atau konvensi penamaan?
- [ ] Tidak — jalur file (file path) disamarkan (obfuscated) dan tidak mengungkapkan teknologi yang mendasarinya  
- [ ] Ya — struktur direktori umum (misalnya, `/wp-content/`, `/_next/`, `/node_modules/`) terlihat  
- [ ] Ya — file unik seperti `robots.txt`, `humans.txt`, atau `sitemap.xml` berisi metadata spesifik teknologi  

### Apakah nomor versi spesifik terpapar dalam kode sumber HTML atau aset statis?
- [ ] Tidak — tidak ada informasi versi yang ditemukan dalam komentar atau parameter aset  
- [ ] Ya — komentar HTML atau tag meta (misalnya, `<meta name="generator" content="...">`) ditemukan  
- [ ] Ya — string versi ditambahkan ke nama file CSS/JS (misalnya, `jquery.js?v=1.12.4`) dan tidak dapat dinonaktifkan  

### Apakah halaman kesalahan default atau antarmuka manajemen mengungkapkan lingkungan perangkat lunak?
- [ ] Tidak — aplikasi menggunakan halaman kesalahan khusus yang generik untuk semua kode status  
- [ ] Ya — halaman kesalahan default untuk web server atau framework diaktifkan dan membocorkan data versi  
- [ ] Ya — portal login administratif (misalnya, `/wp-admin`, `/phpmyadmin`) dapat diakses dan diidentifikasi  

### Apakah cookie mengungkapkan framework manajemen sesi (session management)?
- [ ] Tidak — cookie sesi menggunakan nama generik atau kustom  
- [ ] Ya — nama cookie spesifik teknologi (misalnya, `PHPSESSID`, `JSESSIONID`, `ASPSESSIONID`) digunakan  

---