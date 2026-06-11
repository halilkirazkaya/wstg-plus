## WSTG-IDNT-04 — Testing for Account Enumeration and Guessable User Account

Enumerasi Akun (Account Enumeration) terjadi ketika sebuah aplikasi mengungkapkan keberadaan atau ketidakberadaan akun pengguna melalui variasi dalam respons HTTP, pesan kesalahan, atau perbedaan waktu (timing differences) yang terukur. Penyerang mengeksploitasi perilaku ini dengan secara sistematis mengirimkan daftar nama pengguna untuk mengidentifikasi akun yang valid, yang selanjutnya ditargetkan untuk Credential Stuffing, serangan Brute Force, atau Social Engineering. Kerentanan ini umumnya muncul di portal login, fitur reset kata sandi, formulir registrasi, dan endpoint API yang menangani pengidentifikasi spesifik pengguna. Dari perspektif penyerang, enumerasi yang berhasil secara signifikan mempersempit permukaan serangan (attack surface) dengan mengubah upaya Brute Force buta menjadi eksploitasi terarah terhadap identitas valid yang telah diketahui.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-IDNT-04 |
| **CWE** | CWE-204 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Rendah / Menengah |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/04-Testing_for_Account_Enumeration_and_Guessable_User_Account  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite (Intruder/Comparer)`, `ffuf`, `Wfuzz`, `GoBuster`, `Hashcat`

### Apakah pesan kesalahan autentikasi konsisten di semua skenario?
- [ ] Ya — pesan kesalahan generik digunakan baik untuk nama pengguna yang tidak valid maupun kata sandi yang tidak valid  
- [ ] Ya — pesan serupa, tetapi terdapat sedikit variasi dalam tanda baca atau penggunaan huruf besar/kecil **ditemukan**  
- [ ] Tidak — pesan kesalahan secara eksplisit menyatakan "User not found" atau "Incorrect password" *(Rentan)*  

### Apakah alur reset kata sandi atau "forgot password" membocorkan keberadaan akun?
- [ ] Tidak — aplikasi mengembalikan pesan generik terlepas dari apakah email/username tersebut ada atau tidak  
- [ ] Ya — kontrol telah diterapkan, tetapi bypass **memungkinkan** melalui panjang respons atau kode status  
- [ ] Ya — aplikasi secara eksplisit mengonfirmasi jika email reset telah dikirim atau jika akun **tidak ditemukan**  

### Apakah proses registrasi terlindungi dari enumerasi pengguna?
- [ ] Tidak — registrasi tidak membocorkan informasi atau menggunakan CAPTCHA/Rate Limiting untuk mencegah pengecekan massal  
- [ ] Ya — kontrol telah diterapkan, tetapi bypass **memungkinkan** melalui perbedaan waktu atau respons API  
- [ ] Ya — aplikasi segera memberi tahu pengguna jika username atau email **sudah digunakan**  

### Apakah perbedaan waktu (timing differences) dapat diukur saat membandingkan username yang valid vs. tidak valid?
- [ ] Tidak — waktu respons konsisten terlepas dari validitas akun karena penggunaan perbandingan waktu konstan (constant-time comparisons)  
- [ ] Ya — terdapat perbedaan waktu tetapi terlalu kecil untuk diukur secara andal melalui jaringan  
- [ ] Ya — perbedaan waktu yang terukur **ditemukan** (misalnya, karena eksekusi hashing kata sandi hanya dilakukan untuk pengguna yang valid)  

### Apakah aplikasi menggunakan konvensi penamaan akun yang dapat diprediksi atau ditebak?
- [ ] Tidak — pengidentifikasi akun bersifat acak (UUID) atau ditentukan oleh pengguna dan kompleks  
- [ ] Ya — pengidentifikasi akun mengikuti pola (misalnya, `emp001`, `emp002`) dan enumerasi **memungkinkan**  
- [ ] Ya — pengidentifikasi berupa integer berurutan, sehingga memungkinkan enumerasi database secara penuh  

---