## WSTG-CONF-09 — Test File Permission

Pengujian izin berkas (file permission) melibatkan audit terhadap tingkat kontrol akses yang diberikan pada berkas dan direktori di server web untuk memastikan bahwa sumber daya sensitif tidak terekspos secara berlebihan kepada pengguna atau proses yang tidak sah. Izin yang tidak dikonfigurasi dengan benar dapat memungkinkan penyerang untuk membaca berkas konfigurasi sensitif yang berisi kredensial basis data, melihat kode sumber (source code) eksklusif, atau memodifikasi skrip sisi server untuk mencapai Remote Code Execution (RCE). Kerentanan ini biasanya terjadi selama fase deployment ketika flag "world-readable" atau "world-writable" default dibiarkan pada direktori aplikasi yang kritis, seperti jalur konfigurasi, folder log, atau repositori unggahan. Dari sudut pandang penyerang, menemukan berkas yang tidak diamankan dengan benar sering kali menjadi katalis utama untuk eskalasi hak istimewa (privilege escalation) atau kompromi sistem secara penuh.


| Kolom | Nilai |
|---|---|
| **WSTG ID** | WSTG-CONF-09 |
| **CWE** | CWE-732 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Sedang / Tinggi* |

> *Tingkat keparahan menjadi Tinggi jika berkas konfigurasi sensitif (misalnya, `.env`, `web.config`) dapat dibaca atau jika akses tulis ke web root dimungkinkan.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/09-Test_File_Permission  
* https://hacktricks.wiki/en/linux-hardening/privilege-escalation/index.html  

**Tools:** `ls`, `find`, `LinPeas`, `WinPeas`, `ffuf`, `dirsearch`, `Nmap`

### Apakah berkas konfigurasi aplikasi yang sensitif terlindungi dari akses yang tidak sah?
- [ ] Ya — berkas konfigurasi (misalnya, `.env`, `config.php`) **tidak dapat diakses** melalui permintaan web  
- [ ] Ya — berkas tersedia tetapi akses **dibatasi** hanya untuk pengguna lokal yang berwenang  
- [ ] Tidak — berkas konfigurasi sensitif **dapat diakses** dan mengungkapkan kredensial atau rahasia  

### Dapatkah penyerang memodifikasi berkas atau direktori di dalam web root?
- [ ] Tidak — semua direktori web root bersifat **read-only** bagi pengguna web server  
- [ ] Ya — direktori yang dapat ditulis (writable) tersedia tetapi eksekusi skrip **dinonaktifkan**  
- [ ] Ya — direktori world-writable tersedia dan **memungkinkan** pengunggahan serta eksekusi skrip *(Kritis)*  

### Apakah metadata kontrol versi atau berkas cadangan terekspos karena izin yang longgar?
- [ ] Tidak — `.git`, `.svn`, dan berkas cadangan (misalnya, `.bak`, `~`) **tidak ada** atau terlindungi  
- [ ] Ya — direktori metadata tersedia tetapi directory listing **dinonaktifkan**  
- [ ] Ya — metadata atau cadangan sensitif **dapat diakses sepenuhnya**, memungkinkan pemulihan kode sumber  

### Apakah pengguna web server beroperasi dengan prinsip hak istimewa terendah (principle of least privilege)?
- [ ] Ya — proses web server berjalan sebagai pengguna **berhak istimewa rendah yang khusus (dedicated low-privilege)**  
- [ ] Tidak — proses web server berjalan dengan **hak istimewa yang tidak perlu** (misalnya, `root` atau `Administrator`)  

### Apakah berkas log diamankan dari pembacaan atau modifikasi yang tidak sah?
- [ ] Ya — berkas log **dibatasi** hanya untuk pengguna administratif  
- [ ] Ya — berkas log dapat dibaca tetapi **tidak** mengandung token sesi atau PII (Personally Identifiable Information)  
- [ ] Tidak — berkas log **dapat dibaca secara publik** dan mengungkapkan data transaksi sensitif atau informasi pengguna  

---