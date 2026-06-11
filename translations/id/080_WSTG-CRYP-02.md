## WSTG-CRYP-02 — Pengujian untuk Padding Oracle

Kerentanan Padding Oracle terjadi ketika proses dekripsi aplikasi membocorkan informasi mengenai apakah padding dari ciphertext yang didekripsi valid atau tidak valid. Dengan mengamati respons side-channel ini—seperti kode status HTTP yang berbeda, pesan kesalahan spesifik, atau variasi waktu respons (response timing variations)—seorang penyerang dapat mendekripsi data tanpa mengetahui kunci enkripsi dan berpotensi mengenkripsi ulang plaintext arbitrer. Kerentanan ini biasanya muncul pada session cookie yang terenkripsi, token autentikasi, atau parameter URL yang menggunakan block cipher dalam mode Cipher Block Chaining (CBC) dengan padding PKCS#7. Eksploitasi memungkinkan hilangnya kerahasiaan secara penuh dan dapat menyebabkan kompromi integritas melalui pembuatan blok terenkripsi palsu (forged encrypted blocks).


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CRYP-02 |
| **CWE** | CWE-209 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi / Kritis* |

> *Keparahan adalah Kritis jika kerentanan terdapat pada manajemen sesi, token identitas, atau enkripsi PII.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/02-Testing_for_Padding_Oracle  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `PadBuster`, `Burp Suite (Intruder/Padding Oracle Solver)`, `pkcs7-padbuster`, `CyberChef`

### Apakah block cipher digunakan dalam mode CBC untuk data sensitif?
- [ ] Tidak — aplikasi menggunakan authenticated encryption (AES-GCM/ChaCha20-Poly1305)  
- [ ] Ya — aplikasi menggunakan block cipher tetapi padding **tidak** diperlukan (misal: Stream cipher atau mode CTR)  
- [ ] Ya — aplikasi menggunakan mode CBC dan padding **diterapkan**  

### Apakah server mengungkapkan validitas padding melalui side channels?
- [ ] Tidak — server mengembalikan pesan kesalahan yang seragam dan waktu respons yang konsisten  
- [ ] Ya — server mengembalikan kode status HTTP yang berbeda (misal: 200 vs 500) untuk kesalahan padding  
- [ ] Ya — server mengembalikan pesan kesalahan spesifik (misal: "Invalid Padding", "Decryption failed")  
- [ ] Ya — server menunjukkan perbedaan waktu yang terukur selama kegagalan dekripsi  

### Apakah dekripsi otomatis dari ciphertext memungkinkan?
- [ ] Tidak — side channels **tidak** cukup stabil untuk eksploitasi  
- [ ] Ya — ciphertext **dapat** didekripsi secara parsial (beberapa blok terakhir)  
- [ ] Ya — pemulihan plaintext secara penuh **memungkinkan** menggunakan alat seperti `PadBuster`  

### Apakah penyerang dapat melakukan bit-flipping atau memalsukan ciphertext yang valid?
- [ ] Tidak — pemeriksaan integritas (HMAC) diverifikasi **sebelum** dekripsi  
- [ ] Ya — tidak ada pemeriksaan integritas, tetapi konstruksi payload **tidak** layak  
- [ ] Ya — konstruksi ciphertext arbitrer **memungkinkan**, yang mengizinkan eskalasi hak istimewa (privilege escalation) atau injeksi data  

---