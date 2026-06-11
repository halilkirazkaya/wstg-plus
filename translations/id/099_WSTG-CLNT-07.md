## WSTG-CLNT-07 — Cross-Origin Resource Sharing (CORS)

Cross-Origin Resource Sharing (CORS) adalah mekanisme berbasis browser yang memungkinkan server untuk secara eksplisit mengizinkan akses ke sumber dayanya (resources) dari origin yang berbeda, yang secara efektif melonggarkan Same-Origin Policy (SOP). Konfigurasi yang salah (misconfiguration) terjadi ketika aplikasi terlalu mempercayai origin arbitrer, seringkali dengan merefleksikan header `Origin` dalam header respons `Access-Control-Allow-Origin` atau dengan menggunakan whitelist regex yang diimplementasikan dengan buruk. Dari perspektif penyerang, kebijakan CORS yang permisif dapat dieksploitasi dengan menghosting skrip berbahaya pada domain yang dikendalikan yang membuat permintaan terautentikasi (authenticated requests) ke aplikasi yang rentan, sehingga memungkinkan penyerang untuk mengeksfiltrasi data sensitif, seperti token CSRF atau PII, langsung dari badan respons (response body). Kerentanan ini paling kritis pada endpoint API yang memproses sesi terautentikasi dan mengembalikan informasi non-publik.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CLNT-07 |
| **CWE** | CWE-942 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Sedang / Tinggi |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/07-Testing_Cross_Origin_Resource_Sharing  
* https://hacktricks.wiki/en/pentesting-web/cors-bypass.html  
* https://portswigger.net/web-security/cors  

**Tools:** `Burp Suite (CORS Raider)`, `curl`, `Postman`, `Corsy`

### Apakah aplikasi mengimplementasikan header CORS pada endpoint yang terautentikasi?
- [ ] Tidak — CORS **tidak diaktifkan**; browser kembali ke Same-Origin Policy yang ketat secara default  
- [ ] Ya — Header CORS ada dan dibatasi pada **whitelist** yang ketat  
- [ ] Ya — Header CORS ada tetapi dikonfigurasi secara **permisif**  

### Bagaimana server menangani header `Access-Control-Allow-Origin` (ACAO)?
- [ ] ACAO diatur ke domain statis yang spesifik dan **terpercaya**  
- [ ] ACAO diatur ke `*` (wildcard) dan kredensial **tidak dapat** dikirim  
- [ ] ACAO diatur ke `*` (wildcard) dan kredensial **dapat** dikirim *(Catatan: Sebagian besar browser memblokir hal ini)*  
- [ ] ACAO **merefleksikan secara dinamis** nilai header `Origin` dari permintaan  

### Apakah header `Access-Control-Allow-Credentials` (ACAC) diatur ke `true`?
- [ ] Tidak — ACAC **tidak ada** atau diatur ke `false`  
- [ ] Ya — ACAC diatur ke `true`, tetapi ACAO **dibatasi** pada origin yang terpercaya  
- [ ] Ya — ACAC diatur ke `true` dan ACAO **merefleksikan** origin arbitrer *(Kritis)*  

### Dapatkah whitelist origin di-bypass melalui manipulasi subdomain atau null origin?
- [ ] Tidak — whitelist ditegakkan secara ketat dan **tidak dapat** di-bypass  
- [ ] Ya — whitelist menerima **subdomain apa pun** dari target (misalnya, `attacker.target.com`)  
- [ ] Ya — whitelist menerima origin `null` melalui iframe yang di-sandbox  
- [ ] Ya — whitelist rentan terhadap bypass regex (misalnya, `target.com.attacker.com` atau `target.com-safe.com`)  

### Apakah konfigurasi CORS memungkinkan eksfiltrasi data sensitif?
- [ ] Tidak — endpoint yang terpapar **tidak** berisi data sensitif atau spesifik sesi  
- [ ] Ya — endpoint yang terautentikasi mengembalikan PII, kredensial, atau token CSRF yang **dapat** dibaca oleh origin yang tidak sah  

---