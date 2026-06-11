## WSTG-SESS-08 — Session Puzzling

Session Puzzling, yang juga dikenal sebagai Session Variable Overloading, terjadi ketika aplikasi menggunakan variabel sesi yang sama untuk berbagai tujuan di modul yang berbeda atau gagal menghapus status sesi dengan benar selama transisi alur kerja (workflow). Penyerang mengeksploitasi perilaku ini dengan mengisi variabel sesi dalam satu konteks—seperti pratinjau profil yang belum terautentikasi atau pendaftaran multi-langkah—lalu menavigasi ke area yang dilindungi di mana aplikasi secara keliru memercayai variabel yang sudah ada tersebut. Hal ini dapat menyebabkan dampak kritis termasuk Authentication Bypass, Privilege Escalation, atau akses tidak sah ke data sensitif dengan menipu aplikasi agar meyakini bahwa sesi berada dalam status hak akses yang lebih tinggi daripada yang sebenarnya. Kerentanan ini paling sering ditemukan pada aplikasi stateful yang kompleks di mana logika Manajemen Sesi (Session Management) diterapkan secara tidak konsisten di berbagai komponen fungsional.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-SESS-08 |
| **CWE** | CWE-621 |
| **Status Pengujian** | Belum Dilakukan |
| **Severitas** | Tinggi / Kritis |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/08-Testing_for_Session_Puzzling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite (Repeater/Comparator)`, `OWASP ZAP`, `Postman`, `Cookie Editor`

### Apakah aplikasi menggunakan nama variabel sesi yang sama untuk tujuan berbeda di modul yang terpisah?
- [ ] Tidak — nama variabel unik atau cakupannya terisolasi  
- [ ] Ya — terdapat nama variabel yang sama tetapi status dihapus di antara modul  
- [ ] Ya — terdapat nama variabel yang sama dan status **tidak** dihapus  

### Dapatkah tindakan tanpa autentikasi memengaruhi variabel sesi yang digunakan dalam konteks terautentikasi?
- [ ] Tidak — variabel sesi diinisialisasi hanya **setelah** autentikasi berhasil  
- [ ] Ya — variabel sesi dapat diatur sebelum login tetapi **tidak** memengaruhi status pasca-login  
- [ ] Ya — variabel sesi yang diatur selama pra-autentikasi **dipercayai** pasca-autentikasi *(Kritis)*  

### Apakah aplikasi melakukan inisialisasi ulang pengenal sesi (Session ID) dan variabel terkait saat terjadi perubahan tingkat hak akses?
- [ ] Ya — sesi direset sepenuhnya dan ID baru **diterbitkan**  
- [ ] Parsial — ID baru diterbitkan tetapi beberapa variabel sesi **tetap ada**  
- [ ] Tidak — ID sesi dan semua variabel **tetap** tidak berubah  

### Dapatkah hak administratif diperoleh dengan memanipulasi variabel sesi melalui akun tanpa hak khusus (unprivileged account)?
- [ ] Tidak — variabel hak akses **tidak dapat** dimodifikasi oleh pengguna  
- [ ] Ya — variabel dapat dimodifikasi tetapi aplikasi **memvalidasinya** terhadap database backend  
- [ ] Ya — variabel dapat dimodifikasi dan aplikasi **memercayai** status sesi secara implisit *(Kritis)*  

### Apakah status sesi segera dihapus setelah menyelesaikan alur kerja tertentu (misalnya, reset kata sandi, checkout)?
- [ ] Ya — variabel sesi untuk alur kerja dihancurkan setelah selesai  
- [ ] Tidak — variabel alur kerja **tetap ada** dan dapat digunakan kembali di area aplikasi lainnya  

---