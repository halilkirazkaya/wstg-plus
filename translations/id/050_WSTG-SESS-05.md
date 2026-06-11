## WSTG-SESS-05 — Testing for Cross Site Request Forgery

Cross-Site Request Forgery (CSRF) adalah kerentanan di mana penyerang menipu browser korban untuk melakukan tindakan yang tidak diinginkan pada situs web lain di mana korban sedang terautentikasi. Exploit ini memanfaatkan perilaku browser yang secara otomatis menyertakan kredensial "ambient", seperti session cookies atau header Authorization, ke dalam permintaan keluar. Penyerang biasanya menargetkan operasi yang mengubah status (state-changing operations) seperti mengubah kata sandi, memperbarui alamat email, atau mengeksekusi transfer keuangan dengan menghosting skrip berbahaya atau formulir tersembunyi di situs pihak ketiga. Eksploitasi yang berhasil dapat menyebabkan pengambilalihan akun secara penuh atau modifikasi data tanpa izin tanpa sepengetahuan atau persetujuan pengguna.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-SESS-05 |
| **CWE** | CWE-352 |
| **Status Pengujian** | Belum Dilakukan |
| **Severity** | Menengah / Tinggi* |

> *Severity menjadi Tinggi jika tindakan yang rentan memungkinkan pengambilalihan akun, eskalasi hak istimewa (privilege escalation), atau transaksi keuangan tanpa izin.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/05-Testing_for_Cross_Site_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/csrf-cross-site-request-forgery.html  
* https://portswigger.net/web-security/csrf  

**Tools:** `Burp Suite Professional (CSRF PoC Generator)`, `OWASP ZAP`, `CSRFTester`, `python3 (SimpleHTTPServer for PoC hosting)`

### Apakah token anti-CSRF diimplementasikan untuk permintaan yang mengubah status (state-changing requests)?
- [ ] Ya — token yang unik dan kuat secara kriptografis diperlukan untuk semua tindakan yang mengubah status  
- [ ] Ya — token tersedia tetapi **tidak** unik per sesi atau dapat diprediksi  
- [ ] Tidak — token anti-CSRF **tidak** diimplementasikan  

### Apakah validasi sisi server (server-side validation) dari token anti-CSRF sudah kuat?
- [ ] Ya — server memvalidasi token secara ketat dan bypass **tidak dimungkinkan**  
- [ ] Ya — validasi dilakukan tetapi **dapat** dilewati dengan menghapus parameter token  
- [ ] Ya — validasi dilakukan tetapi **dapat** dilewati dengan memberikan token kosong atau dummy  
- [ ] Ya — validasi dilakukan tetapi token **tidak** terikat dengan sesi pengguna  

### Apakah aplikasi mengandalkan metode yang mudah dilewati (bypassable) untuk perlindungan CSRF?
- [ ] Tidak — aplikasi tidak mengandalkan header yang lemah atau pemeriksaan origin saja  
- [ ] Ya — perlindungan hanya mengandalkan header `Referer` atau `Origin` yang **dapat** dipalsukan (spoofed) atau dihapus  
- [ ] Ya — perlindungan mengandalkan pemeriksaan header `X-Requested-With` yang **dapat** dilewati melalui kesalahan konfigurasi API atau CORS  

### Apakah session cookies dikonfigurasi dengan atribut `SameSite`?
- [ ] Ya — `SameSite` disetel ke `Strict` atau `Lax` pada semua cookie yang terkait dengan sesi  
- [ ] Tidak — atribut `SameSite` **hilang**, sehingga menggunakan perilaku bawaan masing-masing browser  
- [ ] Tidak — `SameSite` secara eksplisit disetel ke `None` tanpa flag `Secure` atau mitigasi tambahan  

### Apakah perlindungan CSRF dapat dilewati dengan mengubah metode permintaan HTTP?
- [ ] Tidak — perlindungan diterapkan tanpa memandang metode HTTP yang digunakan  
- [ ] Ya — token hanya divalidasi pada permintaan `POST`, tetapi tindakan tersebut **dapat** dilakukan melalui `GET`  
- [ ] Ya — mengubah metode (misalnya, menjadi `PUT` atau `DELETE`) dapat melewati pemeriksaan token  

---