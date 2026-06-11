## WSTG-ATHZ-01 — Pengujian Directory Traversal File Include

Kerentanan directory traversal dan file inclusion muncul ketika sebuah aplikasi menggunakan input yang dapat dikontrol pengguna untuk menyusun path ke file atau direktori tanpa validasi atau sanitasi yang memadai. Penyerang mengeksploitasi celah ini dengan menyuntikkan sekuens seperti `../` untuk menavigasi ke luar direktori yang dimaksud, yang berpotensi mengakses file sistem yang sensitif, data konfigurasi, atau kode sumber aplikasi. Dalam kasus yang lebih parah yang melibatkan Local File Inclusion (LFI) atau Remote File Inclusion (RFI), penyerang dapat mencapai eksekusi kode jarak jauh (remote code execution) dengan menyertakan skrip berbahaya atau memanfaatkan log poisoning dan PHP wrapper. Kerentanan ini biasanya berada pada parameter yang digunakan untuk pemuatan konten dinamis, template engine, atau endpoint pengambilan gambar di mana logika sisi server menangani path sistem file.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-ATHZ-01 |
| **CWE** | CWE-22 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi / Kritis |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/01-Testing_Directory_Traversal_File_Include  
* https://hacktricks.wiki/en/pentesting-web/file-inclusion/index.html  
* https://portswigger.net/web-security/file-path-traversal  

**Alat:** `Burp Suite`, `FFUF`, `DotDotPwn`, `LFI Suite`, `Wfuzz`

### Apakah parameter menerima nama file atau path untuk pemrosesan di sisi server?
- [ ] Tidak — tidak ada parameter yang terlihat berinteraksi dengan sistem file  
- [ ] Ya — parameter tersedia namun menggunakan daftar izin (allowlist) yang ketat untuk pengenal file  
- [ ] Ya — parameter menerima nama file atau path secara langsung  

### Apakah input disanitasi untuk mencegah sekuens directory traversal?
- [ ] Ya — input divalidasi terhadap allowlist yang ketat dan sekuens traversal **tidak dimungkinkan**  
- [ ] Ya — input disanitasi dengan menghapus sekuens `../`, namun bypass rekursif **mungkin dilakukan**  
- [ ] Tidak — tidak ada sanitasi atau validasi yang **diterapkan** pada input terkait path  

### Apakah mungkin untuk mengakses file di luar direktori yang dibatasi melalui sekuens traversal?
- [ ] Tidak — aplikasi atau OS mencegah akses ke file di luar cakupan yang ditentukan  
- [ ] Ya — akses ke file di dalam web root **dimungkinkan**  
- [ ] Ya — akses ke file sistem yang sensitif (misalnya, `/etc/passwd`, `C:\Windows\win.ini`) **dimungkinkan** *(Kritis)*  

### Apakah server mengizinkan penyertaan URL jarak jauh (RFI)?
- [ ] Tidak — remote file inclusion **dinonaktifkan** pada level server/aplikasi  
- [ ] Ya — file jarak jauh **dapat** disertakan namun eksekusi **tidak dimungkinkan**  
- [ ] Ya — file jarak jauh **dapat** disertakan dan dieksekusi di server *(Kritis)*  

### Apakah filter dapat di-bypass menggunakan encoding atau karakter khusus?
- [ ] Tidak — filter secara efektif menangani berbagai encoding dan null byte  
- [ ] Ya — bypass **dimungkinkan** menggunakan URL encoding, double URL encoding, atau 16-bit Unicode  
- [ ] Ya — bypass **dimungkinkan** menggunakan injeksi null byte (`%00`) atau filesystem wrapper (misalnya, `php://filter`)  

---