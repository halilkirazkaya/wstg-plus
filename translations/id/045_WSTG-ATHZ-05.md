## WSTG-ATHZ-05 — Menguji Kelemahan OAuth

Kelemahan OAuth mencakup berbagai kecacatan dalam implementasi protokol OAuth 2.0 atau OpenID Connect (OIDC), yang sering kali berakibat pada pengambilalihan akun secara penuh (*account takeover*) atau akses data yang tidak sah. Kerentanan ini biasanya muncul selama alur otorisasi (*authorization flow*), khususnya terkait validasi `redirect_uri`, entropi dan verifikasi parameter `state`, serta penanganan Access Token atau ID Token secara aman. Penyerang mengeksploitasi hal ini dengan mencegat kode otorisasi, mengarahkan token ke domain yang dikendalikan penyerang melalui *open redirect*, atau melakukan Cross-Site Request Forgery (CSRF) untuk menghubungkan sesi korban ke akun penyerang. Karena OAuth sering berfungsi sebagai mekanisme autentikasi utama, satu kesalahan konfigurasi dapat merusak integritas seluruh sistem identitas pengguna.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-ATHZ-05 |
| **CWE** | CWE-287 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Tinggi / Kritis |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/05-Testing_for_OAuth_Weaknesses  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/oauth  

**Alat:** `Burp Suite (Repeater/Collaborator)`, `CyberChef`, `OIDC-Checker`, `OAuth-Scanner`

### Apakah parameter `state` diimplementasikan dan divalidasi untuk mencegah CSRF?
- [ ] Ya — `state` bersifat wajib, unik per sesi, dan divalidasi secara ketat di sisi server  
- [ ] Ya — `state` ada tetapi bersifat statis atau dapat diprediksi di berbagai sesi yang berbeda  
- [ ] Ya — `state` ada tetapi aplikasi **tidak** memvalidasinya saat pemanggilan balik (*callback*)  
- [ ] Tidak — parameter `state` **tidak** digunakan dalam permintaan otorisasi *(Kritis)*  

### Apakah `redirect_uri` divalidasi secara ketat terhadap daftar putih (whitelist)?
- [ ] Ya — hanya kecocokan persis terhadap daftar putih yang telah didaftarkan sebelumnya yang **diterima**  
- [ ] Ya — validasi diterapkan tetapi bypass **memungkinkan** melalui *path traversal* atau manipulasi subdomain  
- [ ] Ya — validasi diterapkan tetapi bypass **memungkinkan** melalui *parameter pollution* atau injeksi fragmen  
- [ ] Tidak — aplikasi **mengizinkan** URL arbitrer dalam parameter `redirect_uri` *(Kritis)*  

### Dapatkah `response_type` dimanipulasi untuk mengubah alur (flow)?
- [ ] Tidak — aplikasi hanya menerima alur yang dimaksudkan (misalnya, `code`) dan menolak yang lain  
- [ ] Ya — beralih dari `code` ke `token` (Implicit flow) **memungkinkan** dan membocorkan token di URL  
- [ ] Ya — alur hibrida (*hybrid flows*) atau nilai `response_type` yang tidak sah **diaktifkan** dan diproses  

### Apakah token akses atau kode otorisasi bocor melalui header Referer atau riwayat browser?
- [ ] Tidak — token/kode ditangani dengan aman dan halaman sensitif menggunakan `Referrer-Policy` yang tepat  
- [ ] Ya — token/kode **bocor** ke domain pihak ketiga melalui header `Referer`  
- [ ] Ya — token/kode **terlihat** di riwayat browser karena caching yang tidak aman atau penggunaan permintaan GET  

### Apakah aplikasi menerapkan prinsip "Least Privilege" untuk cakupan (scopes) OAuth?
- [ ] Ya — aplikasi hanya meminta cakupan minimum yang diperlukan untuk fungsionalitas tersebut  
- [ ] Tidak — aplikasi meminta cakupan yang berlebihan atau cakupan administratif secara default  
- [ ] Tidak — pengguna **tidak dapat** meninjau atau membatalkan pilihan cakupan tertentu selama fase persetujuan (*consent phase*)  

---