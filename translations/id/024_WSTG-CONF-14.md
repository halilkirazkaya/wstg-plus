## WSTG-CONF-14 — Pengujian Kesalahan Konfigurasi Header Keamanan HTTP Lainnya

Pengujian untuk kesalahan konfigurasi (misconfiguration) header keamanan HTTP melibatkan verifikasi keberadaan dan implementasi header pertahanan yang tepat, yang memberikan perlindungan esensial terhadap vektor serangan berbasis web yang umum. Meskipun header ini tidak memperbaiki kerentanan kode yang mendasarinya, ketiadaannya secara signifikan meningkatkan risiko serangan man-in-the-middle (MITM), eksploitasi MIME-sniffing, dan kebocoran data lintas-asal (cross-origin) yang tidak disengaja melalui referrer. Dari sudut pandang penyerang, header keamanan yang hilang adalah indikator visibilitas tinggi dari lingkungan yang tidak diperkuat dengan baik (poorly hardened), yang sering kali memfasilitasi rantai eksploitasi yang lebih kompleks seperti session hijacking atau eksfiltrasi informasi. Konfigurasi ini biasanya dievaluasi pada tingkat server dan harus diterapkan secara konsisten di semua jenis respons, termasuk endpoint API dan sumber daya statis.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CONF-14 |
| **CWE** | CWE-16 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Rendah / Sedang |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/14-Test_Other_HTTP_Security_Header_Misconfigurations  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `curl`, `Burp Suite (Repeater/Scanner)`, `OWASP ZAP`, `Hardenize`, `securityheaders.com`

### Audit Keberadaan Header Keamanan
| Header | Ada | Tidak Ada | Konfigurasi yang Direkomendasikan |
|--------|:-------:|:-------:|---------------------------|
| `Strict-Transport-Security` |  |  | `max-age=63072000; includeSubDomains; preload` |
| `X-Content-Type-Options` |  |  | `nosniff` |
| `Referrer-Policy` |  |  | `no-referrer` atau `strict-origin-when-cross-origin` |
| `Permissions-Policy` |  |  | (Spesifik-fitur, misal, `geolocation=()`) |
| `Cross-Origin-Opener-Policy` |  |  | `same-origin` |

### Bagaimana header keamanan ditegakkan di seluruh cakupan aplikasi?
- [ ] Ya — header diterapkan secara global melalui load balancer pusat atau reverse proxy dan **tidak dapat** ditimpa (overridden)  
- [ ] Ya — header diterapkan pada respons dokumen utama tetapi **tidak ada** pada respons API atau aset statis  
- [ ] No — header diterapkan secara tidak konsisten atau **tidak ada** di seluruh domain aplikasi  

### Apakah header Strict-Transport-Security (HSTS) dikonfigurasi dengan benar?
- [ ] Ya — `max-age` diatur ke durasi yang lama (misal, 2 tahun) dan `includeSubDomains` **diaktifkan**  
- [ ] Ya — `max-age` diatur tetapi `includeSubDomains` **dinonaktifkan**, membiarkan subdomain rentan  
- [ ] No — header HSTS **tidak ada** atau `max-age` diatur ke 0, memungkinkan serangan protocol downgrade  

### Apakah Referrer-Policy mencegah kebocoran parameter URL yang sensitif?
- [ ] Ya — kebijakan diatur ke `no-referrer` atau `same-origin` *(Paling aman)*  
- [ ] Ya — kebijakan diatur ke `strict-origin-when-cross-origin`  
- [ ] No — kebijakan **tidak diatur** atau diatur ke `unsafe-url`, berpotensi membocorkan token atau PII sensitif dalam header referer  

### Apakah header X-Content-Type-Options mencegah MIME-sniffing?
- [ ] Ya — direktif `nosniff` **ada** dan diimplementasikan dengan benar  
- [ ] No — header **tidak ada**, berpotensi memungkinkan browser untuk mengeksekusi file yang tidak dapat dieksekusi (misal, gambar) sebagai skrip  

---