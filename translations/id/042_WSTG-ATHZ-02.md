## WSTG-ATHZ-02 — Pengujian untuk Melewati Skema Otorisasi

Melewati skema otorisasi (Bypassing authorization schemas) terjadi ketika aplikasi gagal menerapkan kontrol akses (access controls), yang memungkinkan penyerang untuk mengakses sumber daya atau fungsi di luar izin yang seharusnya. Kerentanan ini biasanya bermanifestasi sebagai Eskalasi Hak Istimewa Horizontal (Horizontal Privilege Escalation), di mana penyerang mengakses data milik pengguna lain dengan tingkat yang sama, atau Eskalasi Hak Istimewa Vertikal (Vertical Privilege Escalation), di mana pengguna dengan hak istimewa rendah memperoleh kemampuan administratif. Penyerang mengeksploitasi cacat ini dengan memanipulasi parameter permintaan seperti ID sumber daya (Resource ID), memodifikasi token sesi (session tokens), atau memanfaatkan header HTTP seperti `X-Original-URL` untuk menghindari pembatasan berbasis jalur (path-based restrictions). Memastikan otorisasi yang kuat sangat penting untuk menjaga kerahasiaan data dan mencegah tindakan administratif tidak sah yang dapat membahayakan seluruh sistem.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-ATHZ-02 |
| **CWE** | CWE-285 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi / Kritis |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/02-Testing_for_Bypassing_Authorization_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**Alat:** `Burp Suite (Autorize extension)`, `Burp Suite (Repeater/Intruder)`, `Postman`, `cURL`, `OWASP ZAP`

### Apakah eskalasi hak istimewa horizontal dicegah di antara pengguna dengan peran yang sama?
- [ ] Ya — pemeriksaan otorisasi **diterapkan** pada setiap permintaan sumber daya dan bypass **tidak mungkin dilakukan**  
- [ ] Ya — pemeriksaan otorisasi **diterapkan** tetapi dapat dilewati melalui IDOR atau manipulasi parameter (parameter tampering)  
- [ ] Tidak — pengguna **dapat** mengakses data milik pengguna lain hanya dengan mengubah ID sumber daya *(Tinggi)*  

### Apakah eskalasi hak istimewa vertikal dicegah untuk pengguna dengan hak istimewa rendah?
- [ ] Tidak — aplikasi hanya memiliki satu tingkat hak istimewa  
- [ ] Ya — fungsi administratif **tidak dapat** diakses oleh pengguna dengan hak istimewa rendah  
- [ ] Ya — fungsi administratif disembunyikan di UI tetapi **dapat** diakses melalui URL langsung  
- [ ] Tidak — pengguna dengan hak istimewa rendah **dapat** melakukan tindakan administratif dengan memanipulasi peran atau izin *(Kritis)*  

### Apakah otorisasi dapat dilewati menggunakan manipulasi kata kerja HTTP (HTTP verb tampering)?
- [ ] Tidak — kontrol akses diterapkan terlepas dari metode HTTP yang digunakan  
- [ ] Ya — mengubah metode (misalnya, dari `GET` ke `POST` atau `PUT`) **melewati** pemeriksaan otorisasi  

### Apakah titik akhir (endpoints) administratif atau terbatas dapat diakses tanpa autentikasi apa pun?
- [ ] Tidak — semua titik akhir sensitif memerlukan sesi yang valid dan izin yang tepat  
- [ ] Ya — beberapa titik akhir administratif dapat diakses jika penyerang mengetahui URL langsungnya  
- [ ] Ya — akses tanpa autentikasi ke fungsi sensitif **dimungkinkan** *(Kritis)*  

### Apakah header HTTP memungkinkan pengelabuan otorisasi berbasis jalur (path-based authorization)?
- [ ] Tidak — aplikasi **tidak** rentan terhadap bypass berbasis header  
- [ ] Ya — header seperti `X-Original-URL`, `X-Rewrite-URL`, atau `X-Forwarded-For` **dapat** digunakan untuk melewati kontrol akses  

---