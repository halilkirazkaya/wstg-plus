## WSTG-INPV-03 — Pengujian terhadap HTTP Verb Tampering

HTTP Verb Tampering mengeksploitasi kelemahan pada bagaimana server web dan framework aplikasi mengotorisasi akses ke sumber daya tertentu berdasarkan metode HTTP yang digunakan. Penyerang mencoba melewati batasan keamanan (security constraints) dengan mengganti metode standar seperti `GET` atau `POST` dengan alternatif seperti `HEAD`, `PUT`, `OPTIONS`, atau bahkan string arbitrer non-standar yang mungkin diproses secara tidak konsisten oleh backend. Kerentanan ini biasanya terjadi ketika konfigurasi keamanan—seperti filter Java EE web.xml atau aturan otorisasi .NET—mencantumkan metode yang diizinkan secara eksplisit tetapi gagal mempertimbangkan metode lainnya, atau ketika pendekatan "deny-by-method" digunakan alih-alih "deny-all." Eksploitasi yang berhasil dapat menghasilkan akses tidak sah ke fungsi administratif, modifikasi data, atau pengungkapan informasi mengenai konfigurasi internal server.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INPV-03 |
| **CWE** | CWE-288 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Sedang / Tinggi* |

> *Keparahan menjadi Tinggi jika fungsi administratif atau modifikasi data (PUT/DELETE) dapat dilakukan tanpa autentikasi.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/03-Testing_for_HTTP_Verb_Tampering  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite (Repeater/Intruder)`, `curl`, `nmap`, `ffuf`

### Apakah aplikasi merespons metode HTTP non-standar atau arbitrer?
- [ ] Tidak — aplikasi menolak semua metode kecuali yang secara eksplisit diperlukan  
- [ ] Ya — aplikasi merespons metode standar (OPTIONS, TRACE) tetapi **tidak dapat** melewati keamanan  
- [ ] Ya — aplikasi menerima string arbitrer (misalnya, FOO, TEST) dan memprosesnya sebagai permintaan `GET` atau `POST`  

### Apakah autentikasi/otorisasi dapat dilewati dengan mengubah metode HTTP?
- [ ] Tidak — kontrol keamanan diterapkan secara global tanpa memandang HTTP verb  
- [ ] Ya — kontrol sudah ada tetapi bypass **mungkin dilakukan** menggunakan metode `HEAD`  
- [ ] Ya — kontrol keamanan **tidak diterapkan** pada metode alternatif, memungkinkan akses tidak sah ke endpoint yang dibatasi  

### Apakah metode berbahaya seperti `PUT` atau `DELETE` diaktifkan pada server?
- [ ] Tidak — metode berbahaya **dinonaktifkan** atau mengembalikan 405 Method Not Allowed  
- [ ] Ya — metode diaktifkan tetapi memerlukan autentikasi tingkat tinggi yang valid  
- [ ] Ya — metode **diaktifkan** dan memungkinkan unggahan file atau penghapusan sumber daya yang tidak sah *(Kritis)*  

### Apakah metode `OPTIONS` mengungkapkan informasi sensitif tentang verb yang diizinkan?
- [ ] Tidak — `OPTIONS` **dinonaktifkan** atau mengembalikan respons generik  
- [ ] Ya — `OPTIONS` mengembalikan header `Allow` tetapi tidak mengekspos metode internal yang dibatasi  
- [ ] Ya — `OPTIONS` mengungkapkan metode internal saja atau administratif yang membantu dalam eksploitasi lebih lanjut  

### Apakah metode `TRACE` diaktifkan, berpotensi memungkinkan Cross-Site Tracing (XST)?
- [ ] Tidak — metode `TRACE` dan `TRACK` **dinonaktifkan**  
- [ ] Ya — `TRACE` **diaktifkan** dan memantulkan kembali header HTTP, termasuk cookie/token sensitif  

---