## WSTG-ATHN-10 — Pengujian Otentikasi yang Lebih Lemah pada Saluran Alternatif

Pengujian otentikasi yang lebih lemah pada saluran alternatif melibatkan identifikasi dan analisis jalur sekunder menuju akses akun—seperti API seluler, alur kerja pemulihan kata sandi, antarmuka help desk, atau subdomain legacy—yang mungkin tidak menerapkan ketegasan keamanan yang sama dengan portal web utama. Saluran alternatif ini seringkali tidak memiliki Multi-Factor Authentication (MFA) yang kuat, Rate Limiting yang ketat, atau persyaratan kata sandi yang kompleks, sehingga menciptakan "titik terlemah" yang merusak seluruh sistem otentikasi. Penyerang secara khusus menargetkan titik masuk yang terabaikan ini untuk melakukan Credential Stuffing atau melewati MFA dengan mengeksploitasi perbedaan dalam cara antarmuka yang berbeda menangani verifikasi identitas. Memastikan kesetaraan (parity) di semua permukaan otentikasi sangat penting untuk mencegah akses yang tidak sah dan menjaga integritas sesi serta data pengguna.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-ATHN-10 |
| **CWE** | CWE-287 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/10-Testing_for_Weaker_Authentication_in_Alternative_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite`, `Postman`, `cURL`, `MobSF`, `Ffuf`

### Apakah saluran otentikasi alternatif ditemukan dan diidentifikasi?
- [ ] Tidak — hanya terdapat satu saluran otentikasi  
- [ ] Ya — beberapa saluran teridentifikasi (misalnya, API aplikasi seluler, desktop client, portal legacy, layanan SOAP)  

### Apakah saluran alternatif menerapkan kesetaraan keamanan dengan saluran utama?
- [ ] Ya — semua saluran menerapkan persyaratan kompleksitas kata sandi dan MFA yang **identik**  
- [ ] Ya — saluran menerapkan kompleksitas kata sandi tetapi MFA **tidak diwajibkan** atau **dapat** dilewati (bypass)  
- [ ] Tidak — saluran alternatif memiliki persyaratan otentikasi yang **jauh lebih lemah**  

### Apakah rate limiting dan penguncian akun (account lockout) konsisten di seluruh saluran?
- [ ] Ya — Rate Limiting dan lockout **diterapkan secara konsisten** di semua endpoint  
- [ ] Ya — kontrol tersedia namun bypass **dimungkinkan** pada saluran tertentu (misalnya, API seluler)  
- [ ] Tidak — Rate Limiting atau lockout **tidak diterapkan** pada jalur otentikasi sekunder  

### Dapatkah MFA dilewati dengan beralih ke saluran alternatif?
- [ ] Tidak — MFA bersifat wajib dan **tidak dapat** dilewati terlepas dari titik masuknya  
- [ ] Ya — MFA diwajibkan pada portal web tetapi **tidak diterapkan** pada API seluler atau legacy  

### Apakah alur pemulihan kata sandi atau "lupa kata sandi" lebih lemah daripada login standar?
- [ ] Tidak — alur pemulihan menggunakan verifikasi out-of-band yang kuat dan tidak membocorkan informasi  
- [ ] Ya — alur pemulihan menggunakan pertanyaan keamanan yang lemah yang **dapat** ditebak atau diriset dengan mudah  
- [ ] Ya — alur pemulihan rentan terhadap Enumeration atau **tidak** memerlukan tingkat pembuktian yang sama dengan login standar  

---