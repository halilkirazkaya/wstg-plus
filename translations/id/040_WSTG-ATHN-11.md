## WSTG-ATHN-11 — Testing Multi-Factor Authentication (MFA)

Pengujian Multi-Factor Authentication (MFA) mengevaluasi ketangguhan lapisan keamanan sekunder yang dirancang untuk mencegah akses yang tidak sah bahkan ketika kredensial utama telah terkompromi. Penyerang sering kali mencoba melewati MFA dengan mengidentifikasi endpoint di mana penerapannya tidak konsisten, seperti versi API lama (legacy), backend seluler, atau alur kerja pengaturan ulang kata sandi (password reset workflows). Eksploitasi sering kali melibatkan manipulasi respons server, melakukan brute-force pada kode yang berumur pendek (OTP), atau memanfaatkan celah kondisi balapan (race conditions) dan manajemen sesi yang memungkinkan pengguna bertransisi ke status terautentikasi tanpa memberikan faktor kedua.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-ATHN-11 |
| **CWE** | CWE-287 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Kerentanan** | Tinggi / Kritis |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/11-Testing_Multi-Factor_Authentication  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite (Intruder/Repeater/Turbo Intruder)`, `Postman`, `Mitproxy`

### Apakah MFA diterapkan secara konsisten di seluruh portal autentikasi?
- [ ] Ya — MFA diwajibkan untuk semua upaya masuk (login) berbasis web, seluler, dan API  
- [ ] Ya — tetapi MFA **tidak diterapkan** pada endpoint tertentu (misalnya, `/api/v1/login` lama atau portal khusus seluler)  
- [ ] Tidak — MFA **tidak diimplementasikan** dalam aplikasi *(Kritis)*  

### Dapatkah langkah verifikasi MFA dilewati melalui navigasi URL langsung atau manipulasi respons?
- [ ] Tidak — navigasi langsung atau manipulasi parameter **tidak dapat** melewati tantangan (challenge) tersebut  
- [ ] Ya — menavigasi langsung ke URL dasbor internal **dimungkinkan** tanpa menyelesaikan tantangan MFA  
- [ ] Ya — memanipulasi respons server (misalnya, mengubah `401 Unauthorized` menjadi `200 OK`) **dimungkinkan** untuk mendapatkan akses  

### Apakah proses verifikasi kode MFA dilindungi dari serangan brute-force?
- [ ] Ya — pembatasan laju (Rate Limiting) yang ketat atau penguncian akun (account lockout) **diterapkan** setelah beberapa upaya OTP yang gagal  
- [ ] Ya — pembatasan laju ada tetapi **dimungkinkan** untuk dilewati melalui rotasi IP atau manipulasi header  
- [ ] Tidak — tidak ada pembatasan laju yang diterapkan, dan brute-force kode secara otomatis **dimungkinkan**  

### Apakah aplikasi mempertahankan status sesi yang aman selama transisi MFA?
- [ ] Ya — token sesi dengan hak istimewa tinggi dikeluarkan **hanya setelah** penyelesaian MFA yang berhasil  
- [ ] Tidak — cookie sesi yang berfungsi penuh dikeluarkan **sebelum** MFA selesai, memungkinkan akses ke beberapa fitur  
- [ ] Tidak — aplikasi menggunakan pengidentifikasi statis atau transisi sesi yang dapat diprediksi sehingga **dapat** dibajak  

### Apakah faktor MFA alternatif (SMS, Email, Kode Cadangan) rentan terhadap eksploitasi?
- [ ] Tidak — semua faktor menggunakan nilai yang aman, tidak dapat diprediksi, dan terbatas waktu  
- [ ] Ya — kode cadangan (backup codes) dapat diprediksi atau **dapat** dienumerasi  
- [ ] Ya — fungsionalitas "Kirim Ulang Kode" dapat disalahgunakan untuk melakukan SMS/Email flooding atau mengungkapkan detail kontak parsial  

---