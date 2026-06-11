## WSTG-SESS-07 — Menguji Batas Waktu Sesi (Session Timeout)

Pengujian batas waktu sesi (session timeout) mengevaluasi apakah sebuah aplikasi secara efektif mengakhiri sesi pengguna setelah periode ketidakaktifan yang ditentukan sebelumnya atau durasi total. Kontrol ini sangat penting untuk memitigasi risiko pembajakan sesi (session hijacking), terutama pada workstation bersama atau dalam skenario di mana penyerang menangkap pengidentifikasi sesi (session identifier) melalui intersepsi jaringan atau XSS. Pentester menganalisis baik idle timeout, yang dipicu setelah periode ketidakaktifan, maupun absolute timeout, yang membatasi total masa pakai sesi terlepas dari aktivitasnya. Dari sudut pandang penyerang, sesi yang tidak terbatas atau terlalu lama memberikan jendela peluang yang diperpanjang untuk mempertahankan akses yang tidak sah dan melewati kebutuhan untuk autentikasi ulang (re-authentication).


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-SESS-07 |
| **CWE** | CWE-613 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Sedang |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/07-Testing_Session_Timeout  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Browser DevTools`

### Apakah aplikasi menerapkan idle session timeout?
- [ ] Ya — sesi kedaluwarsa setelah periode ketidakaktifan yang singkat (misalnya, 15-30 menit)  
- [ ] Ya — sesi kedaluwarsa tetapi durasi timeout **terlalu lama** (misalnya, > 24 jam)  
- [ ] Tidak — sesi **tetap aktif** tanpa batas waktu selama ketidakaktifan  

### Apakah batas waktu sesi (session timeout) diterapkan di sisi server (server-side)?
- [ ] Ya — server menolak permintaan dengan token yang kedaluwarsa terlepas dari status di sisi klien (client-side)  
- [ ] Tidak — timeout **hanya** diterapkan melalui JavaScript sisi klien atau meta-refresh  

### Apakah aplikasi menerapkan absolute session timeout?
- [ ] Ya — sesi dihentikan setelah durasi maksimum yang tetap terlepas dari aktivitas  
- [ ] Tidak — sesi **dapat** diperpanjang tanpa batas waktu selama ada aktivitas terus-menerus  

### Apakah pengidentifikasi sesi (session identifier) dibatalkan validitasnya di server saat timeout?
- [ ] Ya — token sesi **tidak dapat** digunakan kembali setelah ambang batas timeout tercapai  
- [ ] Tidak — token sesi **masih valid** di server bahkan setelah klien dialihkan ke halaman login  

### Bisakah sesi tetap aktif tanpa batas waktu menggunakan permintaan heartbeat otomatis?
- [ ] No — absolute timeout atau kontrol sekunder **mencegah** perpanjangan sesi yang tidak terbatas  
- [ ] Ya — permintaan latar belakang berkala (misalnya, AJAX) **dapat** mempertahankan sesi tanpa batas waktu  

---