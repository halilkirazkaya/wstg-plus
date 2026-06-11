## WSTG-CONF-10 — Subdomain Takeover

Pengambilalihan subdomain (Subdomain Takeover) terjadi ketika entri DNS (biasanya CNAME, tetapi terkadang record A atau MX) mengarah ke sumber daya yang sudah tidak digunakan atau tidak ada pada penyedia cloud pihak ketiga atau layanan hosting. Penyerang mengidentifikasi record DNS yang "menggantung" (dangling) ini dengan mencari pesan kesalahan spesifik dari penyedia seperti AWS, GitHub, atau Azure yang menunjukkan bahwa suatu sumber daya tidak lagi diklaim. Dengan mendaftarkan nama sumber daya yang ditinggalkan tersebut di platform penyedia, penyerang mendapatkan kendali penuh atas subdomain tersebut, memungkinkan mereka untuk melakukan hosting konten berbahaya, mengeksfiltrasi cookie sesi yang dicakupkan ke domain utama, serta mem-bypass perlindungan Content Security Policy (CSP) atau CORS. Kerentanan ini sangat berbahaya karena memanfaatkan kepercayaan yang terkait dengan struktur domain resmi organisasi untuk memfasilitasi phishing berdampak tinggi dan pencurian data.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CONF-10 |
| **CWE** | CWE-1329 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi / Kritis* |

> *Keparahan menjadi Kritis jika subdomain dapat digunakan untuk membajak cookie sesi dari domain utama atau mem-bypass redirect SSO/OAuth.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/10-Test_for_Subdomain_Takeover  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://github.com/EdOverflow/can-i-take-over-xyz  

**Peralatan:** `Subjack`, `SubOver`, `dnscan`, `Amass`, `Nuclei`, `dig`, `nslookup`

### Apakah aplikasi menggunakan hosting pihak ketiga atau layanan cloud melalui subdomain?
- [ ] Tidak — semua subdomain mengarah ke ruang IP yang dikontrol organisasi  
- [ ] Ya — subdomain mengarah ke penyedia pihak ketiga (S3, GitHub Pages, Heroku, dll.)  

### Apakah terdapat record DNS yang "menggantung" (dangling) yang mengarah ke sumber daya yang tidak ada?
- [ ] Tidak — semua record DNS yang teridentifikasi mengarah ke sumber daya aktif yang terkontrol  
- [ ] Ya — record CNAME atau A ada untuk sumber daya yang **sudah tidak ada lagi** pada penyedia  
- [ ] Ya — record DNS mengarah ke NXDOMAIN atau halaman kesalahan spesifik penyedia *(misalnya, "NoSuchBucket")*  

### Apakah sumber daya menggantung yang teridentifikasi dapat berhasil diklaim?
- [ ] Tidak — penyedia memiliki perlindungan terhadap pengklaiman nama yang ditinggalkan atau diperlukan verifikasi akun  
- [ ] Ya — nama sumber daya **dapat** didaftarkan pada platform pihak ketiga oleh pengguna mana pun  

### Apakah domain utama menggunakan cookie dengan cakupan "Domain" yang dapat dikompromikan?
- [ ] Tidak — cookie dibatasi secara ketat pada subdomain tertentu atau menggunakan flag `Host-Only`  
- [ ] Ya — cookie sensitif (ID sesi, JWT) dicakupkan ke domain induk dan **dapat** dieksfiltrasi melalui subdomain yang dibajak  

### Apakah subdomain dapat digunakan untuk mem-bypass header keamanan atau alur autentikasi?
- [ ] Tidak — tidak ada ketergantungan keamanan pada subdomain yang teridentifikasi  
- [ ] Ya — subdomain masuk dalam daftar putih (whitelisted) dalam direktif `script-src` atau `connect-src` CSP  
- [ ] Ya — subdomain merupakan URI pengalihan (redirect URI) yang valid untuk konfigurasi OAuth atau SSO  

---