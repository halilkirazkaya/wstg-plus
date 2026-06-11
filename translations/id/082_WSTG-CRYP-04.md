## WSTG-CRYP-04 — Testing for Weak Cryptographic Primitives

Primitif kriptografi yang lemah melibatkan penggunaan algoritma yang sudah usang atau tidak aman, seperti MD5, SHA-1, DES, atau RC4, yang rentan terhadap serangan kolisi (collision attacks), analisis frekuensi, atau dekripsi brute-force. Kelemahan ini biasanya terjadi pada hashing kata sandi, enkripsi data saat diam (data encryption at rest), pembuatan token sesi, atau verifikasi tanda tangan digital di dalam backend aplikasi. Dari sudut pandang penyerang, mengidentifikasi primitif ini memungkinkan kompromi terhadap integritas dan kerahasiaan data, seperti memecahkan hash yang dicegat untuk mendapatkan kata sandi teks biasa (cleartext) atau memalsukan token autentikasi. Aplikasi modern seharusnya menggunakan primitif yang sesuai standar industri, tahan kolisi (collision-resistant), dan memiliki entropi tinggi (high-entropy) seperti Argon2, Bcrypt, atau AES-GCM untuk memastikan perlindungan yang kuat terhadap kriptanalisis.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CRYP-04 |
| **CWE** | CWE-327 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/04-Testing_for_Weak_Cryptographic_Primitives  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `hashcat`, `John the Ripper`, `Burp Suite`, `CyberChef`, `OpenSSL`, `nmap (ssl-enum-ciphers)`

### Apakah algoritma hashing yang sudah usang seperti MD5 atau SHA-1 digunakan untuk penyimpanan data sensitif?
- [ ] Tidak — aplikasi menggunakan algoritma modern yang tahan kolisi seperti Argon2 atau Bcrypt  
- [ ] Ya — algoritma usang digunakan tetapi **hanya** untuk pemeriksaan integritas yang tidak sensitif terhadap keamanan  
- [ ] Ya — algoritma usang **digunakan** untuk kata sandi atau token sensitif *(Tinggi)*  

### Apakah cipher enkripsi simetris yang lemah seperti DES, 3DES, atau RC4 saat ini diterapkan?
- [ ] Tidak — AES (128-bit atau lebih tinggi) dalam mode aman seperti GCM atau CBC digunakan  
- [ ] Ya — cipher lama **diaktifkan** untuk kompatibilitas mundur tetapi **bukan** sebagai default  
- [ ] Ya — cipher lama adalah metode utama untuk melindungi data saat diam (at rest) atau saat transit  

### Apakah aplikasi mengimplementasikan logika kriptografi "roll-your-own" atau non-standar?
- [ ] Tidak — aplikasi secara ketat mematuhi pustaka kriptografi standar yang telah ditinjau sejawat (peer-reviewed)  
- [ ] Ya — terdapat wrapper khusus tetapi primitif yang mendasarinya **adalah** standar industri  
- [ ] Ya — algoritma kriptografi milik sendiri (proprietary) atau logika khusus **diterapkan** pada data sensitif *(Kritis)*  

### Apakah cryptographic salt dan initialization vector (IV) dikelola sesuai dengan praktik terbaik?
- [ ] Ya — salt unik per pengguna dan IV tidak dapat diprediksi untuk setiap operasi  
- [ ] Ya — salt digunakan tetapi **tidak** unik di seluruh rekaman data  
- [ ] Tidak — salt bersifat statis, hardcoded, atau tidak ada, dan IV **digunakan kembali** di beberapa sesi  

### Apakah panjang bit (bit-length) dari kunci asimetris (RSA, Diffie-Hellman) mencukupi untuk standar modern?
- [ ] Ya — kunci RSA/DH adalah 2048 bit atau lebih tinggi, atau Elliptic Curve (ECC) digunakan  
- [ ] Tidak — kunci kurang dari 2048 bit tetapi bypass **tidak** sepele (trivial)  
- [ ] Tidak — kunci 1024 bit atau lebih kecil, membuatnya **rentan** terhadap faktorisasi atau serangan komputasi  

---