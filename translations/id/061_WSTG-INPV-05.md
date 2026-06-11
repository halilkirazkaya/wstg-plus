## WSTG-INPV-05 — SQL Injection

SQL Injection terjadi ketika input yang diberikan pengguna dimasukkan ke dalam query SQL tanpa parameterisasi atau sanitisasi yang tepat, sehingga memungkinkan penyerang untuk memanipulasi logika query. Eksploitasi yang berhasil dapat menyebabkan eksfiltrasi data yang tidak sah, bypass autentikasi, modifikasi data, dan dalam beberapa kasus, Remote Code Execution (RCE) melalui fitur basis data seperti `xp_cmdshell` atau `LOAD_FILE()`. Kerentanan ini umumnya memengaruhi formulir login, fungsi pencarian, parameter REST API, HTTP headers, dan endpoint mana pun yang menyusun query SQL dinamis dari input yang dikendalikan pengguna. Dari perspektif penyerang, mengidentifikasi titik injeksi adalah gerbang utama menuju kompromi basis data secara penuh dan potensi lateral movement di dalam infrastruktur.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INPV-05 |
| **CWE** | CWE-89 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Kritis (Critical) |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/05-Testing_for_SQL_Injection  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html  
* https://portswigger.net/web-security/sql-injection  
* https://portswigger.net/web-security/nosql-injection  

**Tools:** `sqlmap`, `Burp Suite (Intruder/Repeater)`, `Ghauri`, `jSQL Injection`, `BBQSQL`

### Apakah parameter yang dapat dikontrol pengguna diproses menggunakan metode akses basis data yang aman?
- [ ] Ya — aplikasi menggunakan ORM atau parameterized queries secara eksklusif dan bypass **tidak mungkin**  
- [ ] Ya — parameterized queries digunakan tetapi bypass **mungkin** melalui edge cases (misal: klausa `ORDER BY`)  
- [ ] Tidak — konkatenasi string mentah digunakan untuk membangun query SQL **tanpa** parameterisasi  

### Bisakah mekanisme autentikasi dilewati melalui SQL injection?
- [ ] Tidak — parameter login **tidak** rentan terhadap injeksi  
- [ ] Ya — bypass autentikasi **mungkin** menggunakan payload berbasis tautologi (misal: `' OR 1=1 --`)  

### Apakah eksfiltrasi data memungkinkan melalui teknik in-band atau out-of-band?
- [ ] Tidak — tidak ada eksfiltrasi data yang teridentifikasi  
- [ ] Ya — eksfiltrasi UNION-based atau error-based **mungkin** dilakukan  
- [ ] Ya — eksfiltrasi blind (Boolean atau Time-based) **mungkin** dilakukan  
- [ ] Ya — eksfiltrasi out-of-band (DNS/HTTP) **mungkin** dilakukan  

### Apakah fungsi aplikasi menunjukkan kerentanan second-order SQL injection?
- [ ] Tidak — data yang tersimpan ditangani dengan aman saat diambil untuk query selanjutnya  
- [ ] Ya — input pengguna disimpan dan kemudian digunakan dalam query SQL yang tidak aman **tanpa** validasi  

### Apakah hak istimewa akun layanan basis data dibatasi hingga minimum yang diperlukan?
- [ ] Ya — akun layanan memiliki **least privilege** (misal: terbatas pada tabel/tindakan tertentu saja)  
- [ ] Tidak — akun layanan memiliki hak istimewa tinggi (misal: `DROP TABLE`, hak akses `FILE`, atau `xp_cmdshell` **aktif**)  

---