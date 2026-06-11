## WSTG-CLNT-10 — Pengujian WebSockets

Pengujian WebSocket berfokus pada saluran komunikasi full-duplex yang dibangun antara klien dan server, yang melewati pola request-response HTTP tradisional. Pentester mengevaluasi keamanan proses handshake, keberadaan perlindungan Cross-Site WebSocket Hijacking (CSWSH), dan ketatnya validasi input yang diterapkan pada payload pesan. Eksploitasi biasanya melibatkan pembajakan sesi aktif untuk mengeksfiltrasi data secara real-time atau menyuntikkan payload berbahaya yang memicu kerentanan sisi server seperti Command Injection atau SQLi melalui socket yang persisten. Karena banyak Web Application Firewall (WAF) tradisional tidak memeriksa lalu lintas WebSocket secara efektif, protokol ini sering kali memberikan vektor tersembunyi untuk melewati pertahanan perimeter.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CLNT-10 |
| **CWE** | CWE-1385 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi / Sedang |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/10-Testing_WebSockets  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/websockets  

**Alat:** `Burp Suite (WebSockets tab)`, `OWASP ZAP`, `wscat`, `Socket.io-client`, `Wireshark`

### Apakah koneksi WebSocket dibangun melalui saluran yang aman?
- [ ] Ya — `wss://` (WebSocket Secure) diterapkan untuk semua koneksi  
- [ ] Tidak — `ws://` digunakan, memungkinkan penyadapan person-in-the-middle (PITM)  

### Apakah aplikasi terlindungi dari Cross-Site WebSocket Hijacking (CSWSH)?
- [ ] Tidak — server memvalidasi header `Origin` secara ketat selama handshake  
- [ ] Ya — validasi header `Origin` lemah atau **dapat** dilewati melalui origin null atau palsu (spoofed)  
- [ ] Ya — tidak ada validasi header `Origin` yang dilakukan, memungkinkan penyerang lintas situs untuk memulai koneksi *(Kritis)*  

### Apakah validasi dan sanitasasi input diterapkan pada payload pesan WebSocket?
- [ ] Ya — semua pesan masuk disanitasi dan divalidasi terhadap skema  
- [ ] Ya — beberapa validasi ada namun bypass **mungkin dilakukan** menggunakan payload yang dienkode atau non-standar  
- [ ] Tidak — pesan diproses secara langsung, memungkinkan injeksi (XSS, SQLi, RCE) atau manipulasi logika  

### Apakah pemeriksaan autentikasi dan otorisasi dilakukan pada setiap pesan WebSocket?
- [ ] Ya — sesi diverifikasi untuk setiap pesan atau protokol bersifat stateless dengan token  
- [ ] Ya — autentikasi terjadi pada saat handshake, tetapi otorisasi untuk tindakan tertentu **tidak** diterapkan per pesan  
- [ ] Tidak — autentikasi hanya diperiksa saat handshake, memungkinkan pembajakan sesi untuk memberikan akses penuh  

### Apakah server menerapkan pembatasan laju (Rate Limiting) atau batasan sumber daya pada WebSocket?
- [ ] Ya — Rate Limiting dan ukuran pesan maksimum **diaktifkan** dan diterapkan  
- [ ] Tidak — tidak ada batasan, memungkinkan Denial of Service (DoS) melalui pembanjiran pesan (message flooding) atau ukuran payload yang besar  

---