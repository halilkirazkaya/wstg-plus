## WSTG-INPV-14 — Testing for Incubated Vulnerabilities

Kerentanan inkubasi, yang juga dikenal sebagai kerentanan persisten atau *second-order*, terjadi ketika Payload berbahaya disimpan oleh aplikasi dalam keadaan "dingin" dan kemudian dieksekusi atau diproses dalam konteks yang berbeda. Pola serangan ini biasanya menargetkan sistem back-end, konsol administratif, atau alat pelaporan otomatis yang mengambil data dari basis data bersama atau filesystem tanpa validasi ulang atau API *output encoding* yang memadai. Pentester harus menelusuri siklus hidup input pengguna dari titik masuk ("inkubator") ke setiap lokasi di mana data tersebut akhirnya dirender atau digunakan oleh komponen lain atau aplikasi sekunder. Eksploitasi yang berhasil sering kali menghasilkan dampak tinggi seperti pembajakan sesi administratif melalui Stored XSS atau eksfiltrasi data melalui second-order SQL Injection dalam modul pelaporan internal.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INPV-14 |
| **CWE** | CWE-20 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Tinggi |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/14-Testing_for_Incubated_Vulnerability  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html#second-order-injection  
* https://portswigger.net/web-security/web-cache-deception  
* https://portswigger.net/web-security/web-cache-poisoning  

**Alat:** `Burp Suite (Collaborator)`, `sqlmap`, `OWASP ZAP`, `KXSs`, `Dalfox`

### Apakah terdapat endpoint di mana input yang dikontrol pengguna disimpan untuk diambil kemudian oleh pengguna atau proses lain?
- [ ] Tidak — aplikasi **tidak** menyimpan input pengguna untuk penggunaan di masa mendatang  
- [ ] Ya — input **disimpan** dalam kolom basis data, file log, atau pengaturan konfigurasi  

### Apakah validasi input atau sanitasi diterapkan sebelum data ditulis ke lapisan penyimpanan persisten?
- [ ] Ya — validasi *allow-list* yang ketat **diterapkan** pada titik masuk  
- [ ] Ya — sanitasi umum **diterapkan** tetapi bypass **dimungkinkan** melalui encoding atau karakter non-standar  
- [ ] Tidak — input disimpan dalam bentuk mentah (*raw*), tanpa validasi  

### Apakah data yang disimpan di-encode atau disanitasi dengan benar saat dirender di antarmuka administratif atau sekunder?
- [ ] Ya — *context-aware output encoding* **diterapkan** di setiap lokasi data dirender  
- [ ] Ya — encoding **diterapkan** di beberapa lokasi tetapi **tidak ada** di lokasi lain (misalnya, dasbor admin internal)  
- [ ] Tidak — data dirender tanpa encoding atau sanitasi apa pun, memungkinkan eksekusi Payload  

### Apakah Payload yang disimpan dalam basis data dapat memengaruhi proses batch back-end atau sistem pemantauan out-of-band?
- [ ] Tidak — proses back-end menggunakan metode parsing yang aman atau *parameterized queries*  
- [ ] Ya — Payload yang disimpan **dapat** memicu interaksi dalam logika back-end (misalnya, CSV Injection, Command Injection dalam log)  
- [ ] Ya — Payload yang disimpan **dapat** memicu permintaan *out-of-band* (OOB) ke server yang dikontrol penyerang  

---