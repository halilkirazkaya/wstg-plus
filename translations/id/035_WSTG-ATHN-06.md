## WSTG-ATHN-06 — Testing for Browser Cache Weakness

Kelemahan cache browser (browser cache weakness) terjadi ketika informasi sensitif disimpan dalam cache browser lokal dan dapat diambil oleh pengguna yang tidak berwenang yang memiliki akses ke mesin fisik yang sama. Kurangnya header kontrol cache yang tepat memungkinkan data yang berpotensi sensitif seperti informasi pribadi, rincian akun, atau pengenal sesi (session identifiers) tetap ada setelah pengguna keluar (logout) atau menutup sesi. Penyerang atau pengguna berikutnya pada terminal bersama atau publik dapat mengeksploitasi hal ini dengan menelusuri riwayat browser, menggunakan tombol "Back", atau memeriksa file cache lokal untuk mengeksfiltrasi respons yang tersimpan dalam cache. Memastikan penerapan direktif yang ketat seperti `Cache-Control: no-store` dan `Pragma: no-cache` sangat penting bagi aplikasi apa pun yang menangani konten terautentikasi untuk memastikan data sensitif tidak pernah ditulis ke disk.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-ATHN-06 |
| **CWE** | CWE-525 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Rendah / Sedang* |

> *Keparahan menjadi Sedang jika aplikasi sering diakses dari kios bersama/publik atau berisi data PII/PHI yang sangat sensitif.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/06-Testing_for_Browser_Cache_Weaknesses  

**Alat:** `Burp Suite (Proxy/Repeater)`, `Browser Developer Tools (Network Tab)`, `Zed Attack Proxy (ZAP)`

### Apakah header cache-control yang sesuai tersedia pada halaman terautentikasi atau sensitif?
- [ ] Ya — `Cache-Control: no-store, no-cache` dan `Pragma: no-cache` **ada** dan **dikonfigurasi dengan benar**  
- [ ] Ya — beberapa header ada tetapi `no-store` **hilang**, memungkinkan penyimpanan cache ke disk  
- [ ] Tidak — tidak ada header terkait cache yang **diterapkan**, bergantung pada perilaku default browser  

### Apakah aplikasi menggunakan direktif `private` untuk data spesifik pengguna?
- [ ] Ya — `Cache-Control: private` **diaktifkan** untuk semua konten yang dipersonalisasi guna mencegah cache oleh proxy  
- [ ] Tidak — konten ditandai sebagai `public` atau tidak memiliki direktif `private`, sehingga dapat **disimpan dalam cache** oleh proxy perantara  

### Apakah informasi sensitif dapat diakses melalui tombol "Back" browser setelah logout berhasil?
- [ ] Tidak — browser meminta autentikasi ulang atau halaman **tidak dimuat** dari cache lokal  
- [ ] Ya — informasi sensitif **masih terlihat** melalui tombol "Back" setelah sesi diakhiri  

### Apakah file non-HTML yang sensitif (misalnya, PDF, ekspor CSV, respons JSON API) terlindungi dari caching?
- [ ] Tidak — tidak ada file non-HTML sensitif yang dihasilkan atau ditangani oleh aplikasi  
- [ ] Ya — header cache-control yang ketat **diterapkan** pada semua unduhan file sensitif dan endpoint API  
- [ ] Tidak — unduhan sensitif atau respons API **disimpan** dalam cache browser lokal atau direktori sementara  

### Apakah aplikasi menggunakan `Expires: 0` atau tanggal di masa lalu untuk mencegah penggunaan data yang sudah usang?
- [ ] Ya — header `Expires` disetel ke `0` atau tanggal historis, **memastikan** browser menganggap konten segera usang  
- [ ] Tidak — header `Expires` **hilang** atau disetel ke tanggal di masa depan untuk halaman sensitif  

---