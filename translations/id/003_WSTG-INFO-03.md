## WSTG-INFO-03 — Meninjau Metafile Webserver untuk Kebocoran Informasi (Information Leakage)

Metafile webserver seperti `robots.txt`, `sitemap.xml`, dan `security.txt` dimaksudkan untuk memandu crawler dan menyediakan metadata administratif, namun seringkali secara tidak sengaja mengungkapkan struktur direktori yang sensitif atau endpoint yang tersembunyi. Penyerang menganalisis file-file ini untuk melakukan enumerasi terhadap attack surface aplikasi, mengidentifikasi arahan "Disallow" yang sering kali merujuk pada panel administratif yang tidak tertaut, direktori cadangan (backup), atau lingkungan pengembangan (development environment). Selain instruksi mesin pencari, file dalam direktori `.well-known/` atau file seperti `humans.txt` dapat mengungkapkan technology stack, informasi pengembang, atau kontak keamanan organisasi. Peninjauan sistematis terhadap file-file ini memungkinkan penguji untuk mendapatkan wawasan struktural dan teknis tanpa melakukan penemuan Brute Force yang agresif.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INFO-03 |
| **CWE** | CWE-200 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Low / Informational* |

> *Tingkat keparahan menjadi Medium jika metafile mengungkapkan antarmuka administratif tanpa autentikasi atau cadangan konfigurasi yang sensitif.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/03-Review_Webserver_Metafiles_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `curl`, `wget`, `Burp Suite`, `FFUF`, `GoBuster`

### Apakah file `robots.txt` ada dan mengekspos direktori sensitif?
- [ ] Tidak — `robots.txt` **tidak** ada  
- [ ] Ya — `robots.txt` ada tetapi hanya berisi jalur publik standar  
- [ ] Ya — `robots.txt` mengungkapkan jalur direktori sensitif atau antarmuka administratif  
- [ ] Ya — `robots.txt` mengungkapkan kredensial atau versi perangkat lunak tertentu dalam komentar  

### Apakah file `sitemap.xml` ada dan mengungkapkan struktur aplikasi yang tersembunyi?
- [ ] Tidak — `sitemap.xml` **tidak** ada  
- [ ] Ya — `sitemap.xml` ada tetapi hanya mencantumkan URL publik  
- [ ] Ya — `sitemap.xml` mencantumkan endpoint yang tidak tertaut atau sumber daya internal yang **dapat** diakses  

### Apakah metafile keamanan dan organisasi mengekspos detail teknis?
- [ ] Tidak — tidak ada file metadata yang ditemukan di root atau `.well-known/`  
- [ ] Ya — `security.txt` atau `humans.txt` ada tetapi berisi informasi standar  
- [ ] Ya — metafile mengungkapkan hostname internal, identitas pengembang, atau detail technology stack yang **dapat** membantu dalam social engineering atau serangan lebih lanjut  

### Apakah terdapat metafile warisan (legacy) atau metafile khusus framework yang ada?
- [ ] Tidak — tidak ditemukan file khusus framework (contoh: `info.php`, `composer.json`, `package.json`)  
- [ ] Ya — file seperti `README.md`, `CHANGELOG.txt`, atau `.DS_Store` ada tetapi telah dibersihkan (sanitized)  
- [ ] Ya — file khusus framework atau versi tertentu ada dan **memungkinkan** dilakukannya fingerprinting teknologi yang tepat  

---