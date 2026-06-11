## WSTG-CLNT-11 — Menguji Web Messaging

Web Messaging, atau `postMessage`, memungkinkan komunikasi lintas-asal (cross-origin) antar objek window, seperti halaman induk dan popup yang dimunculkan atau iframe yang disematkan. Mekanisme ini sangat penting bagi fungsionalitas web modern namun menimbulkan risiko signifikan jika aplikasi penerima gagal memverifikasi identitas pengirim atau salah dalam menangani payload pesan. Penyerang mengeksploitasi kerentanan ini dengan menghosting situs berbahaya yang menyematkan aplikasi target dan mengirimkan pesan yang telah dimodifikasi untuk memicu tindakan yang tidak diinginkan, mengeksfiltrasi data sensitif, atau menjalankan Cross-Site Scripting (XSS) berbasis DOM. Eksploitasi yang berhasil terjadi ketika pendengar pesan (message listeners) tidak memvalidasi properti `origin` secara ketat atau meneruskan `message.data` secara langsung ke sink eksekusi yang berbahaya.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CLNT-11 |
| **CWE** | CWE-345 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Sedang / Tinggi |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/11-Testing_Web_Messaging  
* https://hacktricks.wiki/en/pentesting-web/postmessage-vulnerabilities/index.html  

**Alat:** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `PostMessage-Tracker`, `PMHook`

### Apakah aplikasi menggunakan API `postMessage` untuk komunikasi lintas-dokumen?
- [ ] Tidak — pendengar `postMessage` **tidak** ditemukan dalam kode sisi klien (client-side)  
- [ ] Ya — `postMessage` digunakan untuk komunikasi komponen internal  
- [ ] Ya — `postMessage` digunakan untuk berkomunikasi dengan domain atau widget pihak ketiga  

### Apakah origin dari pesan masuk divalidasi dengan ketat?
- [ ] Ya — origin diperiksa terhadap whitelist yang ketat menggunakan `===` atau perbandingan identik *(Paling aman)*  
- [ ] Ya — origin divalidasi menggunakan regex, namun logikanya **rentan** untuk dilewati (bypass) (misalnya, `^https://trusted.com`)  
- [ ] Tidak — properti `origin` **tidak** diperiksa, memungkinkan domain apa pun untuk mengirim pesan  
- [ ] Tidak — wildcard `*` digunakan pada `targetOrigin` dari pengirim, berpotensi membocorkan data ke pendengar yang berbahaya  

### Apakah payload pesan disanitasi sebelum diproses oleh aplikasi?
- [ ] Ya — data diperlakukan sebagai teks biasa dan tidak ada sink berbahaya yang digunakan  
- [ ] Ya — data diparsing (misalnya, `JSON.parse`) dan divalidasi sebelum digunakan, serta bypass **tidak dimungkinkan**  
- [ ] Tidak — data pesan diteruskan secara langsung ke sink berbahaya seperti `innerHTML`, `eval()`, atau `setTimeout()`  
- [ ] Tidak — data pesan digunakan untuk menavigasi jendela atau mengubah `location.href` tanpa validasi  

### Dapatkah penyerang memicu tindakan yang tidak sah atau mengeksfiltrasi data melalui pesan berbahaya?
- [ ] Tidak — bahkan dengan pesan palsu (spoofed message), tidak ada tindakan atau data sensitif yang dapat diakses  
- [ ] Ya — pesan yang dibuat khusus **dapat** memicu perubahan status yang sensitif (misalnya, perubahan kata sandi, pembaruan profil)  
- [ ] Ya — pesan yang dibuat khusus **dapat** mengakibatkan XSS berbasis DOM atau eksfiltrasi token CSRF/data sesi  

---