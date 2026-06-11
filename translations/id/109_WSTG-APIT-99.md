## WSTG-APIT-99 — Testing GraphQL

GraphQL adalah bahasa kueri (query language) untuk API yang memungkinkan klien meminta struktur data tertentu, tetapi miskonfigurasi sering kali menyebabkan risiko keamanan yang signifikan termasuk pengungkapan informasi (information disclosure) dan eksploitasi sumber daya (resource exhaustion). Kerentanan biasanya muncul dari introspeksi (introspection) yang diaktifkan, kurangnya batasan kedalaman/kompleksitas kueri, dan otorisasi tingkat objek yang rusak (Broken Object-Level Authorization - BOLA) di dalam fungsi resolver. Penyerang mengeksploitasi endpoint ini dengan menggunakan introspeksi untuk memetakan seluruh skema, memanfaatkan fragmen sirkular untuk memicu Denial of Service (DoS), atau melewati otorisasi dengan mengakses bidang yang tidak sah melalui kueri bersarang (nested queries). Pengujian berfokus pada endpoint `/graphql`, `/v1/graphql`, atau `/graphiql` untuk memastikan bahwa implementasi menerapkan kontrol akses (access control) dan manajemen sumber daya yang ketat.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-APIT-99 |
| **CWE** | CWE-200 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi / Kritis* |

> *Keparahan menjadi Kritis jika otorisasi tingkat objek yang rusak (BOLA) memungkinkan akses tidak sah ke data sensitif (PII) atau mutasi administratif.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/99-Testing_GraphQL  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/graphql  

**Alat:** `InQL`, `Graphw00f`, `GraphQL Voyager`, `Burp Suite`, `Altair GraphQL Client`, `Clairvoyance`

### Apakah sistem introspeksi (introspection) GraphQL diaktifkan?
- [ ] Tidak — introspeksi **dinonaktifkan** dan skema tidak dapat dipetakan  
- [ ] Ya — introspeksi **diaktifkan** tetapi memerlukan autentikasi administratif  
- [ ] Ya — introspeksi **diaktifkan** dan dapat diakses secara publik *(Tinggi)*  

### Apakah batasan sumber daya (kedalaman dan kompleksitas) diterapkan pada kueri?
- [ ] Ya — batasan kedalaman dan kompleksitas kueri yang ketat **diterapkan** dan dijalankan  
- [ ] Ya — batasan **diterapkan** tetapi dapat dilewati menggunakan alias atau fragmen  
- [ ] Tidak — tidak ada batasan yang **diterapkan**, memungkinkan kueri rekursif dan DoS  

### Apakah Broken Object-Level Authorization (BOLA) terdapat pada resolver?
- [ ] Tidak — otorisasi **diterapkan** pada tingkat bidang dan objek untuk setiap kueri  
- [ ] Ya — otorisasi **diterapkan** pada beberapa bidang tetapi objek sensitif terekspos  
- [ ] Ya — otorisasi **tidak diterapkan**, memungkinkan akses ke objek apa pun melalui manipulasi ID  

### Apakah mutasi (mutations) GraphQL dilindungi dengan benar dari penggunaan yang tidak sah?
- [ ] Tidak — mutasi **tidak** ada di dalam skema  
- [ ] Ya — mutasi memerlukan autentikasi yang valid dan pemeriksaan otorisasi yang ketat  
- [ ] Ya — mutasi **mungkin dilakukan** oleh pengguna yang tidak terautentikasi atau kurang otorisasi  

### Apakah pesan kesalahan membocorkan detail implementasi atau skema yang sensitif?
- [ ] Tidak — pesan kesalahan bersifat umum dan **tidak** mengungkapkan logika internal  
- [ ] Ya — pesan kesalahan mengungkapkan tipe database yang mendasari atau saran melalui "Did you mean...?"  
- [ ] Ya — full stack traces dan data konfigurasi sensitif **terungkap** dalam respons  

---