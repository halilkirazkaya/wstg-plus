## WSTG-APIT-02 — API Broken Object Level Authorization

Broken Object Level Authorization (BOLA), juga dikenal sebagai Insecure Direct Object Reference (IDOR), terjadi ketika API gagal memvalidasi apakah seorang pengguna memiliki izin yang tepat untuk mengakses atau memanipulasi sumber daya (resource) tertentu yang diidentifikasi oleh sebuah ID. Penyerang mengeksploitasi hal ini dengan melakukan enumerasi atau menebak pengenal (identifiers) secara sistematis—seperti ID numerik atau UUID—dalam jalur permintaan (request paths), parameter kueri (query parameters), atau badan JSON (JSON bodies) untuk mengakses data milik pengguna lain. Kerentanan ini adalah masalah yang paling umum dan berdampak besar dalam keamanan API modern, yang berpotensi menyebabkan eksfiltrasi data massal, modifikasi yang tidak sah, atau pengambilalihan akun secara penuh. Dari perspektif penyerang, tujuannya adalah untuk mengidentifikasi titik akhir (endpoints) yang menerima pengenal objek dan menguji apakah logika otorisasi di sisi server (server-side authorization) hilang atau dapat dilewati dengan memanipulasi ID tersebut.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-APIT-02 |
| **CWE** | CWE-285 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Tinggi / Kritis |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/02-API_Broken_Object_Level_Authorization  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/api-testing  

**Alat:** `Burp Suite (Intruder/Repeater)`, `Autorize`, `Postman`, `ffuf`, `Arjun`

### Apakah pengenal objek (object identifiers) dapat diprediksi atau dienumerasi dalam permintaan API?
- [ ] Tidak — pengenal bersifat panjang, acak, dan aman secara kriptografi (misalnya, UUIDv4)  
- [ ] Ya — pengenal berupa integer berurutan (misalnya, `101`, `102`)  
- [ ] Ya — pengenal mengikuti pola yang dapat diprediksi atau diturunkan dari informasi publik  

### Apakah API melakukan validasi kepemilikan objek di sisi server untuk setiap permintaan?
- [ ] Ya — pemeriksaan otorisasi diterapkan untuk setiap permintaan dan bypass **tidak dimungkinkan**  
- [ ] Ya — pemeriksaan diterapkan tetapi bypass **dimungkinkan** melalui parameter pollution atau method tunneling  
- [ ] No — aplikasi hanya mengandalkan autentikasi pengguna tanpa memeriksa kepemilikan sumber daya tertentu  

### Apakah mungkin untuk mengakses atau memodifikasi sumber daya pengguna lain dengan mengubah pengenal?
- [ ] Tidak — akses tidak sah ke sumber daya pengguna lain **tidak dimungkinkan**  
- [ ] Ya — akses **baca** tidak sah (Horizontal BOLA) **dimungkinkan**  
- [ ] Ya — **modifikasi** atau **penghapusan** tidak sah (Horizontal BOLA) **dimungkinkan**  

### Apakah API mengizinkan akses ke objek tingkat administratif atau sistem dengan mengganti ID?
- [ ] Tidak — sumber daya administratif dilindungi oleh lapisan otorisasi sekunder  
- [ ] Ya — mengakses atau memodifikasi sumber daya tingkat sistem (Vertical BOLA) **dimungkinkan**  

### Dapatkah pemeriksaan otorisasi dilewati dengan memindahkan pengenal ke bagian permintaan yang berbeda?
- [ ] Tidak — logika otorisasi konsisten terlepas dari lokasi parameter  
- [ ] Ya — bypass **dimungkinkan** ketika ID dipindahkan dari jalur URL ke badan JSON atau header  
- [ ] Ya — bypass **dimungkinkan** ketika beberapa ID diberikan dan server memproses ID yang tidak sah  

---