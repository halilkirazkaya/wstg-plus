## WSTG-IDNT-02 — Menguji Proses Registrasi Pengguna

Menguji proses registrasi pengguna melibatkan evaluasi alur kerja di mana identitas baru dibuat untuk memastikan proses tersebut tidak dapat disalahgunakan untuk akses yang tidak sah atau eksploitasi otomatis. Kerentanan dalam proses ini sering kali bermanifestasi sebagai kurangnya verifikasi identitas (email/SMS), kerentanan terhadap pembuatan akun massal melalui bot, atau pengungkapan informasi melalui pesan kesalahan yang bertele-tele yang mengungkapkan nama pengguna yang sudah ada. Penyerang mengeksploitasi celah ini untuk melakukan User Enumeration, melakukan kampanye spam skala besar, atau memanipulasi parameter registrasi untuk mendapatkan hak akses yang lebih tinggi (Privilege Escalation) selama fase pembuatan akun. Pengujian ini sangat penting untuk melindungi integritas aplikasi dan mencegah penyerang mengisi sistem dengan akun palsu atau berbahaya.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-IDNT-02 |
| **CWE** | CWE-836 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Sedang |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/02-Test_User_Registration_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite`, `OWASP ZAP`, `Python`, `ffuf`, `Cuppa`

### Apakah verifikasi identitas diperlukan sebelum akun baru menjadi aktif?
- [ ] Ya — registrasi memerlukan token unik dengan batas waktu yang dikirim melalui email atau SMS  
- [ ] Ya — verifikasi diperlukan tetapi token bersifat **lemah** atau **dapat diprediksi**  
- [ ] Tidak — akun diaktifkan secara **langsung** tanpa langkah verifikasi apa pun  

### Apakah proses registrasi mencegah User Enumeration?
- [ ] Ya — aplikasi mengembalikan respons yang **identik** baik untuk email/username baru maupun yang sudah ada  
- [ ] Tidak — pesan kesalahan (misalnya, "Email sudah digunakan") **memungkinkan** penyerang untuk memetakan pengguna yang terdaftar  
- [ ] Tidak — perbedaan waktu dalam respons server **mengungkapkan** apakah seorang pengguna ada atau tidak  

### Apakah kontrol anti-otomatisasi diterapkan untuk mencegah registrasi massal?
- [ ] Ya — mekanisme CAPTCHA atau proof-of-work **tersedia** dan **efektif**  
- [ ] Ya — kontrol tersedia tetapi bypass **mungkin dilakukan** melalui endpoint API atau manipulasi header  
- [ ] Tidak — tidak ada Rate Limiting atau CAPTCHA yang **diterapkan** pada endpoint registrasi  

### Apakah parameter registrasi dapat dimanipulasi untuk meningkatkan hak akses?
- [ ] Tidak — peran pengguna ditetapkan **secara ketat** oleh logika sisi server  
- [ ] Ya — parameter seperti `role`, `admin`, atau `group_id` **dapat** dimodifikasi dalam request untuk mendapatkan akses yang lebih tinggi  

### Apakah kebijakan kata sandi (Password Policy) ditegakkan selama fase registrasi?
- [ ] Ya — persyaratan kata sandi yang kuat **ditegakkan** di sisi server  
- [ ] Tidak — kata sandi yang lemah (misalnya, "password123") **diterima** oleh sistem  

---