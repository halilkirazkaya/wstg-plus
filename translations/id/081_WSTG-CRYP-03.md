## WSTG-CRYP-03 — Testing for Sensitive Information Sent Via Unencrypted Channels

Transmisi data sensitif melalui saluran yang tidak terenkripsi, seperti HTTP biasa, memaparkan informasi terhadap intersepsi dan modifikasi melalui serangan Man-in-the-Middle (MITM). Kerentanan ini biasanya terjadi ketika aplikasi web gagal menerapkan Transport Layer Security (TLS) di seluruh endpoint, terutama pada komponen legacy, formulir login, atau selama transisi awal dari HTTP ke HTTPS. Dari perspektif penyerang, hal ini memungkinkan sniffing pasif terhadap kredensial autentikasi, session tokens, dan personally identifiable information (PII) pada segmen jaringan yang terkompromi atau tidak tepercaya seperti Wi-Fi publik. Memastikan bahwa semua data sensitif dienkapsulasi dalam tunnel TLS yang kuat sangat penting untuk menjaga kerahasiaan dan integritas interaksi pengguna serta mencegah session hijacking.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CRYP-03 |
| **CWE** | CWE-319 |
| **Status Pengujian** | Belum Dilakukan |
| **Severity** | Tinggi / Sedang |

> *Severity menjadi Tinggi jika kredensial autentikasi, session identifiers, atau PII ditransmisikan dalam bentuk cleartext.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/03-Testing_for_Sensitive_Information_Sent_via_Unencrypted_Channels  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Wireshark`, `tcpdump`, `Burp Suite (Proxy/Alerts)`, `nmap`, `sslyze`, `testssl.sh`

### Apakah aplikasi mengizinkan komunikasi melalui HTTP yang tidak terenkripsi untuk endpoint sensitif?
- [ ] Tidak — seluruh lalu lintas aplikasi secara ketat dipaksa melalui HTTPS *(Paling aman)*  
- [ ] Ya — halaman non-sensitif dapat diakses melalui HTTP tetapi endpoint sensitif **tidak**  
- [ ] Ya — endpoint sensitif (login, checkout, profil) **dapat** diakses melalui HTTP yang tidak terenkripsi *(Risiko Tinggi)*  

### Apakah terdapat pengalihan global dari HTTP ke HTTPS?
- [ ] Ya — aplikasi melakukan pengalihan 301/302 ke HTTPS untuk semua permintaan HTTP  
- [ ] Ya — pengalihan hanya dilakukan untuk endpoint sensitif tertentu  
- [ ] Tidak — tidak ada pengalihan dari HTTP ke HTTPS yang **diterapkan**  

### Apakah HTTP Strict Transport Security (HSTS) diimplementasikan?
- [ ] Ya — HSTS **diaktifkan** dengan `max-age` yang lama dan menyertakan direktif `includeSubDomains` serta `preload`  
- [ ] Ya — HSTS **diaktifkan** tetapi tidak memiliki direktif `includeSubDomains` atau `preload`  
- [ ] Tidak — HSTS **tidak diaktifkan** pada server aplikasi  

### Apakah cookie sensitif dilindungi dari transmisi cleartext?

| Nama Cookie | Flag Secure Ada | Flag Secure Tidak Ada |
|-------------|:---------------:|:--------------------:|
| `SessionID` |                 |                      |
| `AuthToken` |                 |                      |
| `CSRF-Token`|                 |                      |

### Apakah aplikasi mentransmisikan data sensitif ke API pihak ketiga melalui saluran yang tidak terenkripsi?
- [ ] Tidak — semua panggilan API eksternal dilakukan melalui tunnel terenkripsi (HTTPS)  
- [ ] Ya — beberapa telemetri non-sensitif dikirim melalui HTTP  
- [ ] Ya — API keys, PII, atau kredensial **dikirim** ke pihak ketiga melalui HTTP yang tidak terenkripsi  

---