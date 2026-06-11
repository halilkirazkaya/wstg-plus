## WSTG-INPV-10 — IMAP SMTP Injection

Kerentanan IMAP dan SMTP Injection muncul ketika aplikasi web tidak menyaring data yang diberikan pengguna secara semestinya sebelum memasukkannya ke dalam perintah yang dikirim ke server email. Dengan menyuntikkan karakter Carriage Return dan Line Feed (CRLF), penyerang dapat menghentikan perintah yang dimaksud dan menyisipkan instruksi email sembarang, seperti menambahkan penerima tambahan (CC/BCC) atau memodifikasi isi pesan. Celah ini paling sering ditemukan pada gateway web-to-mail, formulir kontak, dan klien email yang berkomunikasi langsung dengan server email melalui protokol seperti IMAP4 atau SMTP. Eksploitasi yang berhasil memungkinkan pengiriman spam tanpa izin (unauthorized relaying), eksfiltrasi konten email yang sensitif, dan dalam beberapa kasus, kompromi total terhadap integritas server email.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INPV-10 |
| **CWE** | CWE-93 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/10-Testing_for_IMAP_SMTP_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite (Repeater/Intruder)`, `netcat`, `telnet`, `nmap`

### Apakah aplikasi memproses masukan yang diberikan pengguna untuk membangun perintah IMAP atau SMTP?
- [ ] Tidak — aplikasi **tidak** berinteraksi dengan server email melalui masukan pengguna  
- [ ] Ya — aplikasi berinteraksi dengan server email tetapi menggunakan API atau library yang aman  
- [ ] Ya — aplikasi membangun perintah email mentah menggunakan parameter yang dikendalikan pengguna  

### Apakah masukan divalidasi untuk mencegah penyuntikan urutan Carriage Return (`\r`) dan Line Feed (`\n`)?
- [ ] Ya — karakter CRLF disaring secara **ketat**, di-encode, atau ditolak  
- [ ] Ya — masukan divalidasi terhadap allowlist karakter yang ketat  
- [ ] Tidak — karakter CRLF **tidak** disaring, memungkinkan penghentian perintah dan injeksi  

### Bisakah penyerang menyuntikkan header email tambahan seperti `Bcc:` atau `Cc:`?
- [ ] Tidak — modifikasi header **tidak memungkinkan** karena batasan masukan yang ketat  
- [ ] Ya — penyuntikan header tambahan **memungkinkan** melalui urutan CRLF  
- [ ] Ya — penerusan email (mail relaying) ke domain eksternal **aktif** dan dapat dieksploitasi *(Kritis)*  

### Apakah aplikasi memungkinkan manipulasi perintah IMAP untuk mengakses kotak surat (mailbox) yang tidak sah?
- [ ] Tidak — aplikasi **tidak** menggunakan IMAP atau membatasi akses kotak surat secara ketat hanya untuk pengguna yang terautentikasi  
- [ ] Ya — IMAP command injection **memungkinkan** tetapi terbatas pada enumerasi metadata  
- [ ] Ya — IMAP command injection memungkinkan pembacaan atau penghapusan email pengguna lain  

### Apakah server email backend rentan terhadap command pipelining?
- [ ] Tidak — server email menolak perintah batch atau beberapa perintah dalam satu sesi  
- [ ] Ya — server email memproses beberapa perintah yang disuntikkan melalui satu bidang masukan (input field)  

---