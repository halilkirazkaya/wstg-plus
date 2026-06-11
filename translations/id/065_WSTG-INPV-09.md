## WSTG-INPV-09 — Pengujian untuk XPath Injection

XPath Injection terjadi ketika sebuah aplikasi menggunakan informasi yang diberikan oleh pengguna untuk menyusun kueri XPath untuk data XML, yang memungkinkan penyerang untuk mengganggu logika kueri tersebut. Dengan menyuntikkan sintaks tertentu seperti tanda kutip tunggal, kurung siku, dan operator logika, penyerang dapat menavigasi struktur dokumen XML, melewati autentikasi (Bypass Authentication), atau mengeksfiltrasi node data sensitif. Kerentanan ini umumnya ditemukan pada layanan web (Web Services), antarmuka manajemen konfigurasi, dan sistem warisan (Legacy Systems) yang mengandalkan penyimpanan data berbasis XML daripada basis data SQL tradisional. Dari sudut pandang penyerang, hal ini sering dieksploitasi menggunakan teknik berbasis boolean (boolean-based) atau berbasis kesalahan (error-based) untuk memetakan pohon XML secara sistematis dan mengambil nilai tersembunyi dari backend.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INPV-09 |
| **CWE** | CWE-643 |
| **Test Status** | Belum Dilakukan |
| **Severity** | Tinggi / Kritis |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/09-Testing_for_XPath_Injection  
* https://hacktricks.wiki/en/pentesting-web/xpath-injection.html  

**Alat:** `Burp Suite (Intruder)`, `XCat`, `XPath-Injection-Tool`, `FFUF`

### Apakah input pengguna yang digunakan untuk menyusun kueri XPath telah disanitasi atau diparameterisasi dengan benar?
- [ ] Ya — input **tidak** digunakan dalam kueri XPath atau telah diparameterisasi secara ketat  
- [ ] Ya — validasi input/pengkodean (encoding) yang kuat **telah diterapkan** dan bypass **tidak dimungkinkan**  
- [ ] Ya — beberapa validasi **telah diterapkan** tetapi bypass **dimungkinkan** menggunakan karakter yang dikodekan  
- [ ] Tidak — input pengguna digabungkan secara langsung (concatenated) ke dalam kueri XPath *(Kritis)*  

### Apakah aplikasi mengungkapkan detail struktural XML/XPath melalui pesan kesalahan?
- [ ] Tidak — halaman kesalahan generik ditampilkan dan **tidak ada** detail XML yang bocor  
- [ ] Ya — pesan kesalahan mengungkapkan adanya pemrosesan XML tetapi **tidak ada** detail jalur (path)  
- [ ] Ya — pesan kesalahan yang mendetail (verbose) mengungkapkan sintaks XPath, nama node, atau jalur file  

### Apakah mungkin untuk melewati logika autentikasi atau otorisasi menggunakan logika XPath?
- [ ] Tidak — logika autentikasi **tidak** bergantung pada XML/XPath atau diimplementasikan secara aman  
- [ ] Ya — pemeriksaan login atau izin dapat dilewati menggunakan payload berbasis logika seperti `' or 1=1 or '`  

### Bisakah struktur dokumen atau data XML dienumerasi melalui blind injection?
- [ ] Tidak — aplikasi **tidak** rentan terhadap inferensi berbasis boolean atau berbasis waktu  
- [ ] Ya — nama node dan data **dapat** dieksfiltrasi menggunakan inferensi berbasis boolean  
- [ ] Ya — penelusuran pohon XML lengkap (full XML tree traversal) dan ekstraksi data **dimungkinkan**  

---