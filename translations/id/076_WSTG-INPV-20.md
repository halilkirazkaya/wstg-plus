## WSTG-INPV-20 — Mass Assignment

Mass Assignment, juga dikenal sebagai Overposting atau Insecure Deserialization of parameters, terjadi ketika sebuah aplikasi web secara otomatis mengikat parameter permintaan HTTP yang masuk ke properti objek internal tanpa pemfilteran atribut yang tepat. Kerentanan ini umum ditemukan pada framework MVC modern dan API RESTful di mana JSON, XML, atau data form-encoded dideserialisasi secara langsung ke dalam model data sisi aplikasi. Penyerang mengeksploitasi perilaku ini dengan menyuntikkan parameter tambahan yang tidak terdaftar ke dalam permintaan—seperti `isAdmin`, `role`, atau `account_balance`—untuk mengubah bidang sensitif yang tidak dimaksudkan oleh pengembang untuk dikontrol oleh pengguna. Eksploitasi yang berhasil biasanya berakibat pada privilege escalation, modifikasi data tanpa izin, atau bypassing alur kerja logika bisnis yang kritis.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INPV-20 |
| **CWE** | CWE-915 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi / Sedang* |

> *Keparahan menjadi Tinggi jika atribut administratif, tingkat izin, atau bidang penagihan dapat dimodifikasi.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/20-Testing_for_Mass_Assignment  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Arjun`, `Param Miner`, `Burp Suite (Repeater)`, `Postman`, `ffuf`

### Apakah aplikasi menggunakan automatic binding untuk parameter permintaan yang masuk?
- [ ] Tidak — parameter dipetakan secara manual ke variabel tertentu atau objek yang aman  
- [ ] Ya — automatic binding digunakan tetapi **white-list** yang ketat atau DTO (Data Transfer Objects) diterapkan  
- [ ] Ya — automatic binding digunakan dan atribut internal yang sensitif **dapat** dimodifikasi  

### Dapatkah atribut administratif atau terkait izin berhasil diinjeksi?
- [ ] Tidak — parameter yang diinjeksi diabaikan, dihapus, atau menyebabkan kesalahan validasi  
- [ ] Ya — parameter tambahan diterima tetapi **tidak** memengaruhi status objek backend  
- [ ] Ya — menginjeksi parameter seperti `is_admin`, `role`, atau `permissions` **berhasil** melakukan eskalasi hak istimewa *(Kritis)*  

### Mungkinkah untuk memodifikasi logika bisnis yang sensitif atau bidang keuangan melalui overposting?
- [ ] Tidak — bidang seperti `price`, `balance`, atau `status` terlindungi dari mass assignment  
- [ ] Ya — atribut bisnis yang sensitif **dapat** dimodifikasi dengan menambahkannya ke dalam JSON atau body permintaan form  

### Apakah aplikasi mengungkapkan struktur objek internal yang memfasilitasi penebakan parameter?
- [ ] Tidak — respons hanya mencakup bidang publik yang dimaksudkan dan dokumentasi dibatasi  
- [ ] Ya — bidang objek internal terlihat dalam respons GET, sehingga memungkinkan penemuan parameter (parameter discovery)  
- [ ] Ya — dokumentasi API (Swagger/OpenAPI) secara eksplisit mencantumkan properti internal yang **dapat** ditetapkan  

---