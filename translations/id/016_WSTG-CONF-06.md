## WSTG-CONF-06 — Test HTTP Methods

Pengujian metode HTTP melibatkan identifikasi kata kerja (verbs) mana yang didukung oleh server web dan aplikasi untuk memastikan hanya fungsionalitas yang diperlukan yang diekspos. Selain `GET` dan `POST` standar, server mungkin secara tidak sengaja mengaktifkan metode berbahaya seperti `PUT`, `DELETE`, atau `TRACE`, yang dapat memungkinkan penyerang untuk mengunggah file berbahaya, menghapus konten yang ada, atau mengeksfiltrasi cookie sesi melalui Cross-Site Tracing (XST). Pentester memeriksa header respons seperti `Allow` atau `Public` dan mencoba melewati batasan menggunakan teknik method overriding atau verb tampering untuk mengakses fungsionalitas yang tidak sah. Pengujian ini sangat penting untuk memperkuat (hardening) permukaan serangan dan mencegah kesalahan konfigurasi (misconfiguration) yang dapat menyebabkan kompromi server atau manipulasi data yang tidak sah.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CONF-06 |
| **CWE** | CWE-650 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Rendah / Sedang* |

> *Tingkat keparahan menjadi Tinggi jika `PUT` atau `DELETE` memungkinkan manipulasi file tanpa izin atau jika `TRACE` memfasilitasi eksfiltrasi token sesi.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/06-Test_HTTP_Methods  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `curl`, `nmap`, `Burp Suite (Repeater)`, `ZAP`, `Metasploit`

### Apakah metode HTTP berbahaya seperti `PUT` atau `DELETE` dinonaktifkan di seluruh aplikasi?
- [ ] Ya — hanya metode aman (GET, POST, HEAD) yang **diaktifkan**  
- [ ] Tidak — `PUT` atau `DELETE` **diaktifkan** tetapi memerlukan autentikasi yang valid  
- [ ] Tidak — `PUT` atau `DELETE` **diaktifkan** dan dapat diakses tanpa autentikasi *(Kritis)*  

### Apakah metode `TRACE` dinonaktifkan untuk mencegah Cross-Site Tracing (XST)?
- [ ] Ya — metode `TRACE` **dinonaktifkan** atau mengembalikan 405 Method Not Allowed  
- [ ] Tidak — metode `TRACE` **diaktifkan** tetapi tidak merefleksikan header sensitif  
- [ ] Tidak — metode `TRACE` **diaktifkan** dan merefleksikan header `Cookie` atau `Authorization` *(Sedang)*  

### Apakah HTTP Method Overriding (misalnya, `X-HTTP-Method-Override`) didukung oleh server?
- [ ] Tidak — server **tidak** memproses header method override  
- [ ] Ya — server memproses header override tetapi kontrol akses (access control) **tetap diterapkan**  
- [ ] Ya — server memproses header override dan bypass kontrol akses **mungkin terjadi**  

### Apakah server merespons dengan aman terhadap metode HTTP yang arbitrer atau cacat (malformed)?
- [ ] Ya — server mengembalikan 405 Method Not Allowed atau 501 Not Implemented  
- [ ] Tidak — server mengembalikan 200 OK atau 500 Error untuk metode arbitrer seperti `JEFF` atau `TEST`  
- [ ] Tidak — menggunakan metode arbitrer memungkinkan bypass filter autentikasi atau otorisasi  

### Apakah metode `OPTIONS` dibatasi atau dikonfigurasi untuk meminimalkan pengungkapan informasi (information disclosure)?
- [ ] Ya — `OPTIONS` dinonaktifkan atau mengembalikan header `Allow` yang minimal  
- [ ] Tidak — `OPTIONS` mengungkapkan berbagai macam metode yang diaktifkan, termasuk kata kerja DAV atau debugging  

---