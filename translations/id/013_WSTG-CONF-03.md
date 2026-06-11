## WSTG-CONF-03 — Menguji Penanganan Ekstensi Berkas untuk Informasi Sensitif

Pengujian penanganan ekstensi berkas melibatkan identifikasi apakah server web atau server aplikasi mengungkapkan informasi sensitif dengan menyajikan berkas yang seharusnya dibatasi atau dieksekusi. Penyerang sering kali mencari berkas cadangan (backup), berkas konfigurasi, dan fragmen kode sumber (misalnya, `.bak`, `.old`, `.env`, `.inc`) yang mungkin tertinggal di *web root* dan disajikan sebagai teks biasa karena kurangnya pemetaan *handler* yang spesifik. Kerentanan ini penting karena dapat menyebabkan terbukanya kredensial basis data, API keys, dan logika bisnis internal, yang memfasilitasi eksploitasi infrastruktur lebih lanjut. Paparan ini biasanya terjadi di direktori publik, folder unggahan, atau melalui pengaturan MIME-type yang salah dikonfigurasi pada server.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CONF-03 |
| **CWE** | CWE-552 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Sedang / Tinggi* |

> *Tingkat keparahan menjadi Tinggi jika berkas konfigurasi yang berisi kredensial atau kode sumber lengkap dapat diakses.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/03-Test_File_Extensions_Handling_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `ffuf`, `gobuster`, `dirsearch`, `Burp Suite (Intruder)`, `Wfuzz`

### Apakah ekstensi berkas cadangan atau sementara yang sensitif (misalnya, `.bak`, `.old`, `.swp`, `~`) dapat diakses?
- [ ] Tidak — server mengembalikan 403 Forbidden atau 404 Not Found untuk ekstensi cadangan yang umum  
- [ ] Ya — server mengizinkan pengunduhan berkas cadangan tetapi **tidak** mengandung data sensitif  
- [ ] Ya — server mengizinkan pengunduhan berkas cadangan yang berisi data sensitif atau kode sumber *(Tinggi)*  

### Apakah server mengekspos berkas lingkungan atau konfigurasi sistem (misalnya, `.env`, `.config`, `.yml`, `.ini`)?
- [ ] Tidak — berkas konfigurasi yang dibatasi **tidak dapat** diakses dan mengembalikan 403/404  
- [ ] Ya — berkas konfigurasi sensitif **dapat** diakses dan mengungkapkan rahasia internal atau kredensial  

### Apakah kode sumber dapat diambil dengan menambahkan ekstensi alternatif (misalnya, `.php.txt`, `.jsp.old`, `.aspx.bak`)?
- [ ] Tidak — kode aplikasi **tidak** ditampilkan sebagai teks biasa terlepas dari manipulasi ekstensi  
- [ ] Ya — kode sumber terungkap karena server **tidak** memiliki *handler* untuk ekstensi yang ditambahkan  

### Apakah direktori administratif atau metadata (misalnya, `.git/`, `.svn/`, `.DS_Store`) diblokir dari akses publik?
- [ ] Tidak — direktori/berkas ini **tidak** ada di *web root*  
- [ ] Ya — akses **tidak dimungkinkan** karena aturan *rewrite* di sisi server atau izin akses  
- [ ] Ya — direktori metadata **dapat** diakses dan memungkinkan rekonstruksi kode sumber secara lengkap  

### Bagaimana server menangani permintaan untuk berkas dengan banyak ekstensi (misalnya, `file.php.jpg`)?
- [ ] Tidak — server memprioritaskan keamanan ekstensi internal dengan benar atau memblokir permintaan tersebut  
- [ ] Ya — server memproses berkas sebagai ekstensi pertama, tetapi *bypass* **tidak dimungkinkan** untuk eksekusi  
- [ ] Ya — server mengabaikan ekstensi terakhir, sehingga eksekusi kode atau pengungkapan sumber **dimungkinkan**  

---