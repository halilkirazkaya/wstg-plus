## WSTG-CONF-12 — Content Security Policy (CSP)

Content Security Policy (CSP) adalah mekanisme pertahanan mendalam (defense-in-depth) kritis yang diimplementasikan melalui header respons HTTP untuk memitigasi risiko seperti Cross-Site Scripting (XSS), clickjacking, dan serangan data injection. Dengan mendefinisikan sumber daya dinamis mana yang diizinkan untuk dimuat, CSP yang dikonfigurasi dengan baik membatasi kemampuan penyerang untuk mengeksekusi skrip yang tidak sah atau mengeksfiltrasi data sensitif ke domain eksternal. Pentester mengevaluasi kebijakan untuk direktif yang terlalu permisif, penggunaan kata kunci yang tidak aman seperti `'unsafe-inline'`, dan ketergantungan pada CDN yang tidak aman yang mungkin memfasilitasi bypass. CSP yang lemah atau tidak ada tidak menciptakan kerentanan secara langsung tetapi secara signifikan meningkatkan dampak dari serangan injeksi yang berhasil karena gagal menyediakan lapisan perlindungan sekunder.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CONF-12 |
| **CWE** | CWE-693 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Rendah / Sedang |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/12-Test_for_Content_Security_Policy  
* https://hacktricks.wiki/en/pentesting-web/content-security-policy-csp-bypass/index.html  

**Alat:** `Google CSP Evaluator`, `Burp Suite (CSP Pro)`, `Mozilla Observatory`, `ZAP`, `CSP Mitigator`

### Apakah header Content Security Policy (CSP) terdapat dalam respons aplikasi?
- [ ] Ya — header `Content-Security-Policy` **ada** dan **diterapkan**  
- [ ] Ya — `Content-Security-Policy-Report-Only` **ada** untuk pengujian  
- [ ] Tidak — tidak ada header CSP yang **ada** dalam respons  

### Apakah direktif `script-src` atau `default-src` dibatasi dengan benar?
- [ ] Ya — direktif menggunakan whitelisting yang ketat, nonce, atau hash dan bypass **tidak dimungkinkan**  
- [ ] Ya — direktif **dikonfigurasi dengan benar** tetapi whitelist mencakup CDN yang diketahui dapat di-bypass  
- [ ] Tidak — direktif menggunakan wildcard (`*`) atau skema `data:`, sehingga membuat bypass **mungkin dilakukan**  

### Apakah kebijakan mengizinkan eksekusi skrip inline yang tidak aman atau eval?
- [ ] Tidak — `'unsafe-inline'` dan `'unsafe-eval'` **tidak ada**  
- [ ] Ya — `'unsafe-inline'` **ada** tetapi dilindungi oleh `nonce` atau `hash`  
- [ ] Ya — `'unsafe-inline'` atau `'unsafe-eval'` **diaktifkan** tanpa perlindungan tambahan *(Kritis)*  

### Apakah aplikasi terlindungi dari clickjacking melalui direktif `frame-ancestors`?
- [ ] Ya — `frame-ancestors` diatur ke `'none'` atau `'self'`  
- [ ] Ya — `frame-ancestors` **diaktifkan** tetapi mengizinkan domain pihak ketiga tepercaya tertentu  
- [ ] Tidak — `frame-ancestors` **tidak ada**, mengandalkan `X-Frame-Options` warisan atau tanpa perlindungan  

### Apakah terdapat mekanisme untuk melaporkan pelanggaran CSP?
- [ ] Ya — `report-uri` atau `report-to` **dikonfigurasi** dan aktif  
- [ ] Tidak — pelaporan pelanggaran **dinonaktifkan** atau **tidak dikonfigurasi**  

---