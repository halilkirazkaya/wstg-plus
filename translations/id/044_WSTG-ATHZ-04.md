## WSTG-ATHZ-04 — Pengujian Insecure Direct Object References

Insecure Direct Object References (IDOR) terjadi ketika sebuah aplikasi memberikan akses langsung ke objek berdasarkan input yang diberikan oleh pengguna tanpa melakukan pemeriksaan otorisasi (authorization check) untuk memverifikasi izin pemohon. Kerentanan ini berdampak signifikan pada kerahasiaan dan integritas, karena memungkinkan penyerang untuk melihat, memodifikasi, atau menghapus data milik pengguna lain atau sistem. Hal ini biasanya bermanifestasi dalam parameter URL, data body POST, atau key JSON yang merujuk pada key database internal, nama file, atau pengenal akun. Dari sudut pandang penyerang, eksploitasi melibatkan manipulasi pengenal ini—sering kali melalui integer inkremental atau fuzzing UUID—untuk mengakses sumber daya di luar cakupan otorisasi mereka.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-ATHZ-04 |
| **CWE** | CWE-639 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Tinggi |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/04-Testing_for_Insecure_Direct_Object_References  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/access-control  

**Alat:** `Burp Suite (Autorize extension)`, `Caido`, `Postman`, `ffuf`, `Intruder`

### Apakah aplikasi menggunakan pengenal objek langsung dalam parameter permintaan?
- [ ] Tidak — aplikasi menggunakan referensi tidak langsung (indirect references) atau map terenkripsi *(Paling aman)*  
- [ ] Ya — aplikasi menggunakan pengenal yang tidak dapat diprediksi (misalnya, UUID/hash yang panjang)  
- [ ] Ya — aplikasi menggunakan pengenal yang dapat diprediksi (misalnya, integer inkremental atau username)  

### Apakah otorisasi sisi server (server-side authorization) dilakukan untuk setiap permintaan yang melibatkan pengenal objek?
- [ ] Ya — pemeriksaan sisi server **tidak dapat** dilewati untuk pengenal apa pun  
- [ ] Ya — pemeriksaan sisi server ada, tetapi bypass **mungkin dilakukan** melalui parameter pollution atau manipulasi header  
- [ ] Tidak — tidak ada pemeriksaan otorisasi yang **diterapkan** untuk memverifikasi kepemilikan objek yang diminta  

### Dapatkah penyerang mengakses atau memodifikasi objek milik pengguna lain (IDOR Horizontal)?
- [ ] Tidak — akses ke data pengguna lain **tidak dimungkinkan**  
- [ ] Ya — melihat data pengguna lain **mungkin dilakukan** tetapi modifikasi **tidak dimungkinkan**  
- [ ] Ya — baik melihat maupun memodifikasi data pengguna lain **mungkin dilakukan** *(Kritis)*  

### Apakah mungkin untuk mengakses objek tingkat administratif atau sistem (IDOR Vertikal)?
- [ ] Tidak — akses ke objek dengan hak istimewa lebih tinggi **tidak dimungkinkan**  
- [ ] Ya — akses ke konfigurasi administratif atau file sistem **mungkin dilakukan**  

### Apakah aplikasi membocorkan pengenal objek melalui pencarian, metadata, atau endpoint lainnya?
- [ ] Tidak — pengenal **tidak terekspos** secara tidak sengaja  
- [ ] Ya — pengenal untuk pengguna/objek lain **dapat** dienumerasi melalui endpoint sekunder atau profil publik  

---