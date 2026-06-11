## WSTG-SESS-10 — Pengujian JSON Web Tokens

JSON Web Tokens (JWT) umumnya digunakan untuk manajemen sesi stateless dan propagasi identitas, namun keamanannya bergantung sepenuhnya pada implementasi algoritme penandatanganan (signing algorithms) yang tepat dan verifikasi klaim (claim) yang ketat. Penyerang sering kali mencoba melewati autentikasi dengan mengeksploitasi celah algoritme "none", melakukan Brute Force pada kunci rahasia HS256 yang lemah, atau melakukan serangan perpindahan algoritme (algorithm-switching) di mana kunci publik asimetris digunakan sebagai rahasia HMAC simetris. Kerentanan ini biasanya muncul pada cookie sesi atau header Authorization, yang berpotensi memungkinkan penyerang untuk melakukan eskalasi hak akses (privilege escalation) atau menyamar sebagai pengguna mana pun dengan memodifikasi klaim seperti `role` atau `user_id` tanpa merusak integritas kriptografi token tersebut.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-SESS-10 |
| **CWE** | CWE-347 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Tinggi / Kritis |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/10-Testing_JSON_Web_Tokens  
* https://hacktricks.wiki/en/pentesting-web/hacking-jwt-json-web-tokens.html  
* https://portswigger.net/web-security/jwt  

**Alat:** `jwt_tool`, `Burp Suite (JWT Editor Extension)`, `hashcat`, `john`, `jose`

### Apakah algoritme "none" diterima oleh server?
- [ ] Tidak — server **menolak** token yang menggunakan `alg: none` pada header  
- [ ] Ya — server **menerima** token dengan `alg: none` dan tanda tangan kosong *(Kritis)*  

### Bagaimana tanda tangan JWT diverifikasi dan dilindungi terhadap brute-force?
- [ ] Ya — kunci asimetris yang kuat (RS256/ES256) atau kunci rahasia HMAC dengan entropi tinggi digunakan  
- [ ] Ya — HS256 digunakan tetapi kunci rahasia **dapat** di-brute-force karena entropi yang rendah atau penggunaan nilai default  
- [ ] Tidak — verifikasi tanda tangan **tidak diterapkan** atau **dapat** dilewati melalui perpindahan algoritme/algorithm switching (dari RS256 ke HS256)  

### Apakah klaim standar (exp, nbf, iat) ditegakkan secara ketat oleh backend?
- [ ] Ya — `exp` (expiration) tersedia dan **tidak dapat** dilewati  
- [ ] Ya — `exp` tersedia tetapi server **tidak** menegakkannya selama proses validasi  
- [ ] Tidak — klaim kedaluwarsa **hilang** atau diabaikan, memungkinkan Session Replay tanpa batas waktu  

### Apakah implementasi mencegah injeksi kunci berbasis header (kid, jku, x5u)?
- [ ] Ya — header disanitasi dan hanya kunci/sumber internal tepercaya yang **diizinkan**  
- [ ] Tidak — header `kid` (Key ID) rentan terhadap Directory Traversal atau SQL Injection untuk merujuk ke berkas yang diketahui  
- [ ] Tidak — header `jku` atau `x5u` **dapat** diarahkan ke URL yang dikendalikan penyerang untuk memuat kunci berbahaya (malicious keys)  

---