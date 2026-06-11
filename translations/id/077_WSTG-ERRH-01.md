## WSTG-ERRH-01 — Testing for Improper Error Handling

Penanganan kesalahan yang tidak tepat (Improper Error Handling) terjadi ketika aplikasi web mengungkapkan detail teknis yang sensitif—seperti stack traces, informasi skema basis data (database schema), atau jalur file internal—melalui respons kesalahannya. Penyerang memicu kesalahan ini dengan mengirimkan input yang cacat (malformed input), mengakses sumber daya yang tidak ada, atau menginduksi pengecualian sisi server (server-side exceptions) untuk memetakan arsitektur dasar aplikasi dan mengidentifikasi vektor potensial untuk eksploitasi lebih lanjut. Kebocoran informasi ini sering kali menjadi pendahulu bagi serangan yang lebih parah, termasuk SQL Injection atau path traversal, dengan memberikan spesifikasi lingkungan yang tepat kepada penyerang. Implementasi yang aman harus menyediakan pesan ramah pengguna yang generik (generic user-friendly messages) sambil menangkap diagnostik terperinci hanya dalam log sisi server yang aman.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-ERRH-01 |
| **CWE** | CWE-209 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Rendah / Sedang |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/01-Testing_For_Improper_Error_Handling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite (Repeater/Intruder)`, `ffuf`, `wfuzz`, `Arjun`, `curl`

### Apakah aplikasi menggunakan penanganan kesalahan global yang generik untuk pengecualian yang tidak tertangani (unhandled exceptions)?
- [ ] Ya — halaman kesalahan generik **diaktifkan** dan **tidak** mengungkapkan detail teknis  
- [ ] Ya — halaman kesalahan khusus digunakan tetapi kebocoran **mungkin terjadi** melalui header tertentu  
- [ ] Tidak — halaman kesalahan server default (misalnya, Tomcat, IIS, Nginx) **terlihat**  

### Dapatkah informasi teknis dienumerasi melalui input yang cacat (malformed input) atau kasus batas (boundary cases)?
- [ ] Tidak — kesalahan ditangani dengan baik dengan ID referensi unik untuk logging  
- [ ] Ya — stack traces atau kueri basis data (database queries) **diungkapkan** dalam respons  
- [ ] Ya — jalur file internal, variabel lingkungan (environment variables), atau string versi server **diungkapkan**  

### Apakah respons API membocorkan objek kesalahan verbose atau informasi debug?
- [ ] Tidak — API mengembalikan kode kesalahan standar dan pesan JSON yang telah dibersihkan (sanitized)  
- [ ] Ya — API mengembalikan objek debug verbose atau detail pengecualian lengkap dalam body respons  
- [ ] Ya — stack traces disertakan dalam bidang `details` atau `exception` dari respons JSON  

### Apakah aplikasi berperilaku berbeda (Berbasis waktu atau Berbasis konten) saat terjadi kesalahan?
- [ ] Tidak — tanda tangan (signatures) dan waktu respons konsisten terlepas dari jenis kesalahannya  
- [ ] Ya — respons diferensial **dapat** digunakan untuk mengenumerasi status valid vs. tidak valid (misalnya, User Enumeration)  

---