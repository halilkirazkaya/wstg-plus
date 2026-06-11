## WSTG-ATHN-05 — Testing for Vulnerable Remember Password

Fungsionalitas "Remember Me" atau login persisten dirancang untuk mempertahankan status autentikasi pengguna meskipun peramban (browser) dimulai ulang dengan cara menyimpan token atau kredensial pada sisi klien (client-side). Jika fitur ini diimplementasikan dengan buruk—seperti menyimpan kata sandi dalam teks biasa (plaintext), hash yang lemah, atau token dengan entropi rendah dalam cookie—hal ini akan mengekspos pengguna terhadap pengambilalihan akun (account takeover) melalui Cross-Site Scripting (XSS), akses fisik, atau intersepsi jaringan. Pentester mengevaluasi entropi dari token persisten ini, flag keamanan yang diterapkan pada mekanisme penyimpanan, dan siklus hidup sesi (session lifecycle) untuk memastikan bahwa kenyamanan akses persisten tidak melewati kontrol keamanan kritis seperti Multi-Factor Authentication (MFA) atau pembatalan validitas sesi (session invalidation). Eksploitasi sering kali melibatkan pemanenan token yang berumur panjang ini untuk mendapatkan akses tidak sah ke akun tanpa memerlukan kredensial asli.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-ATHN-05 |
| **CWE** | CWE-522 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan (Severity)** | Sedang / Tinggi* |

> *Keparahan menjadi Tinggi jika kredensial teks biasa (plaintext) atau hash tanpa salt disimpan dalam cookie persisten, atau jika token melewati Multi-Factor Authentication (MFA).

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/05-Testing_for_Vulnerable_Remember_Password  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  
* https://portswigger.net/web-security/authentication  

**Alat:** `Burp Suite (Cookie Editor/Sequencer)`, `Browser Developer Tools`, `EditThisCookie`

### Apakah aplikasi mengimplementasikan fitur "Remember Me" atau "Tetap Masuk"?
- [ ] Tidak — fitur **tidak** ada di dalam aplikasi  
- [ ] Ya — fitur ada dan menggunakan token yang kuat secara kriptografis dan tidak dapat diprediksi  
- [ ] Ya — fitur ada tetapi menggunakan token yang dapat diprediksi atau memiliki entropi rendah *(Sedang)*  
- [ ] Ya — fitur ada dan menyimpan kredensial dalam teks biasa (plaintext) atau kata sandi terenkode base64 *(Tinggi)*  

### Apakah flag keamanan diterapkan dengan benar pada cookie persisten?
- [ ] Ya — flag `HttpOnly`, `Secure`, dan `SameSite` telah **diterapkan**  
- [ ] Ya — beberapa flag diterapkan tetapi `HttpOnly` **tidak ada**  
- [ ] Ya — beberapa flag diterapkan tetapi `Secure` **tidak ada** pada koneksi HTTPS  
- [ ] Tidak — tidak ada flag keamanan yang **diterapkan** pada cookie persisten  

### Apakah token "Remember Me" tetap valid setelah logout manual?
- [ ] Tidak — token segera dibatalkan validitasnya di sisi server (server-side) setelah logout  
- [ ] Ya — token tetap valid tetapi memerlukan kata sandi untuk tindakan sensitif  
- [ ] Ya — token tetap valid dan memungkinkan akses akun penuh **tanpa** autentikasi ulang *(Sedang)*  

### Apakah sesi persisten dihentikan setelah dilakukan perubahan kata sandi?
- [ ] Ya — semua token persisten dicabut (revoked) ketika pengguna mengubah kata sandi mereka  
- [ ] Tidak — token "Remember Me" tetap valid setelah pengaturan ulang/perubahan kata sandi **dilakukan** *(Tinggi)*  

### Apakah token persisten melewati Multi-Factor Authentication (MFA) pada kunjungan berikutnya?
- [ ] Tidak — MFA tetap diperlukan meskipun terdapat token "Remember Me"  
- [ ] Ya — MFA dilewati, tetapi token terikat pada sidik jari perangkat (device fingerprint) tertentu  
- [ ] Ya — MFA **sepenuhnya dilewati** hanya dengan menggunakan cookie persisten dari perangkat apa pun *(Tinggi)*  

---