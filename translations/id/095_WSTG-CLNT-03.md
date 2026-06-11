## WSTG-CLNT-03 — HTML Injection

HTML Injection terjadi ketika aplikasi gagal melakukan sanitasi terhadap masukan yang disediakan pengguna sebelum merendernya di dalam browser, yang memungkinkan penyerang untuk menyisipkan (inject) tag HTML arbitrer ke dalam Document Object Model (DOM) halaman web. Meskipun mirip dengan Cross-Site Scripting (XSS), tujuan utama dari HTML Injection adalah untuk memanipulasi presentasi visual halaman atau untuk memfasilitasi serangan phishing dengan menyisipkan formulir berbahaya dan konten yang menipu. Penyerang menargetkan parameter yang tercermin (reflected) dalam antarmuka pengguna (UI), seperti istilah pencarian, kolom profil pengguna, atau pesan kesalahan, untuk menipu korban agar mengungkapkan kredensial sensitif atau berinteraksi dengan tautan pihak ketiga yang tidak sah. Kerentanan ini mewakili risiko yang signifikan terhadap integritas antarmuka pengguna aplikasi dan dapat menjadi pendahulu bagi rekayasa sosial (social engineering) atau serangan terkait sesi yang lebih kompleks.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CLNT-03 |
| **CWE** | CWE-80 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Rendah / Sedang |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/03-Testing_for_HTML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite`, `OWASP ZAP`, `DOM Invader`, `Wfuzz`

### Apakah masukan yang dikontrol pengguna tercermin dalam DOM tanpa enkoding HTML yang tepat?
- [ ] Tidak — semua masukan yang tercermin telah melalui proses enkoding HTML secara ketat sebelum dirender  
- [ ] Ya — beberapa karakter **telah** dienkoding tetapi bypass untuk tag tertentu **masih dimungkinkan**  
- [ ] Ya — masukan tercermin secara mentah (raw), dan HTML injection **dimungkinkan**  

### Dapatkah elemen struktural HTML diinjeksikan untuk mengubah tata letak halaman?
- [ ] Tidak — aplikasi memfilter atau menghapus tag struktural seperti `<div>`, `<table>`, atau `<iframe>`  
- [ ] Ya — elemen struktural **dapat** diinjeksikan tetapi **tidak** berdampak signifikan pada UI  
- [ ] Ya — perusakan tampilan (defacement) UI yang signifikan **dimungkinkan** melalui tag yang diinjeksikan  

### Apakah mungkin untuk menginjeksikan elemen phishing fungsional seperti formulir atau tautan berbahaya?
- [ ] Tidak — tag `<form>`, `<input>`, dan `<a>` secara efektif diblokir atau disanitasi  
- [ ] Ya — tautan berbahaya **dapat** diinjeksikan untuk mengarahkan pengguna ke situs eksternal  
- [ ] Ya — elemen `<form>` fungsional **dapat** diinjeksikan untuk memanen kredensial *(Sedang)*  

### Apakah header keamanan atau Content Security Policy (CSP) memitigasi dampak dari injeksi?
- [ ] Ya — CSP yang restriktif telah diterapkan dan `form-action` atau `frame-src` **telah diberlakukan**  
- [ ] Tidak — header keamanan tersedia tetapi CSP **dinonaktifkan** atau mengandung unsafe-inline/wildcards  
- [ ] Tidak — tidak ada CSP atau header keamanan relevan yang **diaktifkan**  

---