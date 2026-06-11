## WSTG-INPV-02 — Stored Cross Site Scripting (XSS)

Stored Cross Site Scripting (XSS), juga dikenal sebagai Persistent XSS, terjadi ketika sebuah aplikasi menerima data dari pengguna dan menyimpannya dalam basis data (database), sistem berkas (file system), atau media penyimpanan lainnya secara persisten tanpa validasi atau pengkodean (encoding) yang memadai. Ketika korban kemudian menavigasi ke halaman yang mengambil dan menampilkan data yang belum dibersihkan (unsanitized) ini, skrip berbahaya akan dieksekusi di dalam konteks browser mereka di bawah asal (origin) aplikasi tersebut. Kerentanan ini sangat berbahaya karena tidak memerlukan korban untuk mengeklik tautan yang dirancang khusus; setiap pengguna yang melihat halaman yang terpengaruh menjadi target, yang berpotensi menyebabkan pembajakan sesi (session hijacking), tindakan tidak sah, atau pengambilan kredensial (credential harvesting). Penyerang biasanya menargetkan bagian komentar, profil pengguna, papan pesan, dan log administratif di mana input dirender secara konsisten untuk pengguna lain atau administrator.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INPV-02 |
| **CWE** | CWE-79 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/02-Testing_for_Stored_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/cross-site-scripting  

**Alat:** `Burp Suite`, `XSStrike`, `BeEF`, `KXSStrike`, `Sleepy Puppy`

### Apakah terdapat titik masuk (entry points) yang menyimpan input pengguna untuk ditampilkan kemudian kepada pengguna lain?
- [ ] Tidak — aplikasi tidak menyimpan input yang dapat dikontrol pengguna untuk perenderan di kemudian hari  
- [ ] Ya — input disimpan (misalnya, profil, komentar) tetapi **tidak** dirender dalam konteks HTML  
- [ ] Ya — input disimpan dan dirender kepada pengguna lain atau administrator  

### Apakah pengkodean output (output encoding) atau sanitasi diterapkan saat data yang disimpan dirender?
- [ ] Ya — pengkodean output yang sadar konteks (context-aware output encoding) **diterapkan** dan bypass **tidak dimungkinkan** *(Paling aman)*  
- [ ] Ya — sanitasi (misalnya, `DOMPurify`) **diterapkan** tetapi bypass **dimungkinkan** melalui kesalahan konfigurasi  
- [ ] No — data dirender secara langsung ke dalam DOM **tanpa** pengkodean atau sanitasi *(Tinggi)*  

### Bisakah validasi input atau sanitasi di-bypass menggunakan payload alternatif?
- [ ] Tidak — filter menangani berbagai pengkodean, tag tidak standar, dan event handler secara aman  
- [ ] Ya — bypass **dimungkinkan** menggunakan pengkodean entitas HTML atau tag tertentu (misalnya, `<svg>`, `<img>`)  
- [ ] Ya — bypass **dimungkinkan** melalui payload polyglot atau mutation-based XSS (mXSS)  

### Apakah Content Security Policy (CSP) menyediakan lapisan pertahanan sekunder?
- [ ] Ya — CSP yang ketat **diaktifkan** dan mencegah eksekusi skrip inline dan sumber yang tidak sah  
- [ ] Ya — CSP **diaktifkan** tetapi salah dikonfigurasi (misalnya, `unsafe-inline` atau `script-src` yang luas)  
- [ ] Tidak — tidak ada CSP yang **diaktifkan** untuk aplikasi tersebut  

### Apakah mungkin untuk mencapai "Stored XSS to RCE" atau rantai serangan berdampak tinggi lainnya (Blind XSS)?
- [ ] Tidak — dampak terbatas pada konteks browser sisi klien (client-side)  
- [ ] Ya — stored XSS **dapat** digunakan untuk menargetkan administrator (Blind XSS) di panel internal  
- [ ] Ya — stored XSS **dapat** dirangkai dengan kerentanan lain (misalnya, CSRF, eksfiltrasi data sensitif)  

---