## WSTG-INPV-18 — Testing for Server-Side Template Injection

Server-Side Template Injection (SSTI) terjadi ketika sebuah aplikasi menyematkan masukan yang dikontrol pengguna ke dalam mesin templat (*template engine*) sisi server tanpa sanitasi atau *escaping* yang memadai. Hal ini memungkinkan penyerang untuk menyuntikkan direktif templat berbahaya, yang kemudian dieksekusi di server, berpotensi menyebabkan kompromi sistem secara penuh melalui Remote Code Execution (RCE). SSTI biasanya muncul pada fitur-fitur seperti pembuatan email dinamis, halaman profil, dan sistem notifikasi yang menggunakan mesin seperti Jinja2, Twig, atau FreeMarker. Dari sudut pandang penyerang, kerentanan ini sering dieksploitasi untuk membocorkan variabel lingkungan (*environment variables*) yang sensitif, membaca berkas internal, atau mengeksekusi perintah sistem dengan memanfaatkan objek dan metode bawaan dari mesin templat tersebut.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-18 |
| **CWE** | CWE-1336 |
| **Test Status** | Not Performed |
| **Severity** | Critical |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/18-Testing_for_Server-side_Template_Injection  
* https://hacktricks.wiki/en/pentesting-web/ssti-server-side-template-injection/index.html  
* https://portswigger.net/web-security/server-side-template-injection  

**Tools:** `tplmap`, `Burp Suite (Intruder/Repeater)`, `ffuf`, `SSTI-Payload-Generator`

### Apakah aplikasi menggunakan mesin templat sisi server untuk memproses masukan pengguna?
- [ ] Tidak — aplikasi **tidak** menggunakan templat sisi server untuk konten dinamis  
- [ ] Ya — templat digunakan tetapi masukan pengguna **tidak** disematkan dalam direktif templat  
- [ ] Ya — masukan pengguna disematkan dalam templat **tanpa** sanitasi yang tepat  

### Apakah ekspresi aritmetika dasar dievaluasi saat disuntikkan ke dalam kolom masukan?
- [ ] Tidak — *payload* seperti `{{7*7}}` atau `${7*7}` direfleksikan sebagai string literal  
- [ ] Ya — ekspresi dievaluasi (misalnya, mengembalikan `49`), mengonfirmasi bahwa mesin templat **rentan**  

### Bisakah objek atau metode bawaan mesin templat diakses?
- [ ] Tidak — akses ke objek, metode, atau konfigurasi berbahaya **dibatasi** atau di-*sandbox*  
- [ ] Ya — objek bawaan (misalnya, `self`, `config`, `__class__`) **dapat** diakses untuk membocorkan data sensitif  
- [ ] Ya — Remote Code Execution (RCE) melalui panggilan sistem atau pustaka OS **mungkin dilakukan**  

### Apakah terdapat mekanisme sandbox atau allowlist yang mencegah eksekusi direktif berbahaya?
- [ ] Ya — *allowlist* yang ketat atau *sandbox* yang aman telah diterapkan dan bypass **tidak dimungkinkan**  
- [ ] Ya — *sandbox* tersedia tetapi bypass **mungkin dilakukan** melalui *payload* spesifik yang bergantung pada mesin  
- [ ] No — tidak ada *sandbox* atau *allowlist* yang **diterapkan** pada mesin templat  

---