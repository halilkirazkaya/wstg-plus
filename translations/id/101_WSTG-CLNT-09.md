## WSTG-CLNT-09 — Testing for Clickjacking

Clickjacking, juga dikenal sebagai UI redressing, terjadi ketika penyerang menggunakan lapisan transparan atau buram untuk mengelabui pengguna agar mengeklik tombol atau tautan pada halaman lain padahal mereka bermaksud mengeklik halaman tingkat atas. Kerentanan ini mengompromikan integritas tindakan pengguna, yang berpotensi menyebabkan modifikasi data yang tidak sah, perubahan akun, atau transfer keuangan yang dilakukan atas nama korban. Penyerang biasanya mengeksploitasi hal ini dengan menanamkan (embedding) situs web target di dalam `<iframe>` pada situs berbahaya yang dikendalikan dan melapisinya dengan konten yang menipu atau umpan yang menarik. Hal ini paling sering terjadi pada halaman yang melakukan tindakan perubahan status (state-changing actions) yang sensitif seperti tombol "Hapus Akun", formulir pengaturan ulang kata sandi, atau panel konfigurasi administratif.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CLNT-09 |
| **CWE** | CWE-1021 |
| **Status Pengujian** | Tidak Dilakukan |
| **Tingkat Keparahan** | Sedang / Tinggi* |

> *Tingkat keparahan menjadi Tinggi jika tindakan sensitif (misalnya, transfer dana, penghapusan akun, atau Eskalasi Hak Istimewa (Privilege Escalation)) dapat dipicu melalui clickjacking.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/09-Testing_for_Clickjacking  
* https://hacktricks.wiki/en/pentesting-web/clickjacking.html  
* https://portswigger.net/web-security/clickjacking  

**Alat:** `Burp Suite Clickbandit`, `Browser Developer Tools`, `OWASP ZAP`, `Online Clickjack Test`

### Apakah aplikasi menerapkan header sisi server untuk mencegah framing yang tidak sah?
- [ ] Ya — `X-Frame-Options` atau `Content-Security-Policy` **diterapkan** dan mencegah framing *(Paling aman)*  
- [ ] Ya — header ada tetapi **salah dikonfigurasi** (misalnya, `X-Frame-Options: ALLOW-FROM` yang kurang mendapat dukungan browser secara luas)  
- [ ] No — tidak ada header perlindungan framing yang **diterapkan**  

### Apakah direktif `frame-ancestors` pada `Content-Security-Policy` (CSP) diimplementasikan?
- [ ] Ya — `frame-ancestors 'none'` atau `'self'` **diterapkan**  
- [ ] Ya — `frame-ancestors` ada tetapi **mengizinkan** asal (origin) yang tidak tepercaya atau terlalu luas  
- [ ] No — direktif **hilang** atau CSP tidak diimplementasikan  

### Apakah header `X-Frame-Options` (XFO) dikonfigurasi dengan benar untuk dukungan browser lama (legacy)?
- [ ] Ya — `DENY` atau `SAMEORIGIN` **diterapkan**  
- [ ] Ya — XFO ada tetapi **diabaikan** oleh browser modern karena adanya direktif `frame-ancestors` pada CSP  
- [ ] No — header XFO **hilang** atau disetel ke nilai yang tidak aman  

### Bisakah perlindungan framing dilewati (bypass) menggunakan teknik yang diketahui?
- [ ] No — bypass **tidak dimungkinkan** melalui teknik standar (double-framing, dll.)  
- [ ] Ya — perlindungan **dapat** dilewati karena regex atau logika yang lemah pada daftar putih (whitelist) `frame-ancestors`  
- [ ] Ya — perlindungan **dapat** dilewati karena aplikasi hanya mengandalkan frame-busting berbasis JavaScript  

### Apakah tindakan sensitif dapat dieksploitasi melalui proof-of-concept (PoC) clickjacking?
- [ ] No — framing diblokir atau tidak ada tindakan sensitif yang dapat dijangkau  
- [ ] Ya — UI redressing **dimungkinkan** tetapi membutuhkan interaksi pengguna yang kompleks  
- [ ] Ya — UI redressing **dimungkinkan** dan memungkinkan tindakan perubahan status (state-changing) secara langsung *(Kritis)*  

---