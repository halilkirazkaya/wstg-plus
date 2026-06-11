## WSTG-ERRH-02 — Testing for Stack Traces

Stack trace dihasilkan ketika aplikasi gagal menangani sebuah `exception` dengan baik, yang mengakibatkan terungkapnya status eksekusi internal dan hierarki panggilan (`call hierarchy`) kepada pengguna akhir. Bagi seorang penetration tester, trace ini sangat berharga karena dapat mengungkapkan `application framework`, versi pustaka (`library`), jalur sistem berkas (`file system paths`) internal, dan alur logika, yang secara signifikan mengurangi upaya yang diperlukan untuk `reconnaissance`. Eksploitasi melibatkan pemicuan kesalahan (`error`) secara sengaja melalui `malformed input`, tipe data yang tidak terduga, atau pelanggaran `boundary condition` untuk memaksa aplikasi ke dalam status tidak stabil dan mengeksfiltrasi informasi tentang `technology stack` yang mendasarinya. Kebocoran ini sering kali memberikan konteks yang diperlukan untuk mengidentifikasi komponen pihak ketiga yang rentan atau untuk merancang `exploit` spesifik bagi kelemahan logika yang ditemukan dalam jalur kode yang terungkap.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-ERRH-02 |
| **CWE** | CWE-209 |
| **Status Pengujian** | Belum Dilakukan |
| **Severity** | Low / Medium* |

> *Severity meningkat menjadi Medium jika stack trace mengungkapkan detail arsitektur yang sensitif, kueri database (database queries), atau parameter konfigurasi internal.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/02-Testing_for_Stack_Traces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Peralatan:** `Burp Suite`, `ffuf`, `Arjun`, `Wfuzz`, `Wappalyzer`

### Apakah stack trace lengkap dikembalikan ke klien saat terjadi kesalahan aplikasi?
- [ ] Tidak — aplikasi mengembalikan halaman kesalahan umum atau pesan kesalahan kustom  
- [ ] Ya — stack trace **diaktifkan** tetapi hanya untuk modul tertentu yang tidak sensitif  
- [ ] Ya — stack trace lengkap **diaktifkan** dan terlihat dalam badan respons HTTP  

### Apakah pesan kesalahan mengungkapkan informasi lingkungan yang sensitif?
- [ ] Tidak — pesan kesalahan tidak mengandung detail teknis atau lingkungan  
- [ ] Ya — pesan mengungkapkan **jalur berkas internal** atau **struktur direktori** sisi server  
- [ ] Ya — pesan mengungkapkan **skema database**, **kueri SQL**, atau **versi pustaka pihak ketiga**  

### Dapatkah stack trace dipicu dengan memanipulasi parameter input?
- [ ] Tidak — malformed input ditangani dengan baik atau ditolak oleh validasi  
- [ ] Ya — trace dapat dipicu oleh **tipe data yang tidak terduga** (misalnya, mengirimkan array di mana string diharapkan)  
- [ ] Ya — trace dipicu oleh **null bytes**, **karakter khusus**, atau **nilai batas** (boundary values)  

### Apakah global error handler diterapkan secara konsisten di seluruh aplikasi?
- [ ] Ya — sebuah global exception handler **diterapkan secara konsisten** di semua endpoint yang diuji  
- [ ] Ya — penangan (handler) tersedia tetapi **tidak diterapkan** pada endpoint legacy, API, atau endpoint yang baru ditambahkan  
- [ ] Tidak — tidak ada mekanisme penanganan kesalahan global yang **terdeteksi**, mengakibatkan kesalahan server default  

---