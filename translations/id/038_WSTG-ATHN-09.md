## WSTG-ATHN-09 — Testing for Weak Password Change or Reset Functionalities

Mekanisme perubahan dan reset kata sandi adalah komponen kritis dari manajemen autentikasi yang, jika diimplementasikan dengan tidak tepat, memungkinkan penyerang mengambil alih akun pengguna. Kerentanan ini biasanya melibatkan reset token yang dapat diprediksi, kurangnya account lockouts selama upaya Brute Force, pengiriman token yang tidak aman, atau kemampuan untuk mengubah kata sandi tanpa memverifikasi kata sandi saat ini. Penyerang mengeksploitasi celah ini dengan mencegat atau menebak tautan pemulihan, melakukan Host Header Injection untuk mengalihkan email reset, atau memanfaatkan Session Fixation untuk mempertahankan akses setelah perubahan kata sandi. Memastikan proses ini memerlukan verifikasi yang kuat dan menggunakan keacakan kriptografis sangat penting untuk menjaga integritas akun dan mencegah akses yang tidak sah.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-ATHN-09 |
| **CWE** | CWE-640 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi / Kritis |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/09-Testing_for_Weak_Password_Change_or_Reset_Functionalities  
* https://hacktricks.wiki/en/pentesting-web/reset-password.html  

**Tools:** `Burp Suite (Repeater/Intruder)`, `Python (Custom Scripts)`, `CyberChef`, `OAST (Interactsh/Burp Collaborator)`

### Apakah kata sandi saat ini diperlukan untuk menetapkan kata sandi baru selama perubahan standar?
- [ ] Ya — kata sandi saat ini **diperlukan** dan diverifikasi  
- [ ] Ya — kata sandi saat ini **diperlukan** tetapi dapat di-bypass melalui Parameter Pollution atau manipulasi  
- [ ] No — kata sandi saat ini **tidak** diminta, memungkinkan pengambilalihan akun melalui Session Hijacking *(Tinggi)*  

### Apakah token reset kata sandi aman secara kriptografis dan tidak dapat diprediksi?
- [ ] Ya — token memiliki entropi tinggi, acak, dan **tidak dapat diprediksi**  
- [ ] Ya — token dihasilkan tetapi menunjukkan entropi rendah atau pola yang dapat diprediksi (misal: berbasis timestamp)  
- [ ] No — token **dapat diprediksi** atau didasarkan pada data pengguna statis seperti email/ID yang dienkode Base64 *(Kritis)*  

### Bisakah tautan reset kata sandi dialihkan melalui Host Header Injection?
- [ ] No — aplikasi mengabaikan atau memvalidasi header `Host` untuk pembuatan tautan  
- [ ] Ya — aplikasi menggunakan header `Host` atau `X-Forwarded-Host` untuk menyusun tautan, tetapi validasi **ada**  
- [ ] Ya — tautan **dapat** dialihkan ke domain yang dikendalikan penyerang melalui manipulasi header *(Tinggi)*  

### Apakah token reset memiliki siklus hidup yang terbatas dan pembatalan segera?
- [ ] Ya — token kedaluwarsa dengan cepat dan dibatalkan **segera** setelah digunakan atau perubahan kata sandi  
- [ ] Ya — token kedaluwarsa setelah durasi yang lama (misal: 24+ jam) atau memungkinkan **penggunaan berulang**  
- [ ] No — token **tidak pernah kedaluwarsa** atau tetap valid setelah kata sandi berhasil diubah  

### Apakah ada Rate Limiting atau penguncian pada endpoint reset dan perubahan kata sandi?
- [ ] Ya — Rate Limiting yang ketat dan CAPTCHA **diterapkan** untuk mencegah enumerasi dan Brute Force  
- [ ] Ya — kontrol terbatas tersedia tetapi bypass **mungkin dilakukan** melalui rotasi IP atau header spoofing  
- [ ] No — tidak ada Rate Limiting yang **diterapkan**, memungkinkan enumerasi akun massal atau Brute Force pada token  

---