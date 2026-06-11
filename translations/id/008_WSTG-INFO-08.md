## WSTG-INFO-08 — Fingerprint Web Application Framework

Fingerprinting framework aplikasi web melibatkan identifikasi tumpukan teknologi (technology stack) spesifik dan versi perangkat lunak yang digunakan untuk membangun dan menyajikan aplikasi. Penyerang menganalisis HTTP response headers, cookie khusus framework, struktur file yang dapat diprediksi, dan artefak JavaScript unik untuk menentukan apakah target menjalankan platform seperti React, Angular, Django, atau Spring. Fase reconnaissance ini sangat penting karena memungkinkan penguji untuk mempersempit attack surface ke kerentanan yang diketahui (CVE) dan kelemahan konfigurasi umum yang spesifik untuk framework tersebut. Dengan memahami arsitektur yang mendasarinya, penyerang dapat beralih dari pengujian generik ke strategi eksploitasi yang sangat bertarget.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INFO-08 |
| **CWE** | CWE-200 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Informasional |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/08-Fingerprint_Web_Application_Framework  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Wappalyzer`, `BuiltWith`, `WhatWeb`, `nmap`, `Burp Suite`, `curl`

### Apakah HTTP response headers mengekspos nama atau versi framework?
- [ ] Tidak — header seperti `X-Powered-By` atau `Server` **tidak** ada atau telah disanitasi  
- [ ] Ya — nama framework ada tetapi versi **disembunyikan**  
- [ ] Ya — nama framework dan versi **keduanya terekspos**  

### Apakah cookie khusus framework atau struktur direktori dapat diidentifikasi?
- [ ] Tidak — artefak framework umum (misalnya, path `.js`, nama cookie) diobfuskasi atau diubah namanya  
- [ ] Ya — cookie khusus framework (misalnya, `JSESSIONID`, `PHPSESSID`, `csrftoken`) **dapat** diidentifikasi  
- [ ] Ya — struktur direktori (misalnya, `/wp-content/`, `_next/static/`) **terlihat** dan mengonfirmasi framework tersebut  

### Apakah pesan kesalahan atau komentar kode sumber mengungkapkan detail framework?
- [ ] Tidak — penanganan kesalahan (error handling) bersifat generik dan komentar dihapus  
- [ ] Ya — stack trace khusus framework **diaktifkan** dan terlihat dalam respons  
- [ ] Ya — kode sumber HTML berisi komentar atau tag metadata yang mengidentifikasi framework  

### Apakah ada kerentanan yang diketahui terkait dengan versi framework yang diidentifikasi?
- [ ] Tidak — framework yang diidentifikasi sudah mutakhir dengan **tidak ada** kerentanan publik yang diketahui  
- [ ] Ya — versi framework sudah usang dan CVE tertentu **dapat** diidentifikasi  

---