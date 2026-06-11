## WSTG-ATHN-01 — Pengujian Kredensial yang Dikirim melalui Saluran Terenkripsi

Pengujian kredensial yang dikirim melalui saluran terenkripsi memastikan bahwa data autentikasi sensitif, seperti username, password, dan session token, terlindungi dari intersepsi selama transit. Penyerang yang berada di jaringan—misalnya melalui serangan Man-in-the-Middle (MitM) pada Wi-Fi publik atau jaringan internal yang terkompromi—dapat menggunakan alat packet sniffing untuk menangkap kredensial cleartext jika TLS/SSL tidak diterapkan dengan benar. Kerentanan ini biasanya terjadi pada endpoint login, formulir reset password, dan header autentikasi API di mana aplikasi gagal mewajibkan HTTPS atau melakukan pengalihan (redirect) yang tidak tepat dari HTTP. Eksploitasi yang sukses memberikan penyerang akses penuh ke akun pengguna, yang menyebabkan kompromi total pada data pengguna dan potensi lateral movement di dalam aplikasi.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-ATHN-01 |
| **CWE** | CWE-319 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/01-Testing_for_Credentials_Transported_over_an_Encrypted_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Alat:** `Wireshark`, `tcpdump`, `Burp Suite`, `OWASP ZAP`, `testssl.sh`, `sslyze`, `nmap`

### Apakah HTTPS diwajibkan untuk seluruh proses autentikasi?
- [ ] Ya — HSTS **diaktifkan** dan semua trafik dipaksa secara ketat melalui HTTPS  
- [ ] Ya — pengalihan (redirection) ke HTTPS terjadi, tetapi HSTS **tidak diaktifkan**  
- [ ] Tidak — kredensial **dapat** dikirimkan melalui HTTP yang tidak terenkripsi  

### Apakah halaman login dan formulir disajikan melalui saluran terenkripsi?
- [ ] Ya — halaman yang berisi formulir login dikirimkan melalui HTTPS  
- [ ] Tidak — halaman login disajikan melalui HTTP, memungkinkan pembajakan form-action atau sniffing kredensial  

### Apakah `Secure` flag diterapkan pada semua cookie sesi yang sensitif?
- [ ] Ya — `Secure` flag **diterapkan** pada semua cookie autentikasi dan sesi  
- [ ] Ya — `Secure` flag **diterapkan** hanya pada beberapa cookie saja  
- [ ] Tidak — `Secure` flag **tidak diterapkan**, memungkinkan cookie untuk dieksfiltrasi melalui permintaan yang tidak terenkripsi  

### Apakah server mendukung versi TLS yang lemah atau cipher suites yang tidak aman?
- [ ] Tidak — hanya TLS modern (1.2+) dan cipher yang kuat yang **diaktifkan**  
- [ ] Ya — versi lama (TLS 1.0/1.1) atau cipher yang lemah (RC4, DES, CBC) masih **didukung**  

### Apakah aplikasi mencegah transmisi kredensial ke domain pihak ketiga melalui saluran yang tidak terenkripsi?
- [ ] Ya — tidak ada data sensitif yang dikirim ke domain eksternal atau semua panggilan eksternal dipaksa melalui HTTPS  
- [ ] Tidak — header autentikasi atau kredensial dikirim ke endpoint pihak ketiga (misalnya, analitik atau SSO) melalui HTTP  

---