## WSTG-SESS-02 — Pengujian Atribut Cookie

Manajemen sesi sering kali mengandalkan cookie HTTP untuk mempertahankan status (state), sehingga atribut keamanan dari cookie ini menjadi target utama bagi penyerang. Dengan mengabaikan atau salah mengonfigurasi atribut seperti `HttpOnly`, `Secure`, dan `SameSite`, aplikasi memaparkan token sesi terhadap pencurian melalui Cross-Site Scripting (XSS), intersepsi melalui saluran yang tidak terenkripsi, atau serangan Cross-Site Request Forgery (CSRF). Pentester memeriksa flag-flag ini untuk menentukan apakah penyerang dapat mengeksfiltrasi pengidentifikasi sesi dari Document Object Model (DOM) browser atau memaksa browser untuk mengirimkannya dalam permintaan lintas asal (cross-origin requests). Konfigurasi atribut yang tepat adalah langkah pertahanan berlapis (defense-in-depth) yang mendasar untuk memastikan bahwa token sensitif tetap terbatas pada konteks yang aman dan berwenang.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-SESS-02 |
| **CWE** | CWE-614 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Sedang |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/02-Testing_for_Cookies_Attributes  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Alat:** `Burp Suite (Proxy/Scanner)`, `OWASP ZAP`, `Browser Developer Tools (Storage Tab)`, `EditThisCookie`, `curl`

### Analisis Implementasi Flag Cookie
| Atribut | Ada | Tidak Ada |
|-----------|:-------:|:-------:|
| `HttpOnly` |  |  |
| `Secure` |  |  |
| `SameSite` |  |  |

### Apakah flag `HttpOnly` diterapkan pada cookie sesi yang sensitif?
- [ ] Ya — diterapkan pada **semua** cookie sesi sensitif *(Paling aman)*  
- [ ] Ya — hanya diterapkan pada beberapa cookie sesi  
- [ ] No — flag **tidak diterapkan**, memungkinkan akses melalui skrip sisi klien (client-side scripts) *(Kritis)*  

### Apakah flag `Secure` dipaksakan untuk cookie yang dikirimkan melalui HTTPS?
- [ ] Ya — diterapkan pada **semua** cookie yang dikirimkan melalui TLS  
- [ ] No — flag **tidak diterapkan**, memungkinkan cookie dikirimkan melalui HTTP teks biasa (plaintext)  

### Apakah atribut `SameSite` digunakan untuk memitigasi risiko CSRF?
- [ ] Ya — diatur ke `Strict` atau `Lax` untuk cookie sesi  
- [ ] Ya — diatur ke `None` tetapi dengan flag `Secure` yang ada  
- [ ] No — atribut **tidak ada** atau diatur ke `None` **tanpa** `Secure`  

### Apakah atribut `Domain` dan `Path` dibatasi pada cakupan minimum yang diperlukan?
- [ ] Ya — dicakupkan ke subdomain dan jalur aplikasi yang **spesifik**  
- [ ] No — cakupan terlalu **luas** (misalnya, diatur ke domain induk atau root `/`), meningkatkan permukaan serangan (attack surface)  

### Apakah cookie sesi yang sensitif kedaluwarsa dengan benar melalui atribut `Expires` atau `Max-Age`?
- [ ] Ya — tanggal kedaluwarsa yang sesuai telah ditetapkan  
- [ ] No — cookie bersifat persisten dengan masa pakai yang terlalu **lama**  
- [ ] No — cookie hanya untuk sesi tetapi **kurang** dalam penegakan batas waktu (timeout) di sisi server  

---