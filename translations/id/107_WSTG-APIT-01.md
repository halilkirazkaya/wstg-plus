## WSTG-APIT-01 — API Reconnaissance

API reconnaissance adalah proses sistematis untuk mengidentifikasi dan memetakan permukaan serangan (attack surface) API guna mengungkap endpoint, metode yang didukung, dan struktur data yang mendasarinya. Penyerang melakukan fase ini untuk menemukan API yang tidak terdokumentasi (shadow API), versi yang sudah tidak digunakan lagi (deprecated) dengan kerentanan warisan (legacy vulnerabilities), serta berkas dokumentasi yang terekspos seperti definisi Swagger atau OpenAPI yang mengungkapkan logika bisnis internal. Dengan menganalisis pola URL yang dapat diprediksi, berkas JavaScript sisi klien (client-side), dan hasil directory brute-force, seorang lawan dapat membangun peta komprehensif dari API untuk mengidentifikasi target serangan Broken Object Level Authorization (BOLA) atau Mass Assignment. Rekognisi ini biasanya menargetkan jalur umum seperti `/api/v1/`, `/swagger.json`, atau `/graphql` untuk mendapatkan peta jalan dari fungsionalitas backend aplikasi.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-APIT-01 |
| **CWE** | CWE-200 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Informasional / Rendah |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/01-API_Reconnaissance  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/api-testing  

**Alat:** `Kiterunner`, `ffuf`, `Arjun`, `Burp Suite (Logger++)`, `Postman`, `Swagger-ez`

### Apakah berkas dokumentasi API (Swagger, OpenAPI, WSDL) dapat diakses secara publik?
- [ ] Tidak — dokumentasi API **tidak** dapat diakses atau dibatasi oleh autentikasi  
- [ ] Ya — dokumentasi dapat diakses namun memerlukan kredensial yang valid  
- [ ] Ya — dokumentasi API (misalnya, `/swagger-ui.html`) **dapat diakses secara publik** tanpa autentikasi  

### Dapatkah endpoint API yang tidak terdokumentasi atau "shadow API" ditemukan melalui fuzzing?
- [ ] Tidak — fuzzing direktori dan endpoint tidak menghasilkan jalur yang tidak terdokumentasi  
- [ ] Ya — endpoint yang tidak terdokumentasi ditemukan tetapi **tidak dapat** diakses tanpa otorisasi  
- [ ] Ya — endpoint yang tidak terdokumentasi ditemukan dan **dapat** diakses tanpa otorisasi yang valid  

### Apakah aplikasi mengungkapkan beberapa versi API (misalnya, /v1/, /v2/, /beta/)?
- [ ] Tidak — hanya versi API saat ini yang sudah diperkuat (hardened) yang dapat diakses  
- [ ] Ya — versi lama (legacy) tersedia tetapi menerapkan kontrol keamanan yang **sama** dengan versi saat ini  
- [ ] Ya — versi lama (misalnya, `/v1/`) dapat diakses dan **tidak memiliki** kontrol keamanan seperti pada versi yang lebih baru  

### Apakah sumber daya sisi klien (JavaScript/Aplikasi Mobile) membocorkan struktur endpoint API atau kunci?
- [ ] Tidak — kode sisi klien **tidak** mengandung jalur API yang dikodekan secara keras (hardcoded) atau kunci sensitif  
- [ ] Ya — kode sisi klien mengandung peta endpoint API tetapi **tidak ada** kunci sensitif  
- [ ] Ya — kunci API, rahasia (secrets), atau URL endpoint khusus internal **dikodekan secara keras (hardcoded)** dalam sumber daya sisi klien  

### Apakah parameter atau header tersembunyi dapat ditemukan pada endpoint yang diketahui?
- [ ] Tidak — fuzzing parameter (misalnya, melalui `Arjun`) **tidak** mengungkapkan input tersembunyi  
- [ ] Ya — parameter tersembunyi (misalnya, `debug=true`, `admin=1`) ditemukan tetapi **tidak** berfungsi  
- [ ] Ya — parameter tersembunyi ditemukan dan **dapat** digunakan untuk mengubah perilaku aplikasi  

---