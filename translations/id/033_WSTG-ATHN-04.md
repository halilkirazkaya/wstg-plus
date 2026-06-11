## WSTG-ATHN-04 — Testing for Bypassing Authentication Schema

Melewati skema autentikasi (Bypassing authentication schema) melibatkan identifikasi dan eksploitasi celah dalam logika aplikasi yang memungkinkan penyerang mendapatkan akses ke sumber daya yang dilindungi tanpa memberikan kredensial yang valid. Kerentanan ini biasanya terjadi ketika aplikasi mengandalkan kontrol sisi klien (client-side controls), gagal menegakkan otorisasi sisi server (server-side authorization) pada setiap permintaan, atau membiarkan "backdoor" administratif dan endpoint debug terekspos di lingkungan produksi. Penyerang mengeksploitasi kelemahan ini dengan melakukan forced browsing ke URL sensitif, memanipulasi parameter HTTP seperti `authenticated=true`, atau melakukan spoofing header untuk menipu aplikasi agar menganggap sesi telah terbentuk. Eksploitasi yang berhasil mengakibatkan runtuhnya perimeter autentikasi secara total, yang berpotensi menyebabkan eksfiltrasi data yang tidak sah atau kompromi sistem secara penuh.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-ATHN-04 |
| **CWE** | CWE-287 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Kritis |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/04-Testing_for_Bypassing_Authentication_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Alat:** `Burp Suite`, `Ffuf`, `Gobuster`, `Dirsearch`, `Postman`

### Apakah halaman internal yang sensitif dapat diakses langsung tanpa sesi aktif?
- [ ] Tidak — akses langsung menghasilkan pengalihan ke login atau respons 403/404  
- [ ] Ya — beberapa halaman sensitif dapat diakses tetapi menyediakan data yang terbatas atau tidak sensitif  
- [ ] Ya — halaman administratif sensitif atau halaman khusus pengguna **dapat** diakses sepenuhnya tanpa autentikasi *(Kritis)*  

### Apakah aplikasi mengandalkan flag atau parameter sisi klien untuk menentukan status autentikasi?
- [ ] Tidak — status autentikasi dikelola secara ketat melalui status sesi sisi server  
- [ ] Ya — flag sisi klien ada tetapi modifikasi **tidak** memberikan akses ke area yang dilindungi  
- [ ] Ya — memodifikasi parameter (contoh: `is_authenticated=true`, `user_role=admin`) **memungkinkan** bypass autentikasi  

### Apakah autentikasi dapat dilewati dengan melakukan spoofing atau manipulasi header HTTP tertentu?
- [ ] Tidak — header seperti `X-Forwarded-For`, `Referer`, atau header khusus tidak berdampak pada autentikasi  
- [ ] Ya — memanipulasi header (contoh: `X-Custom-IP-Authorization`, `X-Remote-User`) **mungkin dilakukan** dan memberikan akses yang tidak sah  

### Apakah terdapat jalur alternatif atau artefak pengembangan yang menghindari alur login standar?
- [ ] Tidak — semua sumber daya yang dilindungi dijaga oleh pemeriksaan autentikasi terpusat yang siap produksi  
- [ ] Ya — endpoint lama (legacy), rute debug (contoh: `/debug/login`), atau versi API yang terlupakan **dapat** diakses tanpa kredensial  

### Apakah aplikasi gagal melakukan autentikasi ulang atau memverifikasi sesi untuk tindakan dengan hak istimewa tinggi?
- [ ] Tidak — tindakan dengan hak istimewa tinggi atau sensitif memerlukan token sesi yang baru atau valid  
- [ ] Ya — setelah bypass ditemukan pada satu endpoint, hal itu **dapat** digunakan untuk melakukan tindakan administratif di seluruh aplikasi  

---