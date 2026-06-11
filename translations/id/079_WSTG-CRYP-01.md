## WSTG-CRYP-01 — Pengujian Keamanan Transport Layer yang Lemah

Pengujian untuk keamanan transport layer yang lemah melibatkan analisis implementasi dan konfigurasi SSL/TLS untuk memastikan kerahasiaan dan integritas data selama transit. Penyerang menargetkan kesalahan konfigurasi seperti protokol yang usang (SSLv2, TLS 1.0), cipher suite yang lemah (NULL, RC4, atau anonymous), dan sertifikat yang tidak valid atau ditandatangani dengan lemah untuk melakukan serangan Man-in-the-Middle (MitM). Pengujian ini biasanya menargetkan endpoint web server atau load balancer aplikasi di mana komunikasi terenkripsi dibangun. Eksploitasi yang berhasil memungkinkan penyerang untuk mencegat data sensitif, membajak sesi pengguna (session hijacking), atau menurunkan koneksi ke status tidak aman melalui penurunan versi protokol (protocol downgrade) atau dekripsi lalu lintas.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CRYP-01 |
| **CWE** | CWE-326 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Tinggi / Sedang* |

> *Tingkat keparahan adalah Tinggi jika data sensitif (PII, kredensial, token sesi) ditransmisikan; Sedang untuk lalu lintas informasi umum.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/01-Testing_for_Weak_Transport_Layer_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `testssl.sh`, `sslyze`, `nmap`, `OpenSSL`, `Qualys SSL Labs`

### Apakah protokol TLS/SSL yang usang atau tidak aman didukung?
- [ ] Tidak — hanya TLS 1.2 dan TLS 1.3 yang **diaktifkan**  
- [ ] Ya — TLS 1.0 atau TLS 1.1 **diaktifkan**  
- [ ] Ya — SSLv2 atau SSLv3 **diaktifkan** *(Kritis)*  

### Apakah cipher suite yang lemah atau tidak aman diterima oleh server?
- [ ] Tidak — hanya cipher yang kuat dan modern (misal, AES-GCM, ChaCha20) yang **didukung**  
- [ ] Ya — cipher dengan enkripsi lemah (misal, 3DES, RC4) atau panjang bit rendah **didukung**  
- [ ] Ya — cipher NULL, anonymous (ADH), atau export **didukung** *(Kritis)*  

### Apakah sertifikat server valid dan tepercaya?
- [ ] Ya — sertifikat valid, sesuai dengan domain, dan ditandatangani oleh **CA tepercaya**  
- [ ] Tidak — sertifikat telah **kedaluwarsa** atau menggunakan **algoritma tanda tangan yang lemah** (misal, SHA-1)  
- [ ] Tidak — sertifikat **ditandatangani sendiri (self-signed)** atau terjadi **ketidaksesuaian (mismatch)** nama domain  

### Apakah server mendukung Secure Renegotiation dan mencegah serangan Downgrade?
- [ ] Ya — Secure Renegotiation **didukung** dan TLS Fallback SCSV **diaktifkan**  
- [ ] Tidak — Secure Renegotiation **tidak didukung**, berpotensi memungkinkan injeksi MitM  
- [ ] Tidak — Server rentan terhadap serangan **POODLE** atau **ROBOT**  

### Apakah HTTP Strict Transport Security (HSTS) diimplementasikan dengan benar?

| Header | Ada | Tidak Ada |
|--------|:-------:|:-------:|
| `Strict-Transport-Security` |  |  |

- [ ] Ya — header HSTS ada dengan `max-age` yang panjang dan `includeSubDomains` **diaktifkan**  
- [ ] Ya — header HSTS ada tetapi tidak ada `includeSubDomains` atau memiliki **max-age yang pendek**  
- [ ] Tidak — header HSTS **tidak ada**  

---