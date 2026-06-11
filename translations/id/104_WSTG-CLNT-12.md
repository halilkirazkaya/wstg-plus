## WSTG-CLNT-12 — Menguji Penyimpanan Browser

Menguji penyimpanan browser melibatkan analisis terhadap bagaimana sebuah aplikasi memanfaatkan mekanisme sisi klien (client-side) seperti `LocalStorage`, `SessionStorage`, `IndexedDB`, dan `WebSQL` untuk menyimpan data secara persisten. Informasi sensitif yang disimpan secara tidak aman—termasuk PII, token otentikasi, atau identitas sesi—dapat dikompromikan jika penyerang berhasil melakukan Cross-Site Scripting (XSS) atau memperoleh akses fisik ke perangkat pengguna. Pentester harus menentukan apakah data disimpan dalam bentuk cleartext, apakah data tetap dapat diakses setelah logout, dan apakah siklus hidup penyimpanan dikelola dengan tepat untuk mencegah kebocoran data. Dari perspektif penyerang, mekanisme penyimpanan ini merupakan target yang menguntungkan untuk mengeksfiltrasi informasi pemeliharaan status (state-maintaining information) yang memungkinkan terjadinya pembajakan sesi (session hijacking) atau persistensi akun jangka panjang.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CLNT-12 |
| **CWE** | CWE-922 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan (Severity)** | Sedang / Tinggi* |

> *Tingkat keparahan menjadi Tinggi jika token sesi atau PII sensitif disimpan di `LocalStorage` dan dapat diakses melalui XSS.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/12-Testing_Browser_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Browser DevTools`, `Storage Explorer`, `Burp Suite`, `OWASP ZAP`

### Apakah aplikasi menyimpan informasi sensitif di penyimpanan browser?
- [ ] Tidak — aplikasi **tidak** menggunakan penyimpanan browser atau hanya menyimpan status UI yang tidak sensitif  
- [ ] Ya — data sensitif disimpan tetapi **dienkripsi** menggunakan kriptografi sisi klien yang kuat  
- [ ] Ya — data sensitif (token, PII, rahasia) disimpan dalam bentuk **cleartext** *(Sedang)*  

### Apakah data yang disimpan dapat diakses oleh skrip yang tidak sah (risiko XSS)?
- [ ] Tidak — data disimpan dalam cookie HttpOnly (bukan penyimpanan browser) atau penyimpanan **tidak digunakan**  
- [ ] Ya — `LocalStorage`/`SessionStorage` digunakan, membuat data **dapat diakses sepenuhnya** melalui payload XSS apa pun *(Tinggi)*  

### Apakah penyimpanan browser dibersihkan dengan tepat saat pengguna melakukan logout?
- [ ] Ya — semua penyimpanan yang terkait dengan aplikasi **dibersihkan secara eksplisit** selama proses logout  
- [ ] Tidak — penyimpanan tetap ada setelah logout, tetapi **tidak** berisi data sesi yang sensitif  
- [ ] Tidak — token otentikasi atau PII **tetap ada** dalam penyimpanan setelah sesi dihentikan *(Sedang)*  

### Apakah aplikasi menggunakan IndexedDB atau WebSQL untuk dataset yang besar/sensitif?
- [ ] Tidak — mekanisme penyimpanan ini **tidak digunakan**  
- [ ] Ya — data disimpan dan **disanitasi dengan benar** sebelum dirender di DOM  
- [ ] Ya — dataset sensitif disimpan dalam bentuk **cleartext** di dalam struktur `IndexedDB`/`WebSQL`  

### Apakah integritas data yang disimpan dapat dimanipulasi untuk memengaruhi logika aplikasi?
- [ ] Tidak — aplikasi memvalidasi atau menandatangani data sebelum memprosesnya dari penyimpanan  
- [ ] Ya — memodifikasi nilai penyimpanan (misalnya, peran pengguna, flag) **dimungkinkan** dan menghasilkan perubahan perilaku aplikasi  

---