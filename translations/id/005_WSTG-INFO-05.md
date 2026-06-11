## WSTG-INFO-05 — Meninjau Konten Halaman Web untuk Kebocoran Informasi

Meninjau konten halaman web melibatkan inspeksi manual dan otomatis terhadap respons aplikasi, termasuk file HTML, JavaScript, dan CSS, untuk mengidentifikasi informasi sensitif yang tidak sengaja terpapar kepada pengguna. Penyerang meneliti kode sumber sisi klien (client-side source code) untuk mencari komentar pengembang, field formulir tersembunyi, jalur server internal, API key yang di-hardcode, dan informasi debugging yang dapat membantu fase eksploitasi lebih lanjut. Kebocoran ini sering terjadi ketika pengembang lupa menghapus metadata atau kode debugging sebelum pindah ke lingkungan produksi, yang berpotensi mengungkapkan celah logika atau detail infrastruktur backend. Dari perspektif penyerang, hal ini menyediakan metode dengan upaya rendah (low-effort) untuk memetakan struktur internal aplikasi dan menemukan titik masuk yang terabaikan tanpa memicu peringatan keamanan atau berinteraksi dengan logika backend server.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INFO-05 |
| **CWE** | CWE-200 |
| **Status Pengujian** | Tidak Dilakukan |
| **Tingkat Keparahan** | Rendah / Sedang* |

> *Tingkat keparahan menjadi Tinggi jika ditemukan kredensial yang di-hardcode, kunci privat (private keys), atau variabel lingkungan internal yang sensitif.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/05-Review_Web_Page_Content_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**Alat:** `Burp Suite (Target/Proxy)`, `Browser Developer Tools`, `SecretFinder`, `grep`, `truffleHog`, `LinkFinder`

### Apakah terdapat komentar pengembang yang berisi informasi sensitif di dalam kode sumber?
- [ ] Tidak — tidak ada komentar atau hanya ditemukan komentar umum yang tidak sensitif  
- [ ] Ya — komentar tersedia namun **tidak** berisi logika atau data sensitif  
- [ ] Ya — komentar **mengungkapkan** jalur internal, query SQL, atau instruksi administratif  
- [ ] Ya — komentar **mengungkapkan** kredensial atau catatan pengembang yang sensitif *(Tinggi)*  

### Apakah field input HTML tersembunyi berisi data sensitif atau informasi status (state)?
- [ ] Tidak — tidak ditemukan field tersembunyi atau hanya berisi token yang tidak sensitif  
- [ ] Ya — field tersembunyi digunakan untuk status (state) namun **terenkripsi** atau **ditandatangani (signed)**  
- [ ] Ya — field tersembunyi berisi password **plaintext**, peran pengguna, atau ID internal  

### Apakah terdapat secret, API key, atau alamat IP internal yang di-hardcode di dalam file JavaScript?
- [ ] Tidak — tidak ditemukan string sensitif atau secret dalam file JS  
- [ ] Ya — file JS berisi API key publik dengan pembatasan yang **tepat**  
- [ ] Ya — file JS berisi API key yang **tidak terlindungi**, IP internal, atau endpoint privat  
- [ ] Ya — file JS berisi kredensial administratif atau kunci privat yang **di-hardcode** *(Kritis)*  

### Apakah aplikasi mengungkapkan versi framework atau jalur file internal di dalam kode sumber?
- [ ] Tidak — informasi framework dan jalur file **diminifikasi** atau **diobfuskasi**  
- [ ] Ya — nama dan versi framework **terlihat** namun telah dipatch  
- [ ] Ya — jalur file internal atau jalur server absolut **terekspos** dalam kode sumber  

### Apakah kode sumber rentan bocor karena kurangnya minifikasi atau obfuskasi?
- [ ] Tidak — kode produksi **diminifikasi sepenuhnya** dan bersih dari komentar  
- [ ] Ya — kode diminifikasi namun komentar **tetap ada** dalam sumber  
- [ ] Ya — source map (file `.map`) **dapat diakses secara publik**, memungkinkan rekonstruksi penuh dari kode sumber  

---