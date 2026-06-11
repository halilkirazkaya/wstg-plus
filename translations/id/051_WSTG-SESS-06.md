## WSTG-SESS-06 — Pengujian Fungsionalitas Logout

Fungsionalitas logout adalah kontrol keamanan kritis yang dirancang untuk mengakhiri sesi pengguna dan membatalkan pengenal sesi (session identifiers) yang terkait baik di sisi klien maupun server. Kegagalan dalam mengimplementasikan logout dengan benar memungkinkan serangan Session Fixation atau hijacking, karena penyerang yang mendapatkan akses ke mesin setelah pengguna melakukan "logout" berpotensi menggunakan kembali token sesi yang masih aktif. Pentester mengevaluasi hal ini dengan memeriksa apakah cookie sesi dihapus dari browser, apakah status sesi di sisi server dihancurkan secara eksplisit, dan apakah navigasi tombol kembali (back-button) memungkinkan akses ke data sensitif yang tersimpan dalam cache. Implementasi logout yang aman memastikan bahwa setelah tindakan tersebut dipicu, tidak ada permintaan berikutnya yang menggunakan token sesi lama yang diizinkan oleh aplikasi.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-SESS-06 |
| **CWE** | CWE-613 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Sedang |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/06-Testing_for_Logout_Functionality  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Alat:** `Burp Suite (Repeater/Proxy)`, `Browser Developer Tools`, `OWASP ZAP`

### Apakah mekanisme logout yang fungsional tersedia dan dapat diakses?
- [ ] Ya — tombol logout tersedia dan memicu permintaan pengakhiran (termination request)  
- [ ] Tidak — tombol logout **tidak ada** atau **tidak** memicu kejadian pengakhiran  

### Apakah pengenal sesi (session identifier) dibatalkan validitasnya di sisi server?
- [ ] Ya — server menolak semua permintaan berikutnya yang menggunakan token sesi lama  
- [ ] Tidak — server **terus menerima** token sesi lama setelah logout  

### Apakah aplikasi menghapus cookie sesi di browser saat logout?
- [ ] Ya — cookie ditimpa dengan tanggal kedaluwarsa atau nilai kosong  
- [ ] Ya — cookie tetap ada tetapi **tidak lagi valid** di server  
- [ ] Tidak — cookie tetap ada di browser dan **mempertahankan** nilai aslinya  

### Dapatkah konten terautentikasi yang sensitif diakses melalui tombol 'Kembali' (Back) browser setelah logout?
- [ ] Tidak — header `Cache-Control: no-store` atau serupa mencegah penampilan halaman sensitif setelah logout  
- [ ] Ya — halaman sensitif tersimpan dalam cache dan **dapat** dilihat melalui navigasi setelah sesi diakhiri  

---