## WSTG-CONF-11 — Test Cloud Storage

Pengujian miskonfigurasi cloud storage (penyimpanan awan) melibatkan identifikasi dan audit layanan penyimpanan objek (object storage) yang dapat diakses secara publik seperti Amazon S3, Azure Blobs, atau Google Cloud Storage. Sumber daya ini sering kali berisi data sensitif termasuk cadangan (backups), kode sumber, Informasi Identitas Pribadi (PII), atau file konfigurasi yang terekspos karena Daftar Kontrol Akses (ACL) atau kebijakan Manajemen Identitas dan Akses (IAM) yang terlalu permisif. Penyerang mengenumerasi nama bucket melalui rekonaisans pada file JavaScript, sumber HTML, dan teknik Brute Force untuk menemukan titik masuk guna eksfiltrasi data (data exfiltration) atau modifikasi file yang tidak sah. Eksploitasi dapat menyebabkan dampak signifikan, termasuk kebocoran data penuh atau kemampuan untuk menyajikan file berbahaya dari domain tepercaya.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CONF-11 |
| **CWE** | CWE-922 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi / Kritis* |

> *Tingkat keparahan menjadi Kritis jika PII, kredensial, atau cadangan produksi dapat diakses, atau jika akses tulis tanpa autentikasi diaktifkan.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/11-Test_Cloud_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `aws-cli`, `gsutil`, `azure-cli`, `S3Scanner`, `CloudEnum`, `FFUF`, `Burp Suite`

### Apakah endpoint cloud storage (buckets/containers) diidentifikasi dan dienumerasi?
- [ ] Tidak — tidak ada endpoint cloud storage yang digunakan atau ditemukan  
- [ ] Ya — endpoint penyimpanan diidentifikasi melalui analisis kode atau pemantauan lalu lintas  
- [ ] Ya — endpoint penyimpanan ditemukan melalui brute force konvensi penamaan  

### Dapatkah pengguna tanpa autentikasi melihat daftar konten (listing) dari cloud storage?
- [ ] Tidak — listing bucket **dinonaktifkan** dan mengembalikan 403 Forbidden atau 404 Not Found  
- [ ] Ya — listing **diaktifkan** tetapi tidak ada file sensitif yang ditemukan  
- [ ] Ya — listing **diaktifkan** dan nama file/metadata sensitif terlihat  

### Apakah pengunduhan file sensitif dari bucket yang teridentifikasi dimungkinkan?
- [ ] Tidak — file dilindungi oleh kebijakan IAM atau diperlukan autentikasi yang valid  
- [ ] Ya — file dapat diakses tetapi memerlukan URL yang ditandatangani (Signed URL) yang **tidak** mudah ditebak  
- [ ] Ya — file sensitif (cadangan, `.env`, PII) dapat **diakses secara publik** untuk diunduh *(Kritis)*  

### Apakah unggahan atau modifikasi file tanpa autentikasi diizinkan?
- [ ] Tidak — unggahan atau modifikasi tanpa autentikasi **tidak dimungkinkan**  
- [ ] Ya — unggahan terautentikasi diperlukan tetapi bypass **dimungkinkan** melalui kondisi kebijakan yang lemah  
- [ ] Ya — penulisan, penimpaan, atau penghapusan file tanpa autentikasi **dimungkinkan** *(Kritis)*  

### Apakah kebijakan Cross-Origin Resource Sharing (CORS) pada endpoint penyimpanan bersifat restriktif?
- [ ] Ya — CORS **dinonaktifkan** atau dibatasi pada origin tepercaya tertentu  
- [ ] Tidak — CORS **diaktifkan** dengan origin wildcard `*`, memungkinkan akses berbasis web yang tidak sah  

---