## WSTG-INPV-06 — Testing for LDAP Injection

LDAP Injection terjadi ketika aplikasi memasukkan data yang diberikan pengguna ke dalam filter LDAP (Lightweight Directory Access Protocol) tanpa sanitisasi atau escaping yang tepat, sehingga memungkinkan penyerang untuk memanipulasi logika kueri. Dengan menyuntikkan karakter khusus seperti tanda bintang (asterisk), tanda kurung, dan operator logika, penyerang dapat mengubah filter pencarian untuk melewati mekanisme autentikasi atau mengeksfiltrasi informasi direktori yang sensitif termasuk username, keanggotaan grup, dan atribut organisasi. Kerentanan ini biasanya muncul pada fitur-fitur seperti pencarian direktori perusahaan, portal karyawan, atau sistem Single Sign-On (SSO) yang terhubung dengan Active Directory atau OpenLDAP. Dari sudut pandang penyerang, eksploitasi yang berhasil sering kali melibatkan teknik blind untuk melakukan enumerasi struktur direktori atau nilai atribut secara bit-demi-bit ketika output kueri langsung ditekan oleh aplikasi.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INPV-06 |
| **CWE** | CWE-90 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/06-Testing_for_LDAP_Injection  
* https://hacktricks.wiki/en/pentesting-web/ldap-injection.html  

**Tools:** `Burp Suite (Intruder/Repeater)`, `ldapsearch`, `JNDIExploit`, `Wfuzz`, `nmap`

### Apakah aplikasi menggunakan LDAP untuk autentikasi atau pencarian direktori?
- [ ] Tidak — LDAP **tidak digunakan** dalam arsitektur aplikasi  
- [ ] Ya — LDAP digunakan dan semua input disanitisasi secara ketat atau diparameterisasi  
- [ ] Ya — LDAP digunakan dan input pengguna digabungkan ke dalam filter **tanpa** escaping yang tepat  

### Apakah meta-karakter LDAP telah di-escape atau difilter dengan benar pada input pengguna?
- [ ] Ya — karakter seperti `(`, `)`, `&`, `|`, `*`, dan `\` **tidak diizinkan** atau di-escape dengan benar  
- [ ] Tidak — meta-karakter **dapat** disuntikkan untuk mengubah struktur filter LDAP  

### Dapatkah mekanisme autentikasi dilewati melalui injeksi?
- [ ] Tidak — logika login **tidak rentan** terhadap manipulasi kueri  
- [ ] Ya — autentikasi **dapat** dilewati menggunakan injeksi logika OR (`|`) atau wildcard (`*`) pada kolom username atau password  

### Apakah eksfiltrasi data secara blind dimungkinkan dari layanan direktori?
- [ ] Tidak — hasil pencarian terbatas dan tidak ada perbedaan waktu (timing) atau boolean yang teramati  
- [ ] Ya — atribut direktori **dapat** dienumerasi secara bit-demi-bit melalui teknik boolean-based blind injection  
- [ ] Ya — rekaman direktori lengkap **ditampilkan** dalam respons aplikasi karena manipulasi kueri  

### Apakah komponen aplikasi sekunder (misalnya, pengirim email, SSO) rentan terhadap injeksi atribut berbasis LDAP?
- [ ] Tidak — atribut LDAP ditangani dengan aman di semua komponen  
- [ ] Ya — penyerang **dapat** mengubah atribut mereka sendiri (misalnya, email, keanggotaan grup) melalui injeksi untuk meningkatkan hak akses (escalate privileges) atau mengalihkan komunikasi  

---