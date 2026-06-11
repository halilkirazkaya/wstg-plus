## WSTG-BUSL-01 — Pengujian Validasi Data Logika Bisnis (Test Business Logic Data Validation)

Pengujian validasi data logika bisnis berfokus pada kemampuan aplikasi untuk mengidentifikasi dan menolak data yang benar secara sintaksis tetapi tidak valid secara logika dalam konteks domain bisnis. Penyerang mengeksploitasi celah ini dengan mengirimkan nilai yang tidak terduga—seperti kuantitas negatif dalam keranjang belanja, tanggal di masa lalu untuk acara mendatang, atau integer yang sangat besar—untuk melewati batasan dan memanipulasi status aplikasi. Kerentanan ini sering ditemukan dalam alur kerja multi-langkah (multi-step workflows), proses checkout, dan modul manajemen akun di mana logika sisi server (server-side logic) gagal memverifikasi integritas kontekstual dari data yang diberikan pengguna. Eksploitasi yang berhasil dapat menyebabkan kerugian finansial, kompromi integritas, dan akses tidak sah ke fitur atau data terbatas.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-BUSL-01 |
| **CWE** | CWE-20 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan (Severity)** | Tinggi / Sedang |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/01-Test_Business_Logic_Data_Validation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**Alat:** `Burp Suite (Repeater/Intruder)`, `Postman`, `Python (Requests)`, `OWASP ZAP`

### Apakah aplikasi menerapkan batasan rentang logika pada input numerik?
- [ ] Ya — aplikasi menolak nilai negatif, nol, atau nilai yang terlalu besar dengan benar jika tidak sesuai *(Paling aman)*  
- [ ] Ya — aplikasi menolak beberapa rentang yang tidak valid tetapi **rentan** terhadap integer overflow atau bypass batasan (boundary bypass)  
- [ ] No — aplikasi menerima nilai numerik apa pun terlepas dari konteks bisnis *(Kritis)*  

### Apakah pemeriksaan validasi sisi server konsisten di semua lapisan aplikasi?
- [ ] Ya — validasi diterapkan secara konsisten di lapisan API/layanan dan basis data  
- [ ] Ya — validasi **diterapkan** pada UI/Frontend tetapi bypass melalui permintaan API langsung **dimungkinkan**  
- [ ] No — validasi **tidak diterapkan** atau tidak konsisten, memungkinkan terjadinya korupsi data logis  

### Dapatkah logika bisnis disubversi dengan memanipulasi data dalam alur kerja multi-langkah (multi-step workflows)?
- [ ] No — status (state) dikelola dengan aman di server dan data dari langkah sebelumnya **tidak dapat** dirusak  
- [ ] Ya — validasi terjadi pada langkah awal tetapi pengiriman akhir **tidak** memverifikasi ulang integritas data  
- [ ] Ya — bypass alur kerja **dimungkinkan** dengan melewati langkah-langkah atau mengirimkan data yang tidak berurutan  

### Apakah aplikasi menangani data "kasus tepi" (edge case) yang melewati batasan bisnis dengan benar?
- [ ] Ya — batasan bersifat kuat dan menangani input yang tidak biasa tetapi valid secara sintaksis (misalnya, tahun kabisat, pergeseran zona waktu)  
- [ ] Ya — beberapa kasus tepi ditangani tetapi kombinasi tertentu **dapat** menyebabkan perilaku yang tidak terduga  
- [ ] No — kasus tepi secara langsung menyebabkan kegagalan logika, seperti pembelian gratis atau perubahan status yang tidak sah  

---