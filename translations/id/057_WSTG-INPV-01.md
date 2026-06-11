## WSTG-INPV-01 — Reflected Cross Site Scripting (XSS)

Reflected Cross Site Scripting (XSS) terjadi ketika sebuah aplikasi menyertakan data yang tidak terpercaya dalam respons HTTP tanpa validasi atau encoding yang memadai, sehingga menyebabkan payload dieksekusi dalam konteks browser korban. Penyerang mengirimkan payload berbahaya melalui social engineering, biasanya melalui URL yang dimanipulasi atau formulir, untuk mengambil alih sesi pengguna, mengeksfiltrasi cookie sensitif, atau melakukan tindakan tidak sah atas nama pengguna. Kerentanan ini umumnya ditemukan pada parameter pencarian, pesan kesalahan, dan endpoint mana pun yang merefleksikan input secara langsung kembali ke antarmuka pengguna. Eksploitasi bergantung pada interaksi korban dengan tautan berbahaya, menjadikannya vektor serangan non-persisten namun sangat efektif untuk menargetkan pengguna administratif atau pengguna terautentikasi tertentu.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INPV-01 |
| **CWE** | CWE-79 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Tinggi |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/01-Testing_for_Reflected_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/cross-site-scripting  

**Alat:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `XSStrike`, `KXS`, `dalfox`

### Apakah input yang diberikan pengguna direfleksikan dalam body respons?
- [ ] Tidak — input **tidak pernah** direfleksikan kembali ke pengguna  
- [ ] Ya — input direfleksikan tetapi di-encode dengan benar dan bypass **tidak dimungkinkan**  
- [ ] Ya — input direfleksikan **tanpa** encoding atau sanitasi apa pun  

### Apakah output encoding berbasis konteks diimplementasikan?
- [ ] Ya — encoding yang tepat (HTML, Atribut, JavaScript) **diterapkan** berdasarkan konteks refleksi yang spesifik  
- [ ] Ya — encoding **diterapkan** tetapi tidak memadai untuk konteks tertentu (misalnya, HTML encoding di dalam tag `<script>`)  
- [ ] Tidak — encoding **tidak diterapkan**  

### Dapatkah validasi input atau filter Web Application Firewall (WAF) di-bypass?
- [ ] Tidak — filter secara efektif memblokir semua payload XSS yang umum dan ter-obfuscated  
- [ ] Ya — filter tersedia tetapi bypass **dimungkinkan** menggunakan character encoding, tag non-standar, atau polyglot  
- [ ] Tidak — tidak ada filter atau mekanisme validasi yang diterapkan  

### Apa konteks eksekusi dari input yang direfleksikan?
- [ ] Aman — input direfleksikan di lokasi non-executable (misalnya, di dalam tag `<div>` atau `<span>` standar)  
- [ ] Berisiko — input direfleksikan di dalam atribut HTML atau di dalam `src`/`href` dari tag  
- [ ] Kritis — input direfleksikan langsung di dalam blok `<script>`, event handler, atau template literal  

### Apakah header keamanan browser modern tersedia untuk memitigasi dampak XSS?
- [ ] Ya — `Content-Security-Policy` (CSP) **diaktifkan** dengan script-src yang restriktif  
- [ ] Ya — `Content-Security-Policy` (CSP) **diaktifkan** tetapi mengandung `unsafe-inline` atau daftar putih (whitelist) yang lemah  
- [ ] Tidak — tidak ada header CSP atau header legacy `X-XSS-Protection` yang tersedia  

---