## WSTG-SESS-01 — Testing for Session Management Schema

Pengujian skema manajemen sesi berfokus pada analisis bagaimana aplikasi menangani siklus hidup dan struktur pengidentifikasi sesi untuk memastikan ketahanannya terhadap prediksi dan pembajakan (hijacking). Penyerang menangkap beberapa token sesi untuk melakukan analisis statistik, mencari pola, entropi rendah, atau kenaikan yang dapat diprediksi yang memungkinkan mereka memalsukan sesi yang valid bagi pengguna lain. Kelemahan dalam skema ini sering kali muncul sebagai kerentanan Session Fixation atau penggunaan nilai yang kurang acak, biasanya ditemukan dalam cookie HTTP, parameter URL, atau implementasi header khusus. Mengompromikan skema sesi adalah serangan berdampak tinggi yang memberikan akses penuh kepada lawan ke status autentikasi korban tanpa memerlukan kredensial utama mereka.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-SESS-01 |
| **CWE** | CWE-330 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/01-Testing_for_Session_Management_Schema  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Alat:** `Burp Suite (Sequencer)`, `OWASP ZAP`, `Python (pandas/numpy for entropy analysis)`, `Cookie Editor`

### Apakah pengidentifikasi sesi (session identifier) sudah cukup kompleks dan acak?
- [ ] Ya — token lolos uji keacakan statistik (entropi tinggi) dan bypass **tidak dimungkinkan**  
- [ ] Ya — token panjang tetapi berisi pola yang dapat diprediksi atau segmen statis  
- [ ] Tidak — token bersifat sekuensial, berdasarkan timestamp yang dapat diprediksi, atau memiliki entropi yang **sangat rendah** *(Kritis)*  

### Di mana pengidentifikasi sesi ditransmisikan dan disimpan?
- [ ] Pengidentifikasi disimpan dalam cookie `Secure` dan `HttpOnly` *(Paling aman)*  
- [ ] Pengidentifikasi disimpan dalam cookie tetapi tidak memiliki flag `Secure` atau `HttpOnly`  
- [ ] Pengidentifikasi ditransmisikan **hanya** melalui parameter URL atau permintaan `GET` *(Risiko Tinggi)*  

### Apakah aplikasi menerbitkan pengidentifikasi sesi baru setelah autentikasi berhasil?
- [ ] Ya — ID sesi baru yang unik **dibuat** segera setelah login  
- [ ] Tidak — ID sesi tetap sama sebelum dan sesudah login *(Memungkinkan terjadinya Session Fixation)*  

### Apakah pengidentifikasi sesi terikat pada alamat IP atau User-Agent tertentu untuk mencegah penggunaan kembali?
- [ ] Ya — sesi dibatalkan jika sidik jari (fingerprint) klien berubah secara signifikan  
- [ ] Tidak — sesi **dapat** digunakan kembali di berbagai alamat IP atau perangkat tanpa verifikasi tambahan  

### Apakah pengidentifikasi sesi dibatalkan dengan benar di sisi server setelah logout atau timeout?
- [ ] Ya — status sesi di sisi server **sepenuhnya dihancurkan** saat logout  
- [ ] Tidak — sesi tetap valid di server meskipun cookie di sisi klien dihapus  
- [ ] Tidak — sesi **tidak pernah kedaluwarsa** atau memiliki periode timeout yang sangat lama  

---