## WSTG-IDNT-03 — Pengujian Proses Penyediaan Akun (Account Provisioning)

Proses penyediaan akun (Account Provisioning) mengatur bagaimana identitas baru dibuat, diverifikasi, dan diberi hak istimewa dalam lingkungan aplikasi. Penyerang menargetkan alur kerja ini untuk melakukan pendaftaran massal otomatis untuk spam atau Denial-of-Service (DoS), melewati langkah verifikasi identitas untuk membuat akun palsu, atau memanipulasi parameter untuk memberikan diri mereka izin yang ditingkatkan selama fase pendaftaran. Kerentanan sering kali muncul dalam formulir pendaftaran mandiri (self-service), sistem khusus undangan (invitation-only), atau konsol penyediaan administratif di mana kelemahan logika bisnis (business logic flaws) memungkinkan pembuatan akun yang tidak sah atau eskalasi hak istimewa (Privilege Escalation). Dari sudut pandang penyerang, mengompromikan alur penyediaan memberikan pijakan yang sah yang dapat digunakan sebagai basis untuk eksploitasi lebih dalam, eksfiltrasi data, atau serangan rekayasa sosial (Social Engineering).


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-IDNT-03 |
| **CWE** | CWE-285 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Menengah / Tinggi* |

> *Tingkat keparahan menjadi Kritis jika penyerang dapat menyediakan akun administratif atau melewati batasan pendaftaran global.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/03-Test_Account_Provisioning_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite`, `FFUF`, `Postman`, `Python`, `MailHog`

### Apakah pendaftaran mandiri (self-registration) dikontrol secara ketat atau dibatasi hanya untuk pengguna yang berwenang?
- [ ] Ya — pendaftaran **dinonaktifkan** atau memerlukan persetujuan administratif manual  
- [ ] Ya — pendaftaran terbuka tetapi dibatasi untuk **domain email yang berwenang** atau token undangan  
- [ ] Tidak — pendaftaran **terbuka** untuk semua pengguna tanpa batasan domain atau token  

### Apakah mekanisme verifikasi identitas yang kuat (misalnya, email/SMS) diperlukan sebelum aktivasi akun?
- [ ] Ya — verifikasi **diperlukan** dan **tidak dapat** dilewati  
- [ ] Ya — verifikasi **diperlukan** tetapi **dapat** dilewati melalui manipulasi parameter (Parameter Tampering) atau token yang dapat diprediksi  
- [ ] Tidak — akun **langsung aktif** segera setelah pengiriman tanpa verifikasi apa pun  

### Dapatkah pengguna memanipulasi parameter peran (Role) atau izin selama permintaan pendaftaran?
- [ ] Tidak — peran **ditetapkan di sisi server (Server-Side)** dan tidak ada parameter di sisi klien (Client-Side)  
- [ ] Ya — parameter peran ada tetapi manipulasi **tidak dimungkinkan** karena adanya validasi sisi server  
- [ ] Ya — parameter peran/izin (misalnya, `role=admin`, `is_staff=true`) **dapat** dimodifikasi untuk mendapatkan akses yang ditingkatkan *(Kritis)*  

### Apakah pembatasan laju (Rate Limiting) atau kontrol anti-otomatisasi diterapkan untuk mencegah pembuatan akun massal?
- [ ] Ya — Rate Limiting dan CAPTCHA **aktif** dan efektif  
- [ ] Ya — kontrol tersedia tetapi bypass **dimungkinkan** melalui spoofing header atau layanan pemecah CAPTCHA  
- [ ] Tidak — tidak ada Rate Limiting atau kontrol anti-otomatisasi yang **diterapkan**  

### Apakah proses penyediaan membocorkan informasi tentang akun yang ada (User Enumeration)?
- [ ] No — pesan respons **identik** untuk pengguna yang sudah ada dan yang tidak ada  
- [ ] Yes — perbedaan respons atau variasi waktu **memungkinkan** dilakukannya enumerasi (User Enumeration) pengguna yang sudah ada  

---