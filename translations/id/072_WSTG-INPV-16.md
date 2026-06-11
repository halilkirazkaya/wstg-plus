## WSTG-INPV-16 — HTTP Request Smuggling

HTTP Request Smuggling (HRS) terjadi ketika rantai server HTTP (seperti load balancer dan back-end web server) menginterpretasikan batasan permintaan (request boundaries) secara berbeda, biasanya karena header `Content-Length` dan `Transfer-Encoding` yang bertentangan. Penyerang mengeksploitasi desinkronisasi ini untuk "menyelundupkan" (smuggle) entri ke dalam buffer permintaan server back-end, yang secara efektif memberikan awalan (prefix) pada permintaan pengguna sah berikutnya dengan segmen yang dikendalikan penyerang. Teknik ini memungkinkan serangan berat termasuk session hijacking, mem-bypass kontrol keamanan, dan cache poisoning. Eksploitasi biasanya dilakukan dengan analisis berbasis waktu (timing-based analysis) atau dengan mengamati respons yang tidak terduga dari back-end saat permintaan berikutnya dikirim ke server.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INPV-16 |
| **CWE** | CWE-444 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Kritis / Tinggi |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/16-Testing_for_HTTP_Request_Smuggling  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  
* https://portswigger.net/web-security/request-smuggling  

**Tools:** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `curl`, `SmuggleHunter`

### Apakah server front-end dan back-end menangani header `Transfer-Encoding: chunked` secara konsisten?
- [ ] Ya — kedua server menangani chunked encoding secara identik dan desinkronisasi **tidak dimungkinkan**  
- [ ] Tidak — server menginterpretasikan header secara berbeda tetapi sanitasi/normalisasi **diterapkan**  
- [ ] Tidak — desinkronisasi CL.TE (Content-Length/Transfer-Encoding) **dimungkinkan** *(Kritis)*  
- [ ] Tidak — desinkronisasi TE.CL (Transfer-Encoding/Content-Length) **dimungkinkan** *(Kritis)*  

### Dapatkah penyerang meracuni (poison) cache sisi server atau klien menggunakan permintaan yang diselundupkan?
- [ ] Tidak — caching **dinonaktifkan** atau tidak rentan terhadap peracunan  
- [ ] Ya — permintaan yang diselundupkan **dapat** mengalihkan pengguna ke domain berbahaya atau menyuntikkan skrip melalui cache poisoning  

### Apakah mungkin untuk menangkap permintaan pengguna lain atau token sesi melalui penggabungan permintaan (request concatenation)?
- [ ] Tidak — penyelundupan **tidak** memungkinkan paparan data antar-pengguna  
- [ ] Ya — penyelundupan memungkinkan penambahan permintaan pengguna berikutnya ke dalam bodi `POST` yang dikendalikan penyerang, sehingga eksfiltrasi **dimungkinkan**  

### Apakah infrastruktur menggunakan HTTP/2, dan apakah rentan terhadap desinkronisasi H2.CL atau H2.TE?
- [ ] Tidak — aplikasi hanya menggunakan HTTP/1.1 atau HTTP/2 diimplementasikan dengan aman tanpa downgrade  
- [ ] Ya — terjadi downgrade cleartext dari HTTP/2 ke HTTP/1.1 dan desinkronisasi **dimungkinkan**  

### Apakah header yang cacat (malformed) (misalnya, `Transfer-Encoding:  chunked` dengan spasi) ditangani dengan aman?
- [ ] Ya — header yang cacat ditolak atau dinormalisasi di semua tingkatan  
- [ ] Tidak — desinkronisasi TE.TE **dimungkinkan** karena penanganan header yang cacat atau tersembunyi (obscured) yang tidak konsisten  

---