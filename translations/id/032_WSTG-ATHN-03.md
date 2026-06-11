## WSTG-ATHN-03 — Testing for Weak Lock Out Mechanism

Mekanisme penguncian akun (account lockout mechanism) adalah kontrol pertahanan berlapis (defense-in-depth) yang krusial yang dirancang untuk memitigasi serangan brute-force dan credential stuffing dengan menonaktifkan akun setelah sejumlah upaya autentikasi yang gagal yang telah ditentukan. Tanpa kebijakan penguncian yang kuat, penyerang dapat menggunakan alat otomatis untuk menebak kata sandi secara sistematis pada beberapa akun (password spraying) atau menargetkan satu akun bernilai tinggi hingga berhasil. Kerentanan ini biasanya muncul pada portal login, formulir reset kata sandi, dan API endpoint yang tidak memiliki pembatasan laju (rate limiting) atau perlindungan tingkat akun. Dari sudut pandang penyerang, mekanisme penguncian yang lemah atau tidak ada memungkinkan upaya menebak kata sandi yang tidak terbatas, sementara mekanisme yang terlalu agresif dapat dieksploitasi untuk menyebabkan Denial of Service (DoS) terdistribusi terhadap pengguna yang sah dengan sengaja mengunci akun mereka.

| Kolom | Nilai |
|---|---|
| **WSTG ID** | WSTG-ATHN-03 |
| **CWE** | CWE-307 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Sedang / Tinggi* |

> *Tingkat keparahan menjadi Tinggi jika aplikasi menangani PII sensitif, data finansial, atau jika akun administratif rentan terhadap brute-force tanpa adanya kontrol sekunder.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/03-Testing_for_Weak_Lock_Out_Mechanism  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Alat:** `Burp Suite Intruder`, `THC-Hydra`, `ffuf`, `Patator`, `wfuzz`

### Apakah mekanisme penguncian akun diterapkan setelah sejumlah upaya gagal yang ditentukan?
- [ ] Ya — akun dikunci setelah ambang batas yang wajar dan ditentukan (misalnya, 5-10 upaya)  
- [ ] Ya — akun dikunci tetapi hanya setelah jumlah upaya yang sangat tinggi  
- [ ] Tidak — akun **tidak dapat** dikunci dan memungkinkan brute-force tanpa batas  

### Apakah mekanisme penguncian dapat dilewati menggunakan teknik penghindaran (evasion) umum?
- [ ] Tidak — penguncian diterapkan di sisi server (server-side) dan **tidak** dipengaruhi oleh perubahan di sisi klien (client-side)  
- [ ] Ya — penguncian **mungkin** dilewati dengan merotasi alamat IP atau menggunakan proxy pool  
- [ ] Ya — penguncian **mungkin** dilewati dengan memanipulasi header HTTP seperti `X-Forwarded-For` atau `User-Agent`  
- [ ] Ya — penguncian **mungkin** dilewati dengan menghapus cookie atau memulai sesi baru  

### Apakah mekanisme penguncian menyediakan jalur untuk pemulihan akun atau kedaluwarsa?
- [ ] Ya — akun dibuka secara otomatis setelah durasi yang ditentukan atau melalui verifikasi email  
- [ ] Ya — akun memerlukan intervensi administratif untuk dibuka  
- [ ] Tidak — penguncian bersifat permanen tanpa jalur pemulihan yang jelas, meningkatkan risiko DoS  

### Apakah aplikasi membedakan antara nama pengguna yang valid dan tidak valid selama penguncian?
- [ ] Tidak — respons aplikasi identik baik untuk pengguna yang ada maupun yang tidak ada  
- [ ] Ya — aplikasi mengungkapkan jika sebuah akun dikunci, memungkinkan terjadinya **enumerasi nama pengguna (username enumeration)**  

### Apakah mekanisme penguncian rentan terhadap serangan Denial of Service (DoS)?
- [ ] Tidak — kontrol seperti CAPTCHA atau rate limiting berbasis IP mencegah penguncian akun secara massal  
- [ ] Ya — penyerang **dapat** secara sistematis mengunci semua pengguna yang diketahui dengan memberikan kata sandi yang salah  

---