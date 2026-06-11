## WSTG-ATHZ-03 — Testing for Privilege Escalation

Eskalasi hak istimewa (Privilege Escalation) terjadi ketika penyerang mengeksploitasi kerentanan untuk mendapatkan akses ke sumber daya atau fungsionalitas yang disediakan bagi pengguna dengan tingkat otorisasi yang lebih tinggi atau identitas yang berbeda. Dalam eskalasi vertikal, pengguna standar mencoba mengakses fungsi administratif, sementara eskalasi horizontal melibatkan akses ke data milik pengguna lain dengan tingkat hak istimewa yang sama. Celah ini biasanya bermanifestasi dalam Access Control Lists (ACL) yang diimplementasikan dengan buruk, Insecure Direct Object References (IDOR), atau manipulasi parameter (parameter tampering) di dalam sesi atau token identitas. Dari sudut pandang penyerang, hal ini sering dicapai dengan memanipulasi ID numerik, memodifikasi parameter berbasis peran (role-based parameters) dalam cookie atau JWT, atau memanggil secara langsung endpoint API tersembunyi yang tidak memiliki pemeriksaan otorisasi di sisi server.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-ATHZ-03 |
| **CWE** | CWE-269 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi / Kritis |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/03-Testing_for_Privilege_Escalation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**Peralatan:** `Burp Suite`, `Authorize`, `AuthMatrix`, `Authz`, `Postman`, `Ffuf`

### Apakah aplikasi mengimplementasikan beberapa tingkat hak istimewa atau multi-tenancy?
- [ ] Tidak — aplikasi adalah sistem pengguna tunggal atau peran tunggal  
- [ ] Ya — beberapa peran atau penyewa (tenants) **telah** didefinisikan dan memerlukan pengujian otorisasi  

### Apakah eskalasi hak istimewa horizontal (akses tingkat yang sama) dimungkinkan?
- [ ] Tidak — pemeriksaan otorisasi **diterapkan** pada semua pengidentifikasi sumber daya dan pemilik sesi  
- [ ] Ya — akses ke data pengguna lain **dimungkinkan** melalui IDOR atau parameter tampering  
- [ ] Ya — kebocoran data **dimungkinkan** tetapi modifikasi data pengguna lain **tidak dimungkinkan**  

### Apakah eskalasi hak istimewa vertikal (rendah-ke-tinggi) dimungkinkan?
- [ ] Tidak — endpoint administratif secara ketat menegakkan Role-Based Access Controls (RBAC)  
- [ ] Ya — fungsi administratif **dapat** diakses oleh pengguna dengan hak istimewa rendah melalui akses URL langsung  
- [ ] Ya — fungsi administratif **dapat** diakses dengan memanipulasi header terkait peran, cookie, atau klaim JWT  

### Dapatkah peran atau izin pengguna dimodifikasi melalui Mass Assignment atau Parameter Pollution?
- [ ] Tidak — bidang berbasis peran bersifat hanya-baca (read-only) dan **tidak dapat** dimodifikasi oleh pengguna  
- [ ] Ya — izin pengguna **dapat** ditingkatkan dengan menyertakan parameter tersembunyi (misalnya, `role=admin`) dalam pembaruan profil atau registrasi  

### Apakah pemeriksaan otorisasi ditegakkan secara konsisten di semua versi API dan metode HTTP?
- [ ] Ya — kontrol **diterapkan** secara konsisten di semua versi dan metode  
- [ ] Tidak — versi API lama (misalnya, `/v1/`) atau metode HTTP tertentu (misalnya, `PUT`, `DELETE`, `PATCH`) **melewati (bypass)** otorisasi  
- [ ] Tidak — fungsi administratif disembunyikan di UI tetapi **diaktifkan** pada tingkat API untuk semua pengguna  

---