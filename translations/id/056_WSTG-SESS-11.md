## WSTG-SESS-11 — Testing for Concurrent Sessions

Pengujian sesi konkuren (Concurrent Session) mengevaluasi apakah sebuah aplikasi mengizinkan satu akun pengguna untuk mempertahankan beberapa sesi aktif secara bersamaan di berbagai browser, perangkat, atau alamat IP. Kegagalan dalam membatasi sesi konkuren meningkatkan jendela peluang bagi penyerang untuk menggunakan token sesi yang dibajak atau kredensial yang disusupi tanpa memperingatkan pengguna yang sah atau menghentikan sesi yang ada. Perilaku ini biasanya dinilai dengan melakukan autentikasi dari berbagai sumber dan memantau stabilitas sesi, yang sering kali mengungkapkan kelemahan dalam logika manajemen sesi atau kurangnya aturan bisnis yang berfokus pada keamanan. Dari perspektif penyerang, kurangnya kontrol konkurensi memungkinkan persistensi tersembunyi setelah kompromi awal dan memfasilitasi serangan otomatis paralel.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-SESS-11 |
| **CWE** | CWE-613 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan (Severity)** | Low / Medium |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/11-Testing_for_Concurrent_Sessions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite (Repeater/Containers)`, `Firefox Multi-Account Containers`, `Postman`, `cURL`

### Apakah sesi konkuren dibatasi oleh kebijakan aplikasi?
- [ ] Ya — hanya satu sesi **diizinkan** per pengguna; login baru membatalkan sesi lama *(Paling aman)*  
- [ ] Ya — jumlah sesi konkuren yang tetap **diterapkan** dan tidak dapat dilampaui  
- [ ] Tidak — jumlah sesi konkuren yang tidak terbatas **mungkin dilakukan**  

### Bagaimana aplikasi merespons saat batas sesi tercapai?
- [ ] Sesi aktif tertua **dihentikan secara otomatis** (mitigasi session fixation/hijacking)  
- [ ] Pengguna **diminta** untuk menghentikan sesi yang ada sebelum sesi baru diizinkan  
- [ ] Upaya login baru **diblokir** hingga logout manual dilakukan dari perangkat lain  
- [ ] Tidak ada tindakan yang diambil — beberapa sesi tetap **aktif** secara bersamaan  

### Apakah aplikasi menyediakan antarmuka manajemen sesi untuk pengguna?
- [ ] Ya — pengguna **dapat** melihat sesi aktif (IP, perangkat, waktu) dan menghentikannya dari jarak jauh (Remote Terminate)  
- [ ] Ya — pengguna **dapat** melihat sesi aktif tetapi **tidak dapat** menghentikannya dari jarak jauh  
- [ ] Tidak — pengguna **tidak dapat** melihat atau mengelola sesi aktif lainnya yang terkait dengan akun mereka  

### Apakah pengguna diberi tahu saat aktivitas login konkuren terdeteksi?
- [ ] Ya — peringatan (email, SMS, atau dalam aplikasi) **dipicu** untuk login dari perangkat atau lokasi baru  
- [ ] Tidak — tidak ada notifikasi yang **diberikan** saat sesi konkuren dibuat  

---