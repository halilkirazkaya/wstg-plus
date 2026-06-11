## WSTG-CONF-13 — Path Confusion

Path Confusion muncul dari ketidaksesuaian dalam bagaimana berbagai komponen web, seperti reverse proxy, load balancer, dan server aplikasi backend, mengurai (parse) dan menafsirkan (interpret) URL path. Penyerang mengeksploitasi inkonsistensi ini dengan menyuntikkan karakter tertentu seperti titik koma, slash yang di-encode, atau segmen titik (dot-segments) untuk mengelabui filter keamanan dan mengakses endpoint yang dibatasi atau sumber daya statis. Kerentanan ini dapat menyebabkan kegagalan keamanan kritis, termasuk bypass autentikasi, akses data yang tidak sah, dan Web Cache Poisoning, karena front-end mungkin menerapkan aturan keamanan pada jalur yang ditafsirkan secara berbeda oleh back-end. Eksploitasi yang sukses biasanya terjadi pada batas arsitektural di mana perutean permintaan dan logika normalisasi berbeda di seluruh tech stack.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CONF-13 |
| **CWE** | CWE-444 |
| **Status Pengujian** | Belum Dilakukan |
| **Severitas** | Sedang / Tinggi* |

> *Severitas menjadi Tinggi jika path confusion menghasilkan bypass autentikasi atau kontrol akses untuk endpoint administratif yang sensitif.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/13-Test_for_Path_Confusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite`, `Dirsearch`, `FFUF`, `ParamMiner`, `Arjun`

### Apakah lapisan arsitektur yang berbeda menafsirkan pembatas jalur (path delimiters) (misalnya, `;`, `#`, `?`) secara konsisten?
- [ ] Ya — semua lapisan menormalisasi dan menafsirkan pembatas jalur secara identik  
- [ ] Tidak — terdapat inkonsistensi tetapi **tidak dapat** digunakan untuk mengakses sumber daya yang dibatasi  
- [ ] Tidak — perbedaan memungkinkan penyerang untuk melewati filter front-end dan mencapai logika back-end **adalah mungkin**  

### Apakah kontrol akses dapat di-bypass menggunakan urutan path traversal atau karakter yang di-encode?
- [ ] Tidak — normalisasi diterapkan secara konsisten sebelum pemeriksaan keamanan  
- [ ] Ya — bypass **mungkin dilakukan** melalui urutan segmen titik (misalnya, `/admin/..;/`)  
- [ ] Ya — bypass **mungkin dilakukan** melalui karakter URL-encoded (misalnya, `%2f`, `%2e%2e%2f`)  

### Apakah aplikasi rentan terhadap Web Cache Poisoning melalui path confusion?
- [ ] Tidak — logika caching tidak terpengaruh oleh ambiguitas jalur atau informasi jalur tambahan  
- [ ] Ya — konten berbahaya **dapat** di-cache untuk jalur yang sah karena ketidaksesuaian pemetaan  
- [ ] Ya — informasi sensitif **dapat** di-cache di direktori publik melalui teknik `RCD` (Relative Path Overwrite)  

### Apakah server menangani "informasi jalur tambahan" (PathInfo) secara aman?
- [ ] Tidak — fitur **dinonaktifkan** atau tidak ada  
- [ ] Ya — `PathInfo` ditangani dan **tidak** mengganggu filter keamanan  
- [ ] Tidak — `PathInfo` memungkinkan penyerang untuk menambahkan data yang membingungkan logika perutean aplikasi  

---