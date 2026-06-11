## WSTG-INPV-15 — HTTP Splitting Smuggling

Kerentanan HTTP Splitting dan Smuggling muncul dari ketidaksesuaian tentang bagaimana proksi frontend dan server backend menginterpretasikan serta memproses batas-batas permintaan HTTP, khususnya terkait header `Content-Length` dan `Transfer-Encoding`. Dengan menyusun permintaan yang ambigu, penyerang dapat "menyelundupkan" (smuggle) permintaan tersembunyi ke backend atau menyuntikkan sekuens CRLF untuk memecah respons, yang menyebabkan cache poisoning, pembajakan permintaan (request hijacking), atau bypass kontrol keamanan. Celah ini biasanya bermanifestasi dalam lingkungan kompleks yang menggunakan reverse proxy, load balancer, atau CDN yang memiliki logika pemrosesan (parsing logic) yang tidak konsisten. Dari sudut pandang penyerang, hal ini memungkinkan pengalihan lalu lintas pengguna, pencurian token sesi sensitif, dan eksekusi tindakan yang tidak sah dalam konteks sesi pengguna lain.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INPV-15 |
| **CWE** | CWE-444 |
| **Status Pengujian** | Tidak Dilakukan |
| **Tingkat Kerawanan** | Tinggi / Kritis |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/15-Testing_for_HTTP_Response_Splitting  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  

**Alat:** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `smuggler.py`, `curl`

### Apakah lingkungan rentan terhadap request smuggling melalui ketidaksesuaian CL.TE atau TE.CL?
- [ ] Tidak — server frontend dan backend menangani batas-batas permintaan secara konsisten  
- [ ] Ya — terdapat ketidaksesuaian tetapi eksploitasi **tidak dimungkinkan** karena mitigasi infrastruktur  
- [ ] Ya — smuggling CL.TE atau TE.CL **dimungkinkan**, memungkinkan permintaan tersembunyi mencapai backend *(Tinggi)*  
- [ ] Ya — TE.TE (double encoding/obfuscation) **dimungkinkan** untuk melakukan bypass filter frontend *(Kritis)*  

### Apakah HTTP Response Splitting dapat dicapai melalui injeksi CRLF pada header?
- [ ] Tidak — aplikasi melakukan sanitasi sekuens CRLF dengan benar pada semua input header  
- [ ] Ya — sekuens CRLF tercermin dalam header tetapi response splitting **tidak dimungkinkan**  
- [ ] Ya — injeksi CRLF **dimungkinkan**, memungkinkan injeksi header atau cache poisoning  

### Apakah kontrol keamanan (WAF/ACL) dapat di-bypass menggunakan permintaan yang diselundupkan (smuggled requests)?
- [ ] Tidak — kontrol keamanan berlaku untuk permintaan luar maupun yang diselundupkan  
- [ ] Ya — permintaan yang diselundupkan **dapat** melewati aturan WAF frontend atau ACL berbasis IP  

### Apakah mungkin untuk membajak sesi pengguna lain atau mengalihkan lalu lintas mereka?
- [ ] Tidak — aliran request/response terisolasi dan tidak dapat bersilangan  
- [ ] Ya — request smuggling **dimungkinkan** dan memungkinkan pengambilan permintaan pengguna lain *(Kritis)*  
- [ ] Ya — response splitting **dimungkinkan** dan memungkinkan browser-side cache poisoning atau XSS  

---