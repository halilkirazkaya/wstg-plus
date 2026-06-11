## WSTG-BUSL-06 — Testing for the Circumvention of Work Flows

Pengabaian alur kerja (*Workflow circumvention*) melibatkan manipulasi logika aplikasi untuk melewati urutan atau prasyarat yang dimaksudkan dalam proses multitahap. Penyerang mengidentifikasi endpoint sensitif—seperti konfirmasi pembayaran, persetujuan administratif, atau layar penyelesaian MFA—dan mencoba mengaksesnya secara langsung, dengan melewatkan langkah-langkah perantara yang wajib. Kerentanan ini terjadi ketika logika sisi server gagal mempertahankan *state machine* yang kuat, melainkan mengandalkan asumsi bahwa pengguna mengikuti jalur yang digerakkan oleh antarmuka pengguna (UI). Eksploitasi yang berhasil dapat menyebabkan kegagalan logika bisnis yang kritis, termasuk transaksi yang tidak sah, eskalasi hak istimewa (*privilege escalation*), dan pengabaian mekanisme verifikasi identitas.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-BUSL-06 |
| **CWE** | CWE-841 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Sedang / Tinggi* |

> *Keparahan menjadi Tinggi jika pengabaian memungkinkan transaksi keuangan yang tidak sah, eskalasi hak istimewa, atau akses administratif.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/06-Testing_for_the_Circumvention_of_Work_Flows  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**Alat:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Postman`, `Caido`

### Apakah aplikasi berisi proses bisnis multitahap?
- [ ] Tidak — aplikasi hanya terdiri dari tindakan permintaan tunggal  
- [ ] Ya — proses multitahap (misalnya, keranjang belanja, formulir wizard, registrasi) **tersedia**  

### Apakah urutan langkah-langkah ditegakkan secara ketat oleh server?
- [ ] Ya — pelacakan status (*state tracking*) sisi server memastikan langkah-langkah **tidak dapat** dilewati  
- [ ] Ya — kontrol sisi klien menyarankan sebuah urutan, tetapi server **tidak** memvalidasi progresnya  
- [ ] Tidak — aplikasi **tidak** menegakkan urutan operasi tertentu apa pun  

### Dapatkah penyerang mencapai tahap akhir dari sebuah proses dengan langsung meminta URL atau endpoint terminal?
- [ ] Tidak — akses langsung ke langkah terminal **tidak dimungkinkan** tanpa penyelesaian sebelumnya  
- [ ] Ya — langkah terminal (misalnya, `/api/v1/checkout/complete`) **dapat** dijangkau melalui permintaan langsung  

### Apakah aplikasi memverifikasi bahwa data prasyarat dari langkah sebelumnya ada dan valid?
- [ ] Ya — server memvalidasi semua data langkah sebelumnya sebelum melanjutkan  
- [ ] Tidak — server **rentan** terhadap "pelewatkan" langkah-langkah di mana data diharapkan tetapi tidak diverifikasi  

### Dapatkah kontrol keamanan seperti MFA atau verifikasi identitas diabaikan dengan memanipulasi alur kerja?
- [ ] Tidak — kontrol kritis **tidak dapat** diabaikan melalui manipulasi alur  
- [ ] Ya — pengabaian MFA atau pemeriksaan identitas **dimungkinkan** dengan melompat ke langkah pasca-verifikasi  

---