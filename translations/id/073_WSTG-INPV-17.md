## WSTG-INPV-17 — Testing for Host Header Injection

Host Header Injection terjadi ketika aplikasi mempercayai header `Host` yang diberikan oleh pengguna dan menyertakannya ke dalam logika sisi server, seperti pembuatan tautan atau pengalihan (redirect), tanpa validasi yang memadai. Penyerang memanipulasi header ini untuk memfasilitasi Web Cache Poisoning, memfasilitasi Password Reset Poisoning dengan mengalihkan token sensitif ke domain eksternal, atau melewati kontrol keamanan yang bergantung pada perutean berbasis header. Eksploitasi yang berhasil memungkinkan penyerang untuk mengeksfiltrasi data sesi, melakukan pengambilalihan akun (Account Takeover), atau mengalihkan pengguna yang tidak menaruh curiga ke infrastruktur berbahaya. Kerentanan ini paling umum ditemukan di lingkungan dengan penyeimbang beban (load-balanced) atau aplikasi yang menghasilkan URL absolut secara dinamis berdasarkan konteks permintaan yang masuk.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INPV-17 |
| **CWE** | CWE-20 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Sedang / Tinggi* |

> *Tingkat keparahan menjadi Tinggi jika header Host digunakan untuk menghasilkan tautan sensitif dalam email, seperti pengaturan ulang kata sandi (password reset).

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/17-Testing_for_Host_Header_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/host-header  

**Alat:** `Burp Suite (Repeater/Collaborator)`, `curl`, `Param Miner`, `nmap (http-vhosts)`

### Apakah aplikasi mencerminkan nilai header `Host` dalam tautan, pengalihan (redirect), atau skrip?
- [ ] Tidak — aplikasi menggunakan base URL yang di-hardcode atau konfigurasi yang divalidasi secara ketat  
- [ ] Ya — header `Host` dicerminkan tetapi divalidasi secara ketat terhadap daftar putih (whitelist)  
- [ ] Ya — header `Host` dicerminkan dan bypass **mungkin dilakukan** melalui nilai yang salah format (misalnya, `expected.com:attacker.com`)  
- [ ] Ya — header `Host` dicerminkan **tanpa** validasi apa pun  

### Apakah header alternatif terkait host diproses oleh aplikasi atau infrastruktur?
- [ ] Tidak — header seperti `X-Forwarded-Host`, `X-Host`, atau `Forwarded` diabaikan  
- [ ] Ya — header alternatif diproses tetapi **tidak** menimpa (override) logika internal  
- [ ] Ya — header alternatif **dapat** digunakan untuk menimpa nilai header `Host` utama  

### Dapatkah header `Host` dimanipulasi untuk meracuni email yang dihasilkan sistem (misalnya, pengaturan ulang kata sandi)?
- [ ] Tidak — tautan email menggunakan domain statis tepercaya dari konfigurasi server  
- [ ] Ya — header `Host` memengaruhi tautan email tetapi validasi **tidak dapat** dilewati  
- [ ] Ya — header `Host` **dapat** dimanipulasi untuk mengalihkan token pengaturan ulang kata sandi ke domain yang dikendalikan penyerang *(Kritis)*  

### Apakah aplikasi rentan terhadap Web Cache Poisoning melalui header `Host`?
- [ ] Tidak — tidak ada mekanisme caching atau header `Host` adalah bagian dari cache key  
- [ ] Ya — terdapat cache tetapi ia memvalidasi secara ketat atau mengabaikan header `Host` untuk input yang tidak memiliki key (unkeyed inputs)  
- [ ] Ya — terdapat cache dan **dapat** diracuni oleh header `Host` yang diinjeksikan untuk menyajikan konten berbahaya kepada pengguna lain  

### Apakah server menerima header `Host` sembarang untuk perutean host virtual?
- [ ] Tidak — server mengembalikan kesalahan atau halaman default untuk header `Host` yang tidak dikenali  
- [ ] Ya — server menerima header `Host` apa pun, yang **berguna** untuk rekognisi atau enumerasi layanan internal  

---