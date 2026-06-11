## WSTG-INFO-01 — Melakukan Penemuan Mesin Pencari dan Pengintaian untuk Kebocoran Informasi

Penemuan mesin pencari (Search engine discovery) melibatkan pemanfaatan mesin pencari publik, halaman cache (cached pages), dan layanan pengindeksan untuk mengidentifikasi informasi yang secara tidak sengaja diekspos oleh organisasi target. Penyerang menggunakan operator pencarian tingkat lanjut (Google Dorks) dan layanan pengindeksan pihak ketiga untuk menemukan berkas sensitif, direktori, portal login, pesan kesalahan (error messages), dan metadata yang seharusnya tidak dapat diakses secara publik. Fase pengintaian (reconnaissance) ini sangat penting karena tidak memerlukan interaksi langsung dengan target, sehingga hampir tidak terdeteksi sementara berpotensi mengungkapkan titik masuk bernilai tinggi seperti panel admin yang terekspos, berkas konfigurasi, dump basis data, dan dokumentasi internal.

| Kolom | Nilai |
|---|---|
| **WSTG ID** | WSTG-INFO-01 |
| **CWE** | CWE-200 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Sedang |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/01-Conduct_Search_Engine_Discovery_Reconnaissance_for_Information_Leakage  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/external-recon-methodology/index.html  
* https://portswigger.net/web-security/information-disclosure  

**Alat:** `Google Dorks`, `theHarvester`, `Shodan`, `Censys`, `Wayback Machine`, `Recon-ng`, `SearchDiggity`

### Apakah berkas atau direktori sensitif diindeks oleh mesin pencari?
- [ ] Tidak — tidak ada konten sensitif yang ditemukan dalam hasil mesin pencari  
- [ ] Ya — berkas konfigurasi, cadangan (backups), atau dokumen internal **terindeks** *(Kritis)*  

### Apakah Google Dorking mengungkapkan antarmuka administratif atau login yang terekspos?
- [ ] Tidak — panel admin dan halaman login **tidak** terindeks  
- [ ] Ya — panel admin **terindeks** tetapi memerlukan autentikasi multi-faktor yang kuat  
- [ ] Ya — panel admin **terindeks** dan dapat diakses secara publik **tanpa** kontrol yang memadai  

### Apakah versi halaman dalam cache mengekspos data sensitif yang sudah dihapus?
- [ ] Tidak — tidak ada data sensitif yang ditemukan dalam snapshot cache  
- [ ] Ya — halaman dalam cache berisi kredensial, alamat IP internal, atau informasi sensitif  

### Apakah berkas `robots.txt` secara tidak sengaja mengungkapkan jalur (paths) sensitif?
- [ ] Tidak — `robots.txt` **tidak** mengungkapkan struktur direktori yang sensitif  
- [ ] Ya — `robots.txt` mencantumkan jalur sensitif yang membantu pengintaian (reconnaissance) penyerang  
- [ ] Berkas `robots.txt` tidak ada  

### Apakah layanan pihak ketiga (Shodan, Censys, Wayback Machine) mengungkapkan paparan historis atau saat ini?
- [ ] Tidak — tidak ada temuan signifikan dari layanan pengindeksan pihak ketiga  
- [ ] Ya — snapshot historis atau pemindaian layanan mengungkapkan metadata sensitif atau banner layanan  

---