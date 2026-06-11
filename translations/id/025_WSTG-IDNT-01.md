## WSTG-IDNT-01 — Pengujian Definisi Peran (Test Role Definitions)

Pengujian untuk definisi peran melibatkan identifikasi dan dokumentasi berbagai peran pengguna dan tingkat izin (permission levels) yang ditentukan dalam aplikasi untuk memastikan mereka dipisahkan secara logis dan mematuhi prinsip hak istimewa terendah (principle of least privilege). Seorang penyerang akan menganalisis aplikasi untuk memetakan peran dengan hak istimewa tinggi (high-privilege roles) versus peran pengguna standar, mencari ambiguitas dalam batasan izin atau fungsi administratif yang tidak terdokumentasi. Mengidentifikasi peran-peran ini adalah langkah pertama dalam menemukan jalur eskalasi hak istimewa (privilege escalation paths), karena ketidakkonsistenan dalam definisi peran sering kali menyebabkan akses tidak sah ke data sensitif atau fitur administratif. Fase ini biasanya terjadi selama pengintaian awal (initial reconnaissance) dan pemetaan logika bisnis (business logic mapping), yang berfokus pada profil pengguna, dasbor administratif, dan struktur izin API.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-IDNT-01 |
| **CWE** | CWE-285 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Rendah / Sedang |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/01-Test_Role_Definitions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite (Compare Site Maps)`, `Postman`, `AuthMatrix`, `Authz`, `Browser Developer Tools`

### Apakah peran didefinisikan dan didokumentasikan dengan jelas di dalam aplikasi atau dokumentasinya?
- [ ] Ya — peran didokumentasikan sepenuhnya dan mengikuti prinsip **least privilege**  
- [ ] Ya — peran didokumentasikan tetapi mengandung **izin yang tumpang tindih (overlapping permissions)**  
- [ ] Tidak — tidak ada dokumentasi atau definisi peran formal yang tersedia  

### Apakah terdapat pemisahan yang jelas antara fungsi administratif dan fungsi pengguna standar?
- [ ] Ya — fungsi administratif **terisolasi sepenuhnya** dari tampilan pengguna standar  
- [ ] Ya — terdapat pemisahan tetapi endpoint administratif bersifat **dapat diprediksi atau ditebak (predictable or guessable)**  
- [ ] Tidak — fungsi administratif dan standar **tercampur** dalam antarmuka yang sama  

### Apakah aplikasi menggunakan mekanisme terpusat untuk Role-Based Access Control (RBAC)?
- [ ] Ya — RBAC atau ABAC terpusat telah **diimplementasikan** dan ditegakkan secara konsisten  
- [ ] Ya — terdapat kontrol terdesentralisasi tetapi **diterapkan secara tidak konsisten** di berbagai modul  
- [ ] Tidak — logika kontrol akses **dihardcode** ke dalam halaman atau komponen individual  

### Apakah terdapat peran yang tidak terdokumentasi atau tersembunyi di dalam sistem?
- [ ] Tidak — semua peran yang ditemukan sesuai dengan fungsionalitas yang didokumentasikan  
- [ ] Ya — peran tersembunyi atau "shadow" (misalnya, `super_admin`, `support`, `test`) **tersedia**  

### Dapatkah pengguna dengan hak istimewa rendah melakukan enumerasi izin atau peran pengguna lain?
- [ ] Tidak — pengguna **tidak dapat** melihat atau melakukan enumerasi peran/izin dari entitas lain  
- [ ] Ya — pengguna **dapat** melakukan enumerasi peran melalui halaman profil, metadata, atau respons API *(Sedang)*  

---