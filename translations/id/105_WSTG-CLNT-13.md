## WSTG-CLNT-13 — Testing for Cross Site Script Inclusion

Cross-Site Script Inclusion (XSSI) terjadi ketika aplikasi web mengekspor data sensitif dalam file JavaScript yang dihasilkan secara dinamis atau respons JSONP, yang kemudian dapat disertakan oleh situs yang dikendalikan penyerang melalui tag `<script>`. Karena penyertaan skrip melewati Same-Origin Policy (SOP), penyerang dapat mengeksekusi skrip dalam konteks mereka sendiri dan menimpa objek atau variabel global untuk mengeksfiltrasi informasi sensitif. Kerentanan ini biasanya muncul pada endpoint terautentikasi yang mengembalikan data spesifik pengguna seperti alamat email, API keys, atau metadata sesi dalam format skrip. Dari perspektif penyerang, eksploitasi melibatkan pembuatan halaman berbahaya yang mereferensikan skrip rentan dan menangkap perubahan variabel global atau pemanggilan fungsi yang dihasilkan.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CLNT-13 |
| **CWE** | CWE-200 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Sedang / Tinggi |

> *Tingkat keparahan menjadi Tinggi jika data yang bocor mencakup token CSRF, pengidentifikasi sesi, atau PII (Personally Identifiable Information) yang sangat sensitif.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/13-Testing_for_Cross_Site_Script_Inclusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `JSParser`, `LinkFinder`, `Browser Developer Tools`

### Apakah terdapat endpoint terautentikasi yang mengembalikan JavaScript dinamis atau JSONP yang berisi informasi sensitif?
- [ ] Tidak — semua skrip bersifat statis dan **tidak dapat** berisi data spesifik pengguna  
- [ ] Ya — terdapat skrip dinamis tetapi **tidak berisi** informasi sensitif  
- [ ] Ya — skrip dinamis berisi PII atau token dan **dapat** diakses lintas origin  

### Apakah header `X-Content-Type-Options: nosniff` diimplementasikan pada respons skrip yang sensitif?
- [ ] Ya — header **diterapkan** dan mencegah MIME-type sniffing  
- [ ] Tidak — header **hilang**, berpotensi memungkinkan konten non-skrip dieksekusi sebagai skrip  

### Apakah skrip sensitif dilindungi oleh mekanisme anti-XSSI seperti awalan non-executable atau header khusus?
- [ ] Ya — skrip memerlukan header khusus atau token yang **tidak dapat** dikirim melalui tag `<script>` standar  
- [ ] Ya — skrip menggunakan awalan non-executable (misal: `)]}'`) yang **tidak dapat** dilewati dengan mudah  
- [ ] Tidak — skrip dapat diakses langsung melalui permintaan GET menggunakan ambient credentials *(Kritis)*  

### Apakah eksfiltrasi data sensitif dimungkinkan dengan menimpa variabel global atau prototipe?
- [ ] Tidak — data sensitif memiliki cakupan lokal atau dilindungi melalui fitur peramban modern  
- [ ] Ya — data sensitif ditetapkan ke variabel global dan **dapat** diambil  
- [ ] Ya — data bocor melalui callback JSONP yang **dapat** dicegat  

---