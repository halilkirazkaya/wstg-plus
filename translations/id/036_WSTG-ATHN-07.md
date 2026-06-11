## WSTG-ATHN-07 — Pengujian untuk Kebijakan Kata Sandi yang Lemah

Pengujian untuk kebijakan kata sandi yang lemah melibatkan evaluasi terhadap persyaratan kompleksitas, panjang, dan entropi yang diterapkan oleh aplikasi selama pembuatan akun dan pembaruan kredensial. Kebijakan yang tidak memadai secara langsung membahayakan kerahasiaan akun pengguna dengan membuatnya rentan terhadap automated dictionary attacks, Brute Force attempts, dan Credential Stuffing. Kerentanan ini biasanya terdapat pada registration endpoints, password reset workflows, dan panel manajemen pengguna administratif. Dari sudut pandang penyerang, kebijakan yang permisif secara signifikan mengurangi biaya komputasi dan waktu yang diperlukan untuk berhasil mengompromikan kredensial menggunakan wordlist umum atau pattern-based cracking.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-ATHN-07 |
| **CWE** | CWE-521 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Menengah / Rendah* |

> *Tingkat keparahan menjadi Tinggi jika aplikasi tidak memiliki mekanisme account lockout atau Rate Limiting untuk mencegah penebakan otomatis.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/07-Testing_for_Weak_Authentication_Methods  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**Alat:** `Burp Suite (Intruder)`, `ZAP (Fuzzer)`, `Hydra`, `Hashcat`, `Cewl`

### Apakah panjang kata sandi minimum diterapkan oleh aplikasi?
- [ ] Ya — panjang minimum adalah 12+ karakter *(Paling aman)*  
- [ ] Ya — panjang minimum antara 8 hingga 11 karakter  
- [ ] Ya — panjang minimum **kurang dari** 8 karakter  
- [ ] Tidak — tidak ada panjang minimum yang diterapkan  

### Apakah persyaratan kompleksitas (huruf besar, huruf kecil, angka, simbol) diterapkan?
- [ ] Ya — beberapa kelas karakter bersifat wajib dan Exploit/Bypass **tidak dimungkinkan**  
- [ ] Ya — kelas karakter disarankan tetapi **tidak** diwajibkan  
- [ ] Tidak — tidak ada persyaratan kompleksitas yang diterapkan  

### Apakah aplikasi memblokir kata sandi umum/lemah atau kata sandi yang mengandung data spesifik pengguna?
- [ ] Ya — kata sandi lemah yang diketahui (misal: "password123") dan kata sandi berbasis username **tidak dapat** digunakan  
- [ ] Ya — kata sandi umum diblokir, tetapi data spesifik pengguna (misal: username, email) **dapat** digunakan  
- [ ] Tidak — string apa pun yang memenuhi persyaratan panjang/kompleksitas akan diterima  

### Dapatkah persyaratan kompleksitas atau panjang di-bypass melalui modifikasi client-side?
- [ ] Tidak — kebijakan diterapkan secara ketat pada **server-side**  
- [ ] Ya — kebijakan diterapkan melalui JavaScript dan **dapat** di-bypass dengan melakukan intersepsi request  

### Apakah kebijakan kata sandi dikomunikasikan dengan jelas kepada pengguna tanpa membocorkan detail implementasi?
- [ ] Ya — kebijakan terlihat saat input dan memberikan pesan kegagalan yang bersifat generik  
- [ ] Ya — kebijakan terlihat tetapi pesan kegagalan mengungkapkan regex tertentu atau celah logika  
- [ ] Tidak — kebijakan **tidak** terlihat, memaksa penemuan melalui trial-and-error  

---