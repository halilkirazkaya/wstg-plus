## WSTG-BUSL-07 — Test Defenses Against Application Misuse

Pertahanan terhadap penyalahgunaan aplikasi mengevaluasi kemampuan aplikasi untuk mengidentifikasi, mencatat (log), dan menanggapi perilaku pengguna yang anomali yang menyimpang dari logika bisnis yang dimaksudkan. Berbeda dengan keamanan berbasis tanda tangan (signature-based security) tradisional, pertahanan ini berfokus pada deteksi pola seperti permintaan bertubi-tubi (rapid-fire requests), Credential Stuffing, atau enumerasi sumber daya secara berurutan yang menandakan penyalahgunaan otomatis. Dari sudut pandang penyerang, pengujian ini menentukan ketahanan aplikasi terhadap otomatisasi dan serangan "low and slow" yang dirancang untuk mengeksfiltrasi data atau menguras sumber daya tanpa memicu peringatan keamanan standar. Kegagalan dalam menerapkan pertahanan ini sering kali mengakibatkan pengerukan data (data scraping) secara masif, pengambilalihan akun (Account Takeover), atau Denial of Service (DoS) melalui fitur aplikasi yang sah namun intensif sumber daya.


| Kolom | Nilai |
|---|---|
| **WSTG ID** | WSTG-BUSL-07 |
| **CWE** | CWE-799 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Medium / High |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/07-Test_Defenses_Against_Application_Misuse  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite (Intruder/Turbo Intruder)`, `OWASP ZAP`, `Python (Requests/Selenium)`, `Caido`, `K6`

### Apakah mekanisme rate-limiting atau throttling diterapkan pada endpoint sensitif?
- [ ] Ya — Rate Limiting yang ketat **diterapkan** dan ditegakkan di semua endpoint sensitif *(Paling aman)*  
- [ ] Ya — Rate Limiting **diterapkan** tetapi dapat dilewati melalui rotasi IP atau manipulasi header  
- [ ] Ya — Rate Limiting **diterapkan** hanya pada endpoint autentikasi, membiarkan yang lain terpapar  
- [ ] No — Tidak ada Rate Limiting atau throttling yang **diterapkan** pada fitur aplikasi apa pun  

### Apakah aplikasi mendeteksi dan memblokir pola interaksi otomatis?
- [ ] Ya — analisis perilaku atau CAPTCHA **diaktifkan** dan secara efektif memblokir bot  
- [ ] Ya — anti-otomatisasi **diaktifkan** tetapi bypass **mungkin dilakukan** menggunakan headless browser atau session cycling  
- [ ] No — aplikasi **tidak dapat** membedakan antara pengguna manusia dan skrip otomatis  

### Apakah perilaku anomali dicatat dan diikuti dengan respons defensif?
- [ ] Ya — penyalahgunaan memicu pemblokiran otomatis dan memperingatkan tim keamanan  
- [ ] Ya — penyalahgunaan dicatat untuk audit tetapi **tidak ada** tindakan defensif otomatis yang diambil  
- [ ] No — aplikasi **tidak mencatat** atau menanggapi pola aktivitas yang tidak biasa  

### Apakah pertahanan dapat dilewati dengan memanipulasi pengidentifikasi sisi klien atau header?
- [ ] No — pertahanan bergantung pada state di sisi server dan telemetri yang terverifikasi  
- [ ] Ya — kontrol **dapat** dilewati dengan menghapus cookie atau merotasi string `User-Agent`  
- [ ] Ya — kontrol **dapat** dilewati dengan menyisipkan header seperti `X-Forwarded-For` atau `X-Real-IP`  

---