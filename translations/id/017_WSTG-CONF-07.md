## WSTG-CONF-07 — Menguji HTTP Strict Transport Security

HTTP Strict Transport Security (HSTS) adalah mekanisme keamanan yang memungkinkan server web untuk memberitahu peramban (browser) bahwa server tersebut hanya boleh diakses melalui HTTPS, mencegah serangan penurunan protokol (protocol downgrade attacks) dan pembajakan cookie (cookie hijacking). Dengan mewajibkan koneksi terenkripsi, HSTS memitigasi serangan Man-in-the-Middle (MitM) seperti SSLStrip, di mana penyerang mencegat permintaan HTTP awal yang tidak aman untuk mencegah transisi ke TLS. Dari perspektif penyerang, tidak adanya header ini atau durasi `max-age` yang singkat memberikan celah kesempatan untuk mencegat token sesi sensitif atau kredensial selama proses jabat tangan (handshake) teks polos awal. Pengujian ini berfokus pada verifikasi keberadaan, durasi, dan cakupan header `Strict-Transport-Security` di semua titik akhir (endpoints) aplikasi yang sensitif.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CONF-07 |
| **CWE** | CWE-319 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan (Severity)** | Rendah / Menengah* |

> *Keparahan menjadi Menengah jika aplikasi menangani PII, token sesi, atau kredensial, karena kurangnya HSTS meningkatkan tingkat keberhasilan serangan MitM.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/07-Test_HTTP_Strict_Transport_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `curl`, `Burp Suite`, `nmap`, `SSLLabs`, `hsts-preload-checker`

### Apakah header `Strict-Transport-Security` ada dalam respons HTTPS?
- [ ] Ya — header **tersedia** di semua titik akhir sensitif  
- [ ] Ya — header **tersedia** tetapi hanya pada halaman landas (landing page)  
- [ ] Tidak — header **tidak ditemukan** di aplikasi sepenuhnya  

### Apakah direktif `max-age` diatur ke durasi yang memadai?
- [ ] Ya — `max-age` diatur ke satu tahun atau lebih (misalnya, `31536000`)  
- [ ] Ya — `max-age` diatur tetapi **terlalu singkat** (misalnya, kurang dari 6 bulan)  
- [ ] Tidak — direktif `max-age` **tidak ditemukan** atau diatur ke `0` *(Kritis)*  

### Apakah kebijakan HSTS mencakup semua subdomain?
- [ ] Ya — direktif `includeSubDomains` **diterapkan**  
- [ ] Tidak — direktif `includeSubDomains` **tidak ditemukan**, membuat subdomain rentan terhadap penurunan protokol (downgrade)  
- [ ] Tidak — fitur tidak berlaku karena tidak ada subdomain  

### Apakah aplikasi terdaftar untuk HSTS Preloading?
- [ ] Ya — direktif `preload` **tersedia** dan domain ada dalam daftar preload HSTS  
- [ ] Ya — direktif `preload` **tersedia** tetapi domain **belum** masuk dalam daftar preload  
- [ ] Tidak — direktif `preload` **tidak ditemukan**, memberikan celah untuk MitM pada kunjungan pertama  

### Apakah header HSTS salah dikirim melalui HTTP teks polos (plaintext)?
- [ ] Tidak — header **hanya** dikirim melalui koneksi HTTPS yang aman  
- [ ] Ya — header **dikirim** melalui HTTP, yang diabaikan oleh peramban dan dapat membocorkan detail konfigurasi  

---