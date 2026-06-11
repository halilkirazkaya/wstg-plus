## WSTG-CLNT-01 — Testing for DOM Based Cross Site Scripting

DOM-based Cross-Site Scripting (DOM XSS) terjadi ketika sebuah aplikasi mengandung JavaScript sisi klien (client-side) yang memproses data dari sumber yang tidak terpercaya dengan cara yang tidak aman, biasanya dengan menulis data tersebut kembali ke Document Object Model (DOM) melalui sebuah sink yang berbahaya. Berbeda dengan reflected atau stored XSS, kerentanan ini sepenuhnya berada di dalam kode sisi klien; Payload sering kali tidak dikirim ke server sama sekali, karena diproses oleh sink seperti `innerHTML`, `eval()`, atau `document.write()`. Penyerang mengeksploitasi hal ini dengan membuat URL yang berisi fragmen atau parameter berbahaya yang, ketika dimuat oleh peramban (browser) korban, akan mengeksekusi JavaScript arbitrer dalam konteks sesi pengguna. Hal ini dapat menyebabkan eksfiltrasi token sesi, pencurian data sensitif, dan tindakan tidak sah yang dilakukan atas nama pengguna terautentikasi.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CLNT-01 |
| **CWE** | CWE-79 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Kerentanan** | Tinggi |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/01-Testing_for_DOM-based_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  
* https://portswigger.net/web-security/cross-site-scripting  
* https://portswigger.net/web-security/dom-based  

**Alat:** `Burp Suite (DOM Invader)`, `Browser DevTools`, `KNOXSS`, `XSStrike`, `Snyk (SAST)`

### Apakah sumber yang tidak terpercaya digunakan untuk menangkap data yang dikendalikan pengguna dalam JavaScript?
- [ ] Tidak — JavaScript **tidak** menggunakan `location.hash`, `location.search`, atau `document.referrer`  
- [ ] Ya — sumber yang tidak terpercaya digunakan tetapi data **tidak** diteruskan ke sink eksekusi mana pun  
- [ ] Ya — sumber yang tidak terpercaya digunakan dan data mengalir langsung ke logika sisi klien  

### Apakah data yang dikendalikan pengguna diteruskan ke sink DOM yang berbahaya?
- [ ] Tidak — sink seperti `innerHTML`, `outerHTML`, `document.write()`, atau `eval()` **tidak** digunakan  
- [ ] Ya — sink berbahaya ditemukan tetapi data **disanitasi** secara ketat menggunakan pustaka yang aman seperti DOMPurify  
- [ ] Ya — sink berbahaya ditemukan dan data diproses dengan penyaringan berbasis regex yang **tidak memadai** atau kustom  
- [ ] Ya — sink berbahaya ditemukan dan data diteruskan **tanpa** sanitasi atau encoding apa pun *(Kritis)*  

### Dapatkah sink eksekusi dijangkau melalui payload berbasis URL?
- [ ] Tidak — aliran dari sumber ke sink (source-to-sink) **tidak dapat** dipicu melalui input eksternal  
- [ ] Ya — aliran **mungkin terjadi** tetapi memerlukan interaksi pengguna yang kompleks  
- [ ] Ya — aliran **mungkin terjadi** dan dapat dipicu melalui fragmen URL atau parameter query yang sederhana  

### Apakah header keamanan modern atau mitigasi berbasis peramban menetralkan risiko tersebut?
- [ ] Ya — `Content-Security-Policy` (CSP) yang ketat dengan `script-src` dan `trusted-types` telah **diaktifkan** dan mencegah eksekusi  
- [ ] Ya — CSP tersedia tetapi `unsafe-inline` atau hash yang lemah membuat bypass menjadi **mungkin**  
- [ ] Tidak — tidak ada CSP yang diterapkan untuk memitigasi eksekusi berbasis DOM  

### Apakah aplikasi menggunakan framework sisi klien dengan perlindungan bawaan?
- [ ] Ya — framework (misalnya, Angular, React) secara otomatis melakukan encoding data dan bypass **tidak mungkin dilakukan**  
- [ ] Ya — framework digunakan tetapi "escape hatches" seperti `dangerouslySetInnerHTML` **diaktifkan**  
- [ ] Tidak — aplikasi menggunakan vanilla JavaScript atau pustaka lama (misalnya, versi jQuery lama) tanpa auto-escaping  

---