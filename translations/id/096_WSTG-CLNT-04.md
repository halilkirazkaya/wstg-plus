## WSTG-CLNT-04 — Pengujian Client-Side URL Redirect

Client-side URL redirection terjadi ketika aplikasi web menerima input yang dikontrol pengguna—biasanya melalui parameter URL atau fragmen hash—dan menggunakannya di dalam sink pengalihan sisi klien seperti `window.location` atau `document.location.href`. Kerentanan ini terutama dimanfaatkan oleh penyerang untuk melakukan kampanye phishing yang canggih, karena korban awalnya mempercayai domain yang sah sebelum dialihkan secara diam-diam ke situs berbahaya. Selain phishing, client-side redirect sering kali dapat dirantai untuk mengeksfiltrasi data sensitif seperti access token OAuth atau session identifier jika pengalihan terjadi saat nilai-nilai tersebut ada di dalam URL atau header `Referer`. Dalam Single Page Applications (SPA) modern, kerentanan ini sering muncul dalam logika routing, parameter "return to" setelah autentikasi, atau fungsionalitas pengalihan bahasa.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CLNT-04 |
| **CWE** | CWE-601 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Sedang* |

> *Tingkat keparahan menjadi Tinggi jika pengalihan dapat digunakan untuk mengeksfiltrasi token OAuth, session ID, atau melewati kontrol keamanan dalam alur autentikasi.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/04-Testing_for_Client-side_URL_Redirect  
* https://hacktricks.wiki/en/pentesting-web/open-redirect.html  

**Alat:** `Burp Suite (DOM Invader)`, `Browser DevTools`, `LinkFinder`, `Arjun`, `B-Config`

### Apakah aplikasi menggunakan skrip sisi klien untuk menangani pengalihan berdasarkan input pengguna?
- [ ] Tidak — pengalihan ditangani secara eksklusif di sisi server melalui kode status HTTP `3xx`  
- [ ] Ya — aplikasi menggunakan sink JavaScript seperti `window.location` untuk menavigasi berdasarkan parameter URL  
- [ ] Ya — aplikasi menggunakan router framework sisi klien (misalnya, React Router, Vue Router) yang memproses jalur (path) yang diberikan pengguna  

### Apakah validasi input atau kontrol whitelisting diimplementasikan untuk URL tujuan?
- [ ] Ya — **whitelist** ketat untuk domain/jalur yang diizinkan diterapkan dan bypass **tidak dimungkinkan**  
- [ ] Ya — validasi ada tetapi mengandalkan regex yang lemah atau blacklist, dan bypass **dimungkinkan**  
- [ ] Tidak — aplikasi menerima string arbitrer apa pun sebagai target pengalihan  

### Dapatkah logika pengalihan dilewati menggunakan teknik obfuskasi umum?
- [ ] Tidak — kontrol menangani karakter terenkode, protocol-relative URLs (`//`), dan backslash dengan benar  
- [ ] Ya — bypass **dimungkinkan** menggunakan URL encoding atau double-encoding  
- [ ] Ya — bypass **dimungkinkan** menggunakan protocol-relative URLs atau karakter `@` (misalnya, `https://expected.com@attacker.com`)  
- [ ] Ya — bypass **dimungkinkan** menggunakan browser quirks tertentu atau null byte injection  

### Apakah proses pengalihan mengekspos data sensitif ke situs tujuan?
- [ ] Tidak — tidak ada data sensitif yang ada di URL atau dikirimkan melalui header selama pengalihan  
- [ ] Ya — data sensitif (misalnya, session tokens, API keys) **bocor** melalui header `Referer` ke situs eksternal  
- [ ] Ya — data sensitif yang ada dalam fragmen URL (`#`) **dapat diakses** oleh skrip situs tujuan  

### Apakah mungkin untuk mengeksekusi JavaScript melalui sink pengalihan (DOM-based XSS)?
- [ ] Tidak — sink hanya mengizinkan navigasi ke protokol `http` atau `https`  
- [ ] Ya — sink pengalihan mengizinkan URI `javascript:` atau `data:`, sehingga memungkinkan terjadinya **DOM-based XSS**  

---