## WSTG-ATHN-08 — Pengujian Jawaban Pertanyaan Keamanan yang Lemah

Pengujian terhadap jawaban pertanyaan keamanan yang lemah melibatkan evaluasi mekanisme Knowledge-Based Authentication (KBA) yang digunakan untuk memverifikasi identitas pengguna, biasanya selama alur pemulihan kata sandi atau Multi-Factor Authentication (MFA). Hal ini penting karena pertanyaan keamanan sering kali bergantung pada informasi statis dan tidak rahasia yang dapat dengan mudah ditemukan melalui Open-Source Intelligence (OSINT) atau Social Engineering, yang menyebabkan Account Takeover yang tidak sah. Kerentanan ini biasanya terjadi pada modul "Lupa Kata Sandi" atau "Buka Kunci Akun" di mana pengguna memberikan jawaban atas pertanyaan yang telah ditentukan sebelumnya. Dari sudut pandang penyerang, ini adalah target utama untuk Brute Force otomatis atau riset yang ditargetkan, karena banyak pengguna memberikan jawaban yang dapat diprediksi atau dapat diverifikasi secara publik untuk pertanyaan umum seperti "Siapa nama gadis ibu kandung Anda?" atau "Di SMA mana Anda bersekolah?".


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-ATHN-08 |
| **CWE** | CWE-640 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Sedang / Tinggi* |

> *Keparahan menjadi Tinggi jika pertanyaan keamanan merupakan satu-satunya syarat untuk reset kata sandi penuh dan Account Takeover tanpa verifikasi lebih lanjut.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/08-Testing_for_Weak_Security_Question_Answer  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**Alat:** `Burp Suite (Intruder)`, `ffuf`, `Maltego`, `Sherlock`, `Social Engineering Toolkit (SET)`

### Apakah aplikasi menerapkan pertanyaan keamanan untuk verifikasi identitas atau pemulihan kata sandi?
- [ ] Tidak — pertanyaan keamanan **tidak digunakan** untuk autentikasi atau pemulihan  
- [ ] Ya — pertanyaan keamanan **digunakan** sebagai faktor sekunder opsional  
- [ ] Ya — pertanyaan keamanan **digunakan** sebagai mekanisme pemulihan utama/satu-satunya *(Risiko Tinggi)*  

### Apakah pertanyaan keamanan dipilih dari daftar tetap atau ditentukan oleh pengguna?
- [ ] Tidak — pertanyaan sepenuhnya ditentukan oleh pengguna dan memerlukan entropi tinggi  
- [ ] Ya — pertanyaan bersifat tetap tetapi mencakup opsi yang tidak dapat ditemukan melalui OSINT  
- [ ] Ya — pertanyaan berasal dari daftar tetap topik umum yang dapat ditemukan melalui OSINT *(Rentan)*  

### Apakah Rate Limiting atau Account Lockout diterapkan pada upaya menjawab pertanyaan keamanan?
- [ ] Ya — Rate Limiting ketat dan Account Lockout **diterapkan** untuk mencegah Brute Force  
- [ ] Ya — Rate Limiting **diterapkan** tetapi bypass **dimungkinkan** melalui rotasi IP atau manipulasi header  
- [ ] Tidak — **tidak ada Rate Limiting** yang diberlakukan pada upaya menjawab  

### Apakah aplikasi memberlakukan kompleksitas atau validasi pada konten jawaban?
- [ ] Ya — validasi mencegah kata-kata umum dan memastikan kompleksitas/keunikan jawaban  
- [ ] Ya — validasi dasar tersedia tetapi **dapat** di-bypass dengan wordlist umum atau Dictionary Attack  
- [ ] Tidak — **tidak ada validasi** yang dilakukan; jawaban sederhana atau satu karakter **diizinkan**  

### Bisakah tantangan pertanyaan keamanan di-bypass melalui celah logika (Logical Flaws)?
- [ ] Tidak — alur pemulihan secara ketat memerlukan jawaban yang valid untuk melanjutkan  
- [ ] Ya — alur pemulihan **dapat** di-bypass dengan memanipulasi kode respons HTTP atau parameter sesi  
- [ ] Ya — jawaban tercermin dalam kode sumber atau hidden fields pada halaman  

---