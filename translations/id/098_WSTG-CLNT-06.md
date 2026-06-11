## WSTG-CLNT-06 — Testing for Client-Side Resource Manipulation

Manipulasi sumber daya sisi klien (Client-side resource manipulation) terjadi ketika aplikasi mengizinkan masukan yang dikontrol pengguna untuk memengaruhi sumber atau jalur (path) dari sumber daya yang dimuat oleh browser, seperti skrip, stylesheet, atau iframe. Dengan memanipulasi fragmen URI, parameter kueri, atau sumber DOM lainnya, penyerang dapat memaksa aplikasi untuk memuat konten eksternal yang berbahaya atau menjalankan kode yang tidak sah. Kerentanan ini biasanya ditemukan dalam logika JavaScript yang secara dinamis mengisi atribut `src` atau `href` dari elemen HTML tanpa validasi yang memadai terhadap daftar izinkan (allow-list) yang terpercaya. Dari perspektif penyerang, hal ini dieksploitasi dengan membuat URL yang mengalihkan pemuatan sumber daya ke server yang dikendalikan penyerang, yang berpotensi menghasilkan DOM-based Cross-Site Scripting (XSS), eksfiltrasi data sensitif, atau UI redressing.


| Kolom | Nilai |
|---|---|
| **WSTG ID** | WSTG-CLNT-06 |
| **CWE** | CWE-99 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan (Severity)** | Sedang / Tinggi |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/06-Testing_for_Client-side_Resource_Manipulation  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  

**Alat:** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `grep`, `Snyk`, `LinkFinder`

### Apakah aplikasi menggunakan sink sisi klien untuk memuat atau menyematkan sumber daya secara dinamis?
- [ ] Tidak — sumber daya dimuat hanya melalui HTML statis atau logika sisi server  
- [ ] Ya — JavaScript sisi klien secara dinamis mengisi atribut sumber daya seperti `src`, `href`, atau `data`  

### Apakah kontrol validasi atau daftar izinkan (allow-lists) diterapkan untuk sumber URI sumber daya?
- [ ] Ya — daftar izinkan yang ketat dan validasi asal (origin) **diterapkan** pada semua sumber daya dinamis  
- [ ] Ya — validasi ada tetapi mengandalkan daftar blokir (blacklists) yang lemah atau regex yang mudah dilewati  
- [ ] Tidak — tidak ada kontrol validasi yang diterapkan pada jalur sumber daya yang dikontrol pengguna  

### Apakah mungkin untuk melewati (bypass) validasi URI menggunakan protokol alternatif atau pengodean?
- [ ] Tidak — bypass terhadap filter sumber daya **tidak dimungkinkan**  
- [ ] Ya — bypass **dimungkinkan** menggunakan protokol yang berbeda seperti `data:`, `vbscript:`, atau `javascript:`  
- [ ] Ya — bypass **dimungkinkan** menggunakan karakter terenkode atau null bytes untuk mengakali pencocokan string sederhana  

### Dapatkah penyerang mengalihkan pemuatan sumber daya ke domain eksternal yang tidak terpercaya?
- [ ] Tidak — jalur sumber daya dibatasi pada asal yang sama (same origin) atau subdirektori tetap  
- [ ] Ya — aplikasi **dapat** dipaksa untuk memuat skrip atau gaya dari domain eksternal sewenang-wenang  

### Apa dampak maksimum dari manipulasi sumber daya tersebut?
- [ ] Rendah — manipulasi hanya memengaruhi elemen yang tidak dapat dieksekusi seperti gambar atau teks yang tidak sensitif  
- [ ] Sedang — manipulasi memungkinkan CSS injection atau UI redressing yang signifikan  
- [ ] Tinggi — manipulasi menyebabkan DOM-based XSS atau eksekusi JavaScript sewenang-wenang *(Kritis)*  

---