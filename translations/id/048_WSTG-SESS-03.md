## WSTG-SESS-03 — Session Fixation

Session fixation terjadi ketika aplikasi gagal membatalkan validitas atau merotasi identifier sesi (session identifier) setelah pengguna berhasil melakukan autentikasi, yang memungkinkan penyerang untuk memaksakan token sesi yang telah diketahui kepada korban. Jika aplikasi mempertahankan session ID pra-autentikasi setelah login, penyerang dapat memberikan tautan yang dibuat secara khusus berisi session ID tetap kepada korban dan selanjutnya membajak (hijack) sesi terautentikasi tersebut. Kerentanan ini biasanya muncul pada formulir login atau melalui identifier sesi yang diterima melalui parameter URL dan cookies. Dari sudut pandang penyerang, eksploitasi melibatkan tindakan "menetapkan" (fixing) sesi melalui tautan berbahaya atau kerentanan header injection dan menunggu korban memberikan kredensial, sehingga secara efektif melewati (bypassing) kebutuhan untuk mencuri token aktif.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-SESS-03 |
| **CWE** | CWE-384 |
| **Status Pengujian** | Belum Dilakukan |
| **Severity** | High |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/03-Testing_for_Session_Fixation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite (Proxy/Repeater)`, `OWASP ZAP`, `EditThisCookie`, `Curl`

### Apakah identifier sesi berubah setelah autentikasi berhasil?
- [ ] Ya — identifier sesi baru **diterbitkan** dan yang lama dibatalkan validitasnya *(Paling aman)*  
- [ ] Ya — identifier sesi baru **diterbitkan** tetapi yang lama **tetap valid**  
- [ ] Tidak — identifier sesi **tetap sama** sebelum dan sesudah autentikasi *(Kritis)*  

### Apakah aplikasi menerima identifier sesi yang diberikan melalui parameter URL?
- [ ] Tidak — session ID dikelola **secara eksklusif** melalui cookies  
- [ ] Ya — session ID diterima di URL tetapi **diabaikan** atau **dirotasi** saat login  
- [ ] Ya — session ID di URL **diterima** dan **dipertahankan** setelah autentikasi  

### Dapatkah session ID yang ditentukan penyerang dipaksakan ke dalam aplikasi?
- [ ] Tidak — aplikasi **menolak** session ID yang tidak valid atau tidak ada yang diberikan oleh pengguna  
- [ ] Ya — aplikasi **menerima** dan menginisialisasi sesi menggunakan ID sembarang yang diberikan dalam cookie  
- [ ] Ya — aplikasi **menerima** dan menginisialisasi sesi menggunakan ID sembarang yang diberikan dalam parameter URL  

### Apakah sesi pra-autentikasi dihentikan dengan benar?
- [ ] Ya — sesi anonim **dihancurkan sepenuhnya** saat peristiwa login terjadi  
- [ ] Tidak — sesi anonim **tidak dihancurkan**, memungkinkan potensi pemanenan sesi (session harvesting)  

### Apakah cookie sesi terlindungi dari injeksi sisi klien?
- [ ] Ya — flag `HttpOnly` dan `Secure` **diterapkan** untuk mencegah fixation berbasis skrip  
- [ ] Tidak — flag **tidak diterapkan**, memungkinkan session ID ditetapkan melalui Cross-Site Scripting (XSS)  

---