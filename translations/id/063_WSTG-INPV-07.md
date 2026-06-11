## WSTG-INPV-07 — XML Injection

XML Injection terjadi ketika sebuah aplikasi memasukkan data yang diberikan oleh pengguna ke dalam dokumen atau aliran (*stream*) XML tanpa validasi, sanitisasi, atau *escaping* karakter meta XML yang tepat. Dengan menyuntikkan tag XML atau elemen struktural tertentu, penyerang dapat memanipulasi logika dokumen, melewati mekanisme autentikasi, atau mengganggu integritas data yang diproses oleh *backend*. Kerentanan ini umumnya ditemukan pada layanan web berbasis SOAP, REST API yang menerima XML, asersi SAML, dan aplikasi yang menghasilkan file konfigurasi atau laporan XML secara dinamis. Dari perspektif penyerang, tujuannya adalah untuk keluar dari konteks data yang dimaksudkan guna mengubah struktur dokumen, yang berpotensi menyebabkan eskalasi hak istimewa (*privilege escalation*) yang tidak sah atau eksekusi logika bisnis yang tidak diinginkan.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INPV-07 |
| **CWE** | CWE-91 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi / Sedang |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/07-Testing_for_XML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/xxe  

**Alat:** `Burp Suite (Intruder/Repeater)`, `OWASP ZAP`, `XMLSpy`, `CyberChef`

### Apakah aplikasi menerima dan memproses input berformat XML dari pengguna?
- [ ] Tidak — aplikasi **tidak** memproses input XML  
- [ ] Ya — input XML diproses melalui API (SOAP/REST)  
- [ ] Ya — XML diproses melalui unggahan file (SVG, DOCX, dll.)  

### Apakah karakter meta XML (misalnya, `<`, `>`, `&`, `'`, `"`) di-*escape* atau disanitisasi dengan benar?
- [ ] Ya — semua karakter meta di-*escape* atau disanitisasi dengan **benar** *(Paling aman)*  
- [ ] Ya — beberapa pemfilteran diterapkan tetapi *bypass* **memungkinkan** menggunakan pengodean (*encoding*)  
- [ ] Tidak — input mentah dimasukkan langsung ke dalam struktur XML **tanpa** sanitisasi  

### Apakah validasi skema sisi server (XSD/DTD) diberlakukan untuk semua input XML?
- [ ] Ya — validasi skema yang ketat **diaktifkan** dan mencegah perubahan struktural  
- [ ] Ya — validasi **diaktifkan** tetapi **tidak** mencegah injeksi tag di dalam bidang data  
- [ ] Tidak — validasi skema **dinonaktifkan** atau tidak diimplementasikan  

### Bisakah penyerang menyuntikkan tag XML baru untuk mengubah struktur atau logika dokumen?
- [ ] Tidak — struktur tetap utuh terlepas dari input  
- [ ] Ya — tag dapat disuntikkan tetapi **tidak dapat** mengubah logika bisnis  
- [ ] Ya — injeksi tag **memungkinkan** dan mengizinkan *bypass* logika atau modifikasi data *(Kritis)*  

### Bisakah parser XML dimanipulasi menggunakan bagian CDATA atau komentar XML?
- [ ] Tidak — *parser* menangani atau menolak penanda CDATA/komentar dengan benar  
- [ ] Ya — injeksi `<![CDATA[...]]>` atau `<!-- ... -->` **memungkinkan** untuk menyembunyikan atau melewati filter  

---