## WSTG-INPV-11 — Code Injection

Code Injection (Injeksi Kode) terjadi ketika sebuah aplikasi mengevaluasi cuplikan kode yang diberikan pengguna dalam lingkungan runtime-nya, biasanya melalui fungsi spesifik bahasa pemrograman yang tidak aman seperti `eval()`, `exec()`, atau `system()`. Kerentanan ini berbeda dari Command Injection karena menargetkan konteks eksekusi bahasa pemrograman (misalnya PHP, Python, Node.js) daripada shell sistem operasi yang mendasarinya. Penyerang mengeksploitasi titik-titik ini untuk mengeksekusi logika arbitrer, mengeksfiltrasi variabel lingkungan, atau mencapai full Remote Code Execution (RCE) pada server. Pentester harus menyelidiki fitur yang memproses ekspresi matematika, server-side templates, atau parameter konfigurasi dinamis di mana input mungkin diinterpretasikan sebagai kode yang dapat dieksekusi.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INPV-11 |
| **CWE** | CWE-94 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Kritis |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11-Testing_for_Code_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/deserialization  
* https://portswigger.net/web-security/llm-attacks  

**Tools:** `Burp Suite (Repeater/Intruder)`, `FFUF`, `PayloadsAllTheThings`, `Commix`

### Apakah ada fitur aplikasi yang mengevaluasi input dinamis melalui fungsi evaluasi spesifik bahasa pemrograman?
- [ ] Tidak — fungsi evaluasi dinamis **tidak ditemukan** dalam basis kode  
- [ ] Ya — fungsi seperti `eval()`, `exec()`, atau `include()` digunakan tetapi **tidak** menerima input pengguna  
- [ ] Ya — fungsi digunakan dan memproses input yang diberikan pengguna  

### Apakah mekanisme validasi atau sanitasi input diterapkan pada input sebelum evaluasi?
- [ ] Ya — input divalidasi secara ketat terhadap **whitelist** yang di-hardcode  
- [ ] Ya — input disanitasi melalui blacklisting, namun bypass **dimungkinkan**  
- [ ] Tidak — input diteruskan langsung ke fungsi evaluasi dan **tidak ada kontrol** yang diterapkan  

### Apakah lingkungan eksekusi berada dalam sandbox atau dibatasi untuk mencegah akses tingkat sistem?
- [ ] Ya — eksekusi terjadi dalam sandbox yang sangat terbatas di mana panggilan sistem (system calls) **tidak dapat** dilakukan  
- [ ] Ya — ada beberapa batasan, tetapi sandbox escape **dimungkinkan**  
- [ ] Tidak — lingkungan memungkinkan akses penuh ke API bahasa dan fungsi OS yang mendasarinya *(Kritis)*  

### Bisakah Code Injection digunakan untuk mengeksfiltrasi informasi sensitif?
- [ ] Tidak — eksekusi bersifat blind dan tidak ada komunikasi out-of-band yang **dimungkinkan**  
- [ ] Ya — variabel lingkungan atau file lokal **dapat** dieksfiltrasi melalui permintaan HTTP/DNS  
- [ ] Ya — hasil dari kode yang dieksekusi **tercermin secara langsung** (directly reflected) dalam respons aplikasi  

### Apakah aplikasi memungkinkan Remote File Inclusion atau pemuatan pustaka dinamis?
- [ ] Tidak — hanya jalur kode lokal yang telah ditentukan sebelumnya yang dapat dieksekusi  
- [ ] Ya — aplikasi **dapat** dipaksa untuk memuat dan mengeksekusi kode dari sumber jarak jauh atau yang dikendalikan penyerang  

---