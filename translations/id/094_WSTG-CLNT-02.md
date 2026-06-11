## WSTG-CLNT-02 — Pengujian Eksekusi JavaScript

Pengujian eksekusi JavaScript berfokus pada mengidentifikasi instansi di mana aplikasi salah menangani data yang diberikan pengguna, yang menyebabkan eksekusi skrip tidak sah dalam konteks peramban (browser) korban. Kerentanan ini biasanya menghasilkan Cross-Site Scripting (XSS), yang memungkinkan penyerang mengeksfiltrasi cookie sesi, memanipulasi Document Object Model (DOM), atau melakukan tindakan atas nama pengguna. Penetration tester memeriksa sink seperti `innerHTML`, `document.write()`, dan `eval()` di mana data yang tidak disanitasi dari sumber (source) seperti fragmen URL, `localStorage`, atau `window.name` mungkin diproses. Eksploitasi yang berhasil mengompromikan integritas dan kerahasiaan sesi pengguna serta dapat berfungsi sebagai pivot untuk serangan sisi klien (client-side) yang lebih kompleks.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CLNT-02 |
| **CWE** | CWE-79 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/02-Testing_for_JavaScript_Execution  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/dom-based  
* https://portswigger.net/web-security/prototype-pollution  

**Alat:** `Burp Suite`, `Browser Developer Tools`, `XSStrike`, `DOMPurify`, `KNOXSS`

### Apakah aplikasi memproses data yang diberikan pengguna dalam JavaScript sink yang berbahaya?
- [ ] Tidak — semua data disanitasi atau digunakan dalam sink yang aman seperti `textContent`  
- [ ] Ya — data diproses dalam sink yang berbahaya tetapi sanitasi/encoding **diterapkan**  
- [ ] Ya — data diproses dalam sink yang berbahaya dan sanitasi **tidak diterapkan**  

### Apakah terdapat Content Security Policy (CSP) untuk memitigasi eksekusi skrip?
- [ ] Ya — CSP yang restriktif **diaktifkan** dan bypass **tidak memungkinkan**  
- [ ] Ya — CSP **diaktifkan** tetapi bypass **memungkinkan** melalui `unsafe-inline` atau CDN yang tidak aman  
- [ ] Tidak — tidak ada CSP yang **diaktifkan**  

### Bisakah JavaScript dieksekusi melalui parameter atau fragmen URL (DOM-based XSS)?
- [ ] Tidak — input di-encode dengan benar dan eksekusi **tidak memungkinkan**  
- [ ] Ya — input tercermin dalam DOM tetapi eksekusi **dicegah** oleh perlindungan framework modern  
- [ ] Ya — input dieksekusi langsung di peramban, dan eksploitasi **memungkinkan**  

### Apakah event handler atau skema URI rentan terhadap injeksi skrip?
- [ ] Tidak — input disanitasi untuk menghapus atribut event dan skema `javascript:`  
- [ ] Ya — event handler **dapat** diinjeksi tetapi filter membatasi kompleksitas Payload  
- [ ] Ya — eksekusi JavaScript sewenang-wenang melalui atribut `onerror`, `onload`, atau `href` **memungkinkan**  

---