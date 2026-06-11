## WSTG-INFO-07 — Memetakan Jalur Eksekusi Melalui Aplikasi

Memetakan jalur eksekusi melibatkan identifikasi sistematis terhadap semua endpoint yang dapat dijangkau, aliran fungsional, dan percabangan pengambilan keputusan di dalam aplikasi web. Proses ini sangat penting untuk memastikan bahwa tidak ada fungsionalitas "gelap" (dark functionality)—seperti kode lama (legacy code), antarmuka debug, atau rute API yang tidak terdokumentasi—tetap tersembunyi dari pengawasan keamanan, karena hal-hal tersebut sering kali kekurangan kontrol keamanan modern. Penyerang menggunakan kombinasi automated spidering dan eksplorasi manual untuk memvisualisasikan struktur aplikasi, yang sering kali menemukan kerentanan seperti akses administratif yang tidak sah atau kelemahan logika bisnis (business logic flaws) selama proses tersebut. Dengan mengkorelasikan input ke respons sisi server tertentu, penguji dapat menentukan titik logika pemrosesan sensitif yang memerlukan pengujian mendalam yang terarah guna memastikan cakupan yang kuat.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INFO-07 |
| **CWE** | CWE-200 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Informasi |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/07-Map_Execution_Paths_Through_Application  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite Professional`, `OWASP ZAP`, `Katana`, `FFUF`, `LinkFinder`, `GoBuster`, `ParamMiner`

### Apakah aplikasi telah diindeks sepenuhnya menggunakan crawling otomatis dan manual?
- [ ] Ya — peta komprehensif dari semua sumber daya yang terlihat dan tertaut **telah selesai**  
- [ ] Ya — crawling otomatis **telah selesai** tetapi eksplorasi manual masih tertunda  
- [ ] No — aplikasi **belum** diindeks  

### Apakah endpoint tersembunyi atau tidak terdokumentasi diidentifikasi melalui analisis JS atau directory brute-forcing?
- [ ] Tidak — tidak ada jalur tidak terdokumentasi yang ditemukan melalui analisis side-channel  
- [ ] Ya — jalur tersembunyi ditemukan tetapi **tidak dapat** diakses tanpa otorisasi  
- [ ] Ya — endpoint tidak terdokumentasi **dapat** diakses dan menyediakan fungsionalitas sensitif *(Kritis / Tinggi)*  

### Bisakah jalur eksekusi diubah melalui parameter atau header non-standar?
- [ ] Tidak — parameter seperti `debug`, `test`, atau `admin` **tidak** memengaruhi alur eksekusi  
- [ ] Ya — parameter mengubah output tetapi **tidak** melewati (bypass) kontrol keamanan  
- [ ] Ya — parameter pengubah jalur **diterapkan** dan memungkinkan bypass logika yang dimaksudkan  

### Apakah alur logika bisnis multi-langkah (misalnya, multi-factor auth, checkout) telah dipetakan sepenuhnya?
- [ ] Ya — semua status (state) dan transisi dalam proses multi-langkah **telah diidentifikasi**  
- [ ] Tidak — transisi state machine yang kompleks **tidak dapat** dipetakan sepenuhnya  

### Apakah file dokumentasi API (Swagger/OpenAPI) atau peta sisi klien (Source Maps) terekspos?
- [ ] Tidak — tidak ada peta arsitektur atau file dokumentasi yang terekspos secara publik  
- [ ] Ya — dokumentasi tersedia tetapi **dilindungi** oleh autentikasi  
- [ ] Ya — Swagger UI, OpenAPI JSON, atau JS Source Maps **diaktifkan** dan dapat diakses secara publik  

---