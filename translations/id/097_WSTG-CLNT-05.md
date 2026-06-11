## WSTG-CLNT-05 — Pengujian untuk CSS Injection

CSS Injection terjadi ketika sebuah aplikasi memungkinkan input yang disediakan pengguna untuk memengaruhi Cascading Style Sheets (CSS) dari sebuah halaman web tanpa sanitasi atau escaping yang tepat. Meskipun sering dianggap sebagai masalah kosmetik, penyerang dapat memanfaatkan CSS injection untuk mengeksfiltrasi data sensitif, seperti CSRF tokens atau session IDs, dengan menggunakan attribute selectors dan properti background-image untuk memicu permintaan eksternal. Kerentanan ini biasanya terjadi pada endpoint di mana preferensi pengguna (misalnya, tema kustom, warna font) direfleksikan di dalam blok `<style>` atau atribut `style` inline. Dari perspektif penyerang, eksploitasi yang berhasil dapat menyebabkan UI redressing, phishing melalui modifikasi tata letak yang tidak sah, atau eksfiltrasi data secara sembunyi-sembunyi di lingkungan di mana Content Security Policies yang ketat mungkin memblokir serangan berbasis JavaScript.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CLNT-05 |
| **CWE** | CWE-74 |
| **Status Pengujian** | Belum Dilakukan |
| **Severity** | Sedang / Tinggi* |

> *Severity menjadi Tinggi jika titik injeksi memungkinkan eksfiltrasi atribut sensitif seperti CSRF tokens atau jika dapat digunakan untuk UI redressing dalam alur kerja (workflow) yang sensitif.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/05-Testing_for_CSS_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite`, `CSS-Injection-Payload-Generator`, `CyberChef`, `Postman`

### Apakah aplikasi merefleksikan input yang dikontrol pengguna dalam konteks CSS?
- [ ] Tidak — input **tidak** direfleksikan dalam tag style, atribut, atau file CSS  
- [ ] Ya — input direfleksikan tetapi sangat terbatas pada nilai alfanumerik yang aman  
- [ ] Ya — input direfleksikan di dalam atribut `style` atau blok `<style>`  

### Apakah metakarakter dan fungsi spesifik CSS disanitasi dengan benar?
- [ ] Ya — karakter seperti `{`, `}`, `:`, dan fungsi seperti `url()` telah **di-escape dengan benar**  
- [ ] Ya — sanitasi sudah diterapkan tetapi bypass **dimungkinkan** melalui encoding atau komentar CSS  
- [ ] No — tidak ada sanitasi yang diterapkan pada metakarakter CSS  

### Apakah eksfiltrasi data mungkin dilakukan menggunakan CSS selectors?
- [ ] Tidak — selectors **tidak dapat** memicu permintaan eksternal atau membocorkan data  
- [ ] Ya — eksfiltrasi data parsial **dimungkinkan** melalui attribute selectors dan `background-image`  
- [ ] Ya — eksfiltrasi penuh token sensitif **dimungkinkan** menggunakan teknik Brute Force otomatis  

### Apakah Content Security Policy (CSP) memitigasi risiko CSS injection?
- [ ] Ya — `style-src` dibatasi ke 'self' dan `img-src` atau `connect-src` **dibatasi**  
- [ ] Ya — CSP ada tetapi menggunakan 'unsafe-inline' atau mengizinkan wildcard domain eksternal  
- [ ] Tidak — tidak ada CSP yang diimplementasikan untuk mencegah pemuatan sumber daya eksternal melalui CSS  

### Dapatkah kerentanan ini digunakan untuk melakukan UI redressing atau phishing?
- [ ] Tidak — cakupan injeksi terlalu terbatas untuk memodifikasi tata letak secara signifikan  
- [ ] Ya — modifikasi UI **dimungkinkan**, memungkinkan overlay elemen berbahaya  
- [ ] Ya — kontrol penuh tata letak halaman **dimungkinkan**, memfasilitasi serangan phishing yang sangat meyakinkan  

---