## WSTG-INFO-01 — Melakukan Penemuan Mesin Pencari dan Pengintaian untuk Kebocoran Informasi

Penemuan mesin pencari (Search engine discovery) melibatkan pemanfaatan mesin pencari publik, halaman cache (cached pages), dan layanan pengindeksan untuk mengidentifikasi informasi yang secara tidak sengaja diekspos oleh organisasi target. Penyerang menggunakan operator pencarian tingkat lanjut (Google Dorks) dan layanan pengindeksan pihak ketiga untuk menemukan berkas sensitif, direktori, portal login, pesan kesalahan (error messages), dan metadata yang seharusnya tidak dapat diakses secara publik. Fase pengintaian (reconnaissance) ini sangat penting karena tidak memerlukan interaksi langsung dengan target, sehingga hampir tidak terdeteksi sementara berpotensi mengungkapkan titik masuk bernilai tinggi seperti panel admin yang terekspos, berkas konfigurasi, dump basis data, dan dokumentasi internal.

| Kolom | Nilai |
|---|---|
| **WSTG ID** | WSTG-INFO-01 |
| **CWE** | CWE-200 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Sedang |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/01-Conduct_Search_Engine_Discovery_Reconnaissance_for_Information_Leakage  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/external-recon-methodology/index.html  
* https://portswigger.net/web-security/information-disclosure  

**Alat:** `Google Dorks`, `theHarvester`, `Shodan`, `Censys`, `Wayback Machine`, `Recon-ng`, `SearchDiggity`

### Apakah berkas atau direktori sensitif diindeks oleh mesin pencari?
- [ ] Tidak — tidak ada konten sensitif yang ditemukan dalam hasil mesin pencari  
- [ ] Ya — berkas konfigurasi, cadangan (backups), atau dokumen internal **terindeks** *(Kritis)*  

### Apakah Google Dorking mengungkapkan antarmuka administratif atau login yang terekspos?
- [ ] Tidak — panel admin dan halaman login **tidak** terindeks  
- [ ] Ya — panel admin **terindeks** tetapi memerlukan autentikasi multi-faktor yang kuat  
- [ ] Ya — panel admin **terindeks** dan dapat diakses secara publik **tanpa** kontrol yang memadai  

### Apakah versi halaman dalam cache mengekspos data sensitif yang sudah dihapus?
- [ ] Tidak — tidak ada data sensitif yang ditemukan dalam snapshot cache  
- [ ] Ya — halaman dalam cache berisi kredensial, alamat IP internal, atau informasi sensitif  

### Apakah berkas `robots.txt` secara tidak sengaja mengungkapkan jalur (paths) sensitif?
- [ ] Tidak — `robots.txt` **tidak** mengungkapkan struktur direktori yang sensitif  
- [ ] Ya — `robots.txt` mencantumkan jalur sensitif yang membantu pengintaian (reconnaissance) penyerang  
- [ ] Berkas `robots.txt` tidak ada  

### Apakah layanan pihak ketiga (Shodan, Censys, Wayback Machine) mengungkapkan paparan historis atau saat ini?
- [ ] Tidak — tidak ada temuan signifikan dari layanan pengindeksan pihak ketiga  
- [ ] Ya — snapshot historis atau pemindaian layanan mengungkapkan metadata sensitif atau banner layanan

---

## WSTG-INFO-02 — Fingerprint Web Server

Fingerprinting web server adalah proses mengidentifikasi tipe perangkat lunak tertentu, versi, dan sistem operasi yang mendasari dari sebuah web server target. Penyerang melakukan pengintaian (reconnaissance) ini untuk mengidentifikasi kerentanan yang diketahui, seperti CVE yang belum ditambal atau kelemahan konfigurasi, yang spesifik untuk versi Apache, Nginx, IIS, atau teknologi server lainnya. Hal ini biasanya dicapai dengan menganalisis header respons HTTP (misalnya, `Server`, `X-Powered-By`), memeriksa perilaku protokol yang unik, atau mengamati informasi yang bocor melalui halaman kesalahan (error pages) default dan file sistem. Fingerprinting yang berhasil memungkinkan penyerang untuk menyesuaikan strategi eksploitasi mereka, berpindah dari pemindaian generik ke serangan dengan probabilitas tinggi terhadap celah arsitektur yang diketahui.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INFO-02 |
| **CWE** | CWE-200 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Informasional / Rendah* |

> *Tingkat keparahan menjadi Tinggi (High) jika fingerprinting mengungkapkan versi server yang usang atau sudah mencapai akhir masa pakai (end-of-life/EOL) dengan kerentanan kritis yang diketahui.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/02-Fingerprint_Web_Server  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**Alat:** `curl`, `Nmap`, `WhatWeb`, `Wappalyzer`, `Nikto`, `Netcat`

### Apakah terdapat string identifikasi dalam header respons HTTP?
- [ ] Tidak — header `Server` dan `X-Powered-By` **tidak ada** atau berisi nilai generik  
- [ ] Ya — header mengungkapkan perangkat lunak server tetapi **tidak** nomor versi spesifik  
- [ ] Ya — header mengungkapkan perangkat lunak server spesifik dan nomor versi **persis**  

### Apakah halaman kesalahan default atau respons buatan sistem membocorkan rincian server?
- [ ] Tidak — halaman kesalahan kustom digunakan dan **tidak** mengungkapkan informasi server  
- [ ] Ya — halaman kesalahan default membocorkan nama perangkat lunak server dan/atau versi  
- [ ] Ya — halaman kesalahan membocorkan rincian server, versi, dan **sistem operasi yang mendasari**  

### Apakah versi server diungkapkan melalui file default atau struktur direktori?
- [ ] Tidak — file instalasi default, panduan, dan skrip pengujian telah **dihapus**  
- [ ] Ya — file default (misalnya, `info.php`, `manual/`, `test.html`) **tersedia** dan mengungkapkan data versi  

### Dapatkah tipe server disimpulkan secara akurat melalui analisis perilaku yang unik?
- [ ] Tidak — respons server dinormalisasi dan fingerprinting berbasis perilaku **tidak dimungkinkan**  
- [ ] Ya — perilaku unik (misalnya, urutan header, kode kesalahan tertentu, atau keunikan stack TCP/IP) **memungkinkan** identifikasi server  

### Apakah versi server yang teridentifikasi saat ini didukung dan telah ditambal (patched)?
- [ ] Ya — versi yang teridentifikasi adalah versi terbaru dan **didukung** oleh vendor  
- [ ] Tidak — versi yang teridentifikasi sudah **usang** atau **end-of-life (EOL)** dengan kerentanan yang diketahui

---

## WSTG-INFO-03 — Meninjau Metafile Webserver untuk Kebocoran Informasi (Information Leakage)

Metafile webserver seperti `robots.txt`, `sitemap.xml`, dan `security.txt` dimaksudkan untuk memandu crawler dan menyediakan metadata administratif, namun seringkali secara tidak sengaja mengungkapkan struktur direktori yang sensitif atau endpoint yang tersembunyi. Penyerang menganalisis file-file ini untuk melakukan enumerasi terhadap attack surface aplikasi, mengidentifikasi arahan "Disallow" yang sering kali merujuk pada panel administratif yang tidak tertaut, direktori cadangan (backup), atau lingkungan pengembangan (development environment). Selain instruksi mesin pencari, file dalam direktori `.well-known/` atau file seperti `humans.txt` dapat mengungkapkan technology stack, informasi pengembang, atau kontak keamanan organisasi. Peninjauan sistematis terhadap file-file ini memungkinkan penguji untuk mendapatkan wawasan struktural dan teknis tanpa melakukan penemuan Brute Force yang agresif.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INFO-03 |
| **CWE** | CWE-200 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Low / Informational* |

> *Tingkat keparahan menjadi Medium jika metafile mengungkapkan antarmuka administratif tanpa autentikasi atau cadangan konfigurasi yang sensitif.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/03-Review_Webserver_Metafiles_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `curl`, `wget`, `Burp Suite`, `FFUF`, `GoBuster`

### Apakah file `robots.txt` ada dan mengekspos direktori sensitif?
- [ ] Tidak — `robots.txt` **tidak** ada  
- [ ] Ya — `robots.txt` ada tetapi hanya berisi jalur publik standar  
- [ ] Ya — `robots.txt` mengungkapkan jalur direktori sensitif atau antarmuka administratif  
- [ ] Ya — `robots.txt` mengungkapkan kredensial atau versi perangkat lunak tertentu dalam komentar  

### Apakah file `sitemap.xml` ada dan mengungkapkan struktur aplikasi yang tersembunyi?
- [ ] Tidak — `sitemap.xml` **tidak** ada  
- [ ] Ya — `sitemap.xml` ada tetapi hanya mencantumkan URL publik  
- [ ] Ya — `sitemap.xml` mencantumkan endpoint yang tidak tertaut atau sumber daya internal yang **dapat** diakses  

### Apakah metafile keamanan dan organisasi mengekspos detail teknis?
- [ ] Tidak — tidak ada file metadata yang ditemukan di root atau `.well-known/`  
- [ ] Ya — `security.txt` atau `humans.txt` ada tetapi berisi informasi standar  
- [ ] Ya — metafile mengungkapkan hostname internal, identitas pengembang, atau detail technology stack yang **dapat** membantu dalam social engineering atau serangan lebih lanjut  

### Apakah terdapat metafile warisan (legacy) atau metafile khusus framework yang ada?
- [ ] Tidak — tidak ditemukan file khusus framework (contoh: `info.php`, `composer.json`, `package.json`)  
- [ ] Ya — file seperti `README.md`, `CHANGELOG.txt`, atau `.DS_Store` ada tetapi telah dibersihkan (sanitized)  
- [ ] Ya — file khusus framework atau versi tertentu ada dan **memungkinkan** dilakukannya fingerprinting teknologi yang tepat

---

## WSTG-INFO-04 — Mengenumerasi Aplikasi pada Webserver

Enumerasi aplikasi adalah proses mengidentifikasi semua aplikasi dan layanan web yang dihosting pada satu server web atau alamat IP tunggal. Karena server web modern sering kali menghosting beberapa virtual hosts, lingkungan staging lama, atau antarmuka administratif pada port dan jalur non-standar, kegagalan dalam mengidentifikasi aset tersembunyi ini menyebabkan sebagian besar attack surface tidak teruji. Penyerang menggunakan teknik seperti DNS zone transfers, virtual host brute-forcing, dan reverse IP lookups untuk menemukan entry points yang terlupakan atau tersembunyi ini, yang sering kali kurang aman dibandingkan aplikasi produksi utama.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INFO-04 |
| **CWE** | CWE-200 |
| **Status Pengujian** | Belum Dilakukan |
| **Severitas** | Informasional / Rendah |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/04-Attack_Surface_Identification  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/external-recon-methodology/index.html  

**Alat:** `nmap`, `ffuf`, `gobuster`, `theHarvester`, `Dig`, `DNSrecon`, `Reverse IP Lookups`, `Shodan`

### Apakah terdapat beberapa alamat IP atau alias DNS yang terkait dengan infrastruktur target?
- [ ] Tidak — hanya terdapat satu IP dan A-record tunggal  
- [ ] Ya — beberapa alamat IP atau alias **telah** diidentifikasi, sehingga memperluas cakupan (scope)  

### Apakah server web menghosting beberapa Virtual Hosts (vhosts) pada IP yang sama?
- [ ] Tidak — hanya domain utama yang dilayani oleh server  
- [ ] Ya — virtual hosts tambahan diidentifikasi tetapi akses **tidak dimungkinkan** (misalnya, 403 Forbidden)  
- [ ] Ya — virtual hosts yang tersembunyi, untuk pengembangan, atau staging **telah** diidentifikasi dan dapat diakses  

### Apakah aplikasi dihosting pada port non-standar?
- [ ] Tidak — hanya port standar (80/443) yang terbuka  
- [ ] Ya — port non-standar terbuka tetapi **tidak** menghosting aplikasi web  
- [ ] Ya — aplikasi web tambahan **telah** diidentifikasi pada port non-standar (misalnya, 8080, 8443, 9000)  

### Apakah aplikasi yang berbeda dipetakan ke jalur URI tertentu?
- [ ] Tidak — satu aplikasi tunggal melayani seluruh direktori root  
- [ ] Ya — beberapa aplikasi **telah** ditemukan melalui enumerasi direktori (misalnya, `/api`, `/portal`, `/backup`)  

### Apakah layanan reconnaissance pihak ketiga mengungkapkan aplikasi historis atau sekunder?
- [ ] Tidak — tidak ada temuan signifikan dari Shodan, Censys, atau Wayback Machine  
- [ ] Ya — snapshot historis atau pemindaian layanan mengungkapkan aplikasi yang saat ini **aktif** tetapi tidak tertaut (unlinked)

---

## WSTG-INFO-05 — Meninjau Konten Halaman Web untuk Kebocoran Informasi

Meninjau konten halaman web melibatkan inspeksi manual dan otomatis terhadap respons aplikasi, termasuk file HTML, JavaScript, dan CSS, untuk mengidentifikasi informasi sensitif yang tidak sengaja terpapar kepada pengguna. Penyerang meneliti kode sumber sisi klien (client-side source code) untuk mencari komentar pengembang, field formulir tersembunyi, jalur server internal, API key yang di-hardcode, dan informasi debugging yang dapat membantu fase eksploitasi lebih lanjut. Kebocoran ini sering terjadi ketika pengembang lupa menghapus metadata atau kode debugging sebelum pindah ke lingkungan produksi, yang berpotensi mengungkapkan celah logika atau detail infrastruktur backend. Dari perspektif penyerang, hal ini menyediakan metode dengan upaya rendah (low-effort) untuk memetakan struktur internal aplikasi dan menemukan titik masuk yang terabaikan tanpa memicu peringatan keamanan atau berinteraksi dengan logika backend server.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INFO-05 |
| **CWE** | CWE-200 |
| **Status Pengujian** | Tidak Dilakukan |
| **Tingkat Keparahan** | Rendah / Sedang* |

> *Tingkat keparahan menjadi Tinggi jika ditemukan kredensial yang di-hardcode, kunci privat (private keys), atau variabel lingkungan internal yang sensitif.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/05-Review_Web_Page_Content_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**Alat:** `Burp Suite (Target/Proxy)`, `Browser Developer Tools`, `SecretFinder`, `grep`, `truffleHog`, `LinkFinder`

### Apakah terdapat komentar pengembang yang berisi informasi sensitif di dalam kode sumber?
- [ ] Tidak — tidak ada komentar atau hanya ditemukan komentar umum yang tidak sensitif  
- [ ] Ya — komentar tersedia namun **tidak** berisi logika atau data sensitif  
- [ ] Ya — komentar **mengungkapkan** jalur internal, query SQL, atau instruksi administratif  
- [ ] Ya — komentar **mengungkapkan** kredensial atau catatan pengembang yang sensitif *(Tinggi)*  

### Apakah field input HTML tersembunyi berisi data sensitif atau informasi status (state)?
- [ ] Tidak — tidak ditemukan field tersembunyi atau hanya berisi token yang tidak sensitif  
- [ ] Ya — field tersembunyi digunakan untuk status (state) namun **terenkripsi** atau **ditandatangani (signed)**  
- [ ] Ya — field tersembunyi berisi password **plaintext**, peran pengguna, atau ID internal  

### Apakah terdapat secret, API key, atau alamat IP internal yang di-hardcode di dalam file JavaScript?
- [ ] Tidak — tidak ditemukan string sensitif atau secret dalam file JS  
- [ ] Ya — file JS berisi API key publik dengan pembatasan yang **tepat**  
- [ ] Ya — file JS berisi API key yang **tidak terlindungi**, IP internal, atau endpoint privat  
- [ ] Ya — file JS berisi kredensial administratif atau kunci privat yang **di-hardcode** *(Kritis)*  

### Apakah aplikasi mengungkapkan versi framework atau jalur file internal di dalam kode sumber?
- [ ] Tidak — informasi framework dan jalur file **diminifikasi** atau **diobfuskasi**  
- [ ] Ya — nama dan versi framework **terlihat** namun telah dipatch  
- [ ] Ya — jalur file internal atau jalur server absolut **terekspos** dalam kode sumber  

### Apakah kode sumber rentan bocor karena kurangnya minifikasi atau obfuskasi?
- [ ] Tidak — kode produksi **diminifikasi sepenuhnya** dan bersih dari komentar  
- [ ] Ya — kode diminifikasi namun komentar **tetap ada** dalam sumber  
- [ ] Ya — source map (file `.map`) **dapat diakses secara publik**, memungkinkan rekonstruksi penuh dari kode sumber

---

## WSTG-INFO-06 — Mengidentifikasi Titik Masuk Aplikasi (Identify Application Entry Points)

Mengidentifikasi titik masuk aplikasi melibatkan pemetaan seluruh permukaan serangan (attack surface) dengan mengenumerasi semua kemungkinan cara pengguna atau sistem eksternal dapat berinteraksi dengan aplikasi. Proses ini mencakup penemuan semua URL, endpoint API, parameter (GET, POST, berbasis cookie), header HTTP, dan protokol khusus seperti WebSockets atau gRPC. Bagi seorang penetration tester, tahapan ini sangat krusial karena kerentanan sering ditemukan pada API "bayangan" (shadow API) yang tidak terdokumentasi, endpoint legacy, atau struktur parameter kompleks yang terlewatkan selama pengembangan. Enumerasi yang menyeluruh memastikan tidak ada komponen aplikasi yang tetap menjadi black box yang tidak teruji, sehingga mencegah penyerang menemukan rute yang tidak terpantau ke dalam sistem.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INFO-06 |
| **CWE** | CWE-1059 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Informasional |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/06-Identify_Application_Entry_Points  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite (Target/Proxy)`, `OWASP ZAP`, `ffuf`, `Arjun`, `LinkFinder`, `GoSpider`, `KiteRunner`

### Apakah semua parameter GET dan POST telah dienumerasi di seluruh aplikasi?
- [ ] Ya — enumerasi komprehensif melalui crawling otomatis dan manual **telah selesai**  
- [ ] Ya — hanya parameter umum yang telah diidentifikasi melalui crawling standar  
- [ ] Tidak — parameter sebagian besar **belum diidentifikasi** atau belum diuji  

### Apakah parameter yang tidak terdokumentasi atau tersembunyi (misalnya, debug, admin) dicari menggunakan fuzzing?
- [ ] Tidak — penemuan parameter tersembunyi **tidak diperlukan** untuk cakupan (scope) spesifik ini  
- [ ] Ya — fuzzing parameter (misalnya, menggunakan `Arjun`) **telah dilakukan** tanpa temuan  
- [ ] Ya — parameter tersembunyi **berhasil ditemukan** dan dipetakan untuk pengujian lebih lanjut  
- [ ] Tidak — penemuan parameter tersembunyi **tidak dilakukan**  

### Apakah semua metode HTTP yang didukung (PUT, DELETE, PATCH, dll.) telah diidentifikasi untuk setiap endpoint?
- [ ] Ya — penemuan metode **diterapkan** di seluruh endpoint yang ditemukan  
- [ ] Ya — penemuan metode **diterapkan** hanya pada endpoint yang berisiko tinggi atau sensitif  
- [ ] Tidak — hanya metode standar GET dan POST yang **diidentifikasi**  

### Apakah titik masuk non-standar seperti WebSockets, gRPC, atau header kustom telah dipetakan?
- [ ] Tidak — aplikasi **tidak** menggunakan protokol non-standar atau header kustom  
- [ ] Ya — semua titik masuk spesifik protokol dan header kustom **telah diidentifikasi**  
- [ ] Tidak — titik masuk non-standar **tersedia** namun belum dipetakan  

### Apakah seluruh permukaan API (termasuk endpoint versi seperti /v1/ atau /v2/) telah dipetakan sepenuhnya?
- [ ] Tidak — aplikasi **tidak** mengekspos API  
- [ ] Ya — semua versi dan endpoint API **telah diidentifikasi** dan didokumentasikan  
- [ ] Ya — hanya versi API produksi saat ini yang **diidentifikasi**  
- [ ] Tidak — endpoint API **tetap tidak terpetakan** atau hanya ditemukan sebagian

---

## WSTG-INFO-07 — Memetakan Jalur Eksekusi Melalui Aplikasi

Memetakan jalur eksekusi melibatkan identifikasi sistematis terhadap semua endpoint yang dapat dijangkau, aliran fungsional, dan percabangan pengambilan keputusan di dalam aplikasi web. Proses ini sangat penting untuk memastikan bahwa tidak ada fungsionalitas "gelap" (dark functionality)—seperti kode lama (legacy code), antarmuka debug, atau rute API yang tidak terdokumentasi—tetap tersembunyi dari pengawasan keamanan, karena hal-hal tersebut sering kali kekurangan kontrol keamanan modern. Penyerang menggunakan kombinasi automated spidering dan eksplorasi manual untuk memvisualisasikan struktur aplikasi, yang sering kali menemukan kerentanan seperti akses administratif yang tidak sah atau kelemahan logika bisnis (business logic flaws) selama proses tersebut. Dengan mengkorelasikan input ke respons sisi server tertentu, penguji dapat menentukan titik logika pemrosesan sensitif yang memerlukan pengujian mendalam yang terarah guna memastikan cakupan yang kuat.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INFO-07 |
| **CWE** | CWE-200 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Informasi |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/07-Map_Execution_Paths_Through_Application  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite Professional`, `OWASP ZAP`, `Katana`, `FFUF`, `LinkFinder`, `GoBuster`, `ParamMiner`

### Apakah aplikasi telah diindeks sepenuhnya menggunakan crawling otomatis dan manual?
- [ ] Ya — peta komprehensif dari semua sumber daya yang terlihat dan tertaut **telah selesai**  
- [ ] Ya — crawling otomatis **telah selesai** tetapi eksplorasi manual masih tertunda  
- [ ] No — aplikasi **belum** diindeks  

### Apakah endpoint tersembunyi atau tidak terdokumentasi diidentifikasi melalui analisis JS atau directory brute-forcing?
- [ ] Tidak — tidak ada jalur tidak terdokumentasi yang ditemukan melalui analisis side-channel  
- [ ] Ya — jalur tersembunyi ditemukan tetapi **tidak dapat** diakses tanpa otorisasi  
- [ ] Ya — endpoint tidak terdokumentasi **dapat** diakses dan menyediakan fungsionalitas sensitif *(Kritis / Tinggi)*  

### Bisakah jalur eksekusi diubah melalui parameter atau header non-standar?
- [ ] Tidak — parameter seperti `debug`, `test`, atau `admin` **tidak** memengaruhi alur eksekusi  
- [ ] Ya — parameter mengubah output tetapi **tidak** melewati (bypass) kontrol keamanan  
- [ ] Ya — parameter pengubah jalur **diterapkan** dan memungkinkan bypass logika yang dimaksudkan  

### Apakah alur logika bisnis multi-langkah (misalnya, multi-factor auth, checkout) telah dipetakan sepenuhnya?
- [ ] Ya — semua status (state) dan transisi dalam proses multi-langkah **telah diidentifikasi**  
- [ ] Tidak — transisi state machine yang kompleks **tidak dapat** dipetakan sepenuhnya  

### Apakah file dokumentasi API (Swagger/OpenAPI) atau peta sisi klien (Source Maps) terekspos?
- [ ] Tidak — tidak ada peta arsitektur atau file dokumentasi yang terekspos secara publik  
- [ ] Ya — dokumentasi tersedia tetapi **dilindungi** oleh autentikasi  
- [ ] Ya — Swagger UI, OpenAPI JSON, atau JS Source Maps **diaktifkan** dan dapat diakses secara publik

---

## WSTG-INFO-08 — Fingerprint Web Application Framework

Fingerprinting framework aplikasi web melibatkan identifikasi tumpukan teknologi (technology stack) spesifik dan versi perangkat lunak yang digunakan untuk membangun dan menyajikan aplikasi. Penyerang menganalisis HTTP response headers, cookie khusus framework, struktur file yang dapat diprediksi, dan artefak JavaScript unik untuk menentukan apakah target menjalankan platform seperti React, Angular, Django, atau Spring. Fase reconnaissance ini sangat penting karena memungkinkan penguji untuk mempersempit attack surface ke kerentanan yang diketahui (CVE) dan kelemahan konfigurasi umum yang spesifik untuk framework tersebut. Dengan memahami arsitektur yang mendasarinya, penyerang dapat beralih dari pengujian generik ke strategi eksploitasi yang sangat bertarget.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INFO-08 |
| **CWE** | CWE-200 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Informasional |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/08-Fingerprint_Web_Application_Framework  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Wappalyzer`, `BuiltWith`, `WhatWeb`, `nmap`, `Burp Suite`, `curl`

### Apakah HTTP response headers mengekspos nama atau versi framework?
- [ ] Tidak — header seperti `X-Powered-By` atau `Server` **tidak** ada atau telah disanitasi  
- [ ] Ya — nama framework ada tetapi versi **disembunyikan**  
- [ ] Ya — nama framework dan versi **keduanya terekspos**  

### Apakah cookie khusus framework atau struktur direktori dapat diidentifikasi?
- [ ] Tidak — artefak framework umum (misalnya, path `.js`, nama cookie) diobfuskasi atau diubah namanya  
- [ ] Ya — cookie khusus framework (misalnya, `JSESSIONID`, `PHPSESSID`, `csrftoken`) **dapat** diidentifikasi  
- [ ] Ya — struktur direktori (misalnya, `/wp-content/`, `_next/static/`) **terlihat** dan mengonfirmasi framework tersebut  

### Apakah pesan kesalahan atau komentar kode sumber mengungkapkan detail framework?
- [ ] Tidak — penanganan kesalahan (error handling) bersifat generik dan komentar dihapus  
- [ ] Ya — stack trace khusus framework **diaktifkan** dan terlihat dalam respons  
- [ ] Ya — kode sumber HTML berisi komentar atau tag metadata yang mengidentifikasi framework  

### Apakah ada kerentanan yang diketahui terkait dengan versi framework yang diidentifikasi?
- [ ] Tidak — framework yang diidentifikasi sudah mutakhir dengan **tidak ada** kerentanan publik yang diketahui  
- [ ] Ya — versi framework sudah usang dan CVE tertentu **dapat** diidentifikasi

---

## WSTG-INFO-09 — Fingerprint Web Application

Fingerprinting sebuah aplikasi web adalah proses mengidentifikasi framework tertentu, Content Management System (CMS), atau tumpukan teknologi (technology stack) yang mendasarinya beserta versi terkaitnya. Fase pengintaian (reconnaissance) ini sangat penting karena memungkinkan penyerang untuk memetakan komponen yang teridentifikasi terhadap kerentanan yang diketahui (CVE) dan exploit publik, sehingga mempersempit permukaan serangan (attack surface) secara signifikan. Informasi biasanya dikumpulkan dari header respons HTTP, jalur file (file path) yang unik, nama cookie, metadata kode sumber HTML, dan hash kriptografis (cryptographic hashes) dari aset statis. Dengan menentukan technology stack secara akurat, penyerang dapat beralih dari pemindaian otomatis generik ke eksploitasi (exploitation) yang sangat tertarget pada kelemahan spesifik versi.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INFO-09 |
| **CWE** | CWE-200 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Rendah / Informasional |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/09-Fingerprint_Web_Application  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Wappalyzer`, `WhatWeb`, `BuiltWith`, `CMSmap`, `nmap`, `Nikto`, `Burp Suite`

### Apakah header respons HTTP mengungkapkan technology stack atau nomor versi?
- [ ] Tidak — header sensitif telah dihapus atau berisi nilai generik  
- [ ] Ya — header default seperti `X-Powered-By`, `Server`, atau `X-AspNet-Version` ditemukan  
- [ ] Ya — header mengungkapkan versi spesifik dan usang (outdated) dari web server atau framework aplikasi  

### Apakah framework aplikasi atau CMS dapat diidentifikasi melalui jalur file (file path) unik atau konvensi penamaan?
- [ ] Tidak — jalur file (file path) disamarkan (obfuscated) dan tidak mengungkapkan teknologi yang mendasarinya  
- [ ] Ya — struktur direktori umum (misalnya, `/wp-content/`, `/_next/`, `/node_modules/`) terlihat  
- [ ] Ya — file unik seperti `robots.txt`, `humans.txt`, atau `sitemap.xml` berisi metadata spesifik teknologi  

### Apakah nomor versi spesifik terpapar dalam kode sumber HTML atau aset statis?
- [ ] Tidak — tidak ada informasi versi yang ditemukan dalam komentar atau parameter aset  
- [ ] Ya — komentar HTML atau tag meta (misalnya, `<meta name="generator" content="...">`) ditemukan  
- [ ] Ya — string versi ditambahkan ke nama file CSS/JS (misalnya, `jquery.js?v=1.12.4`) dan tidak dapat dinonaktifkan  

### Apakah halaman kesalahan default atau antarmuka manajemen mengungkapkan lingkungan perangkat lunak?
- [ ] Tidak — aplikasi menggunakan halaman kesalahan khusus yang generik untuk semua kode status  
- [ ] Ya — halaman kesalahan default untuk web server atau framework diaktifkan dan membocorkan data versi  
- [ ] Ya — portal login administratif (misalnya, `/wp-admin`, `/phpmyadmin`) dapat diakses dan diidentifikasi  

### Apakah cookie mengungkapkan framework manajemen sesi (session management)?
- [ ] Tidak — cookie sesi menggunakan nama generik atau kustom  
- [ ] Ya — nama cookie spesifik teknologi (misalnya, `PHPSESSID`, `JSESSIONID`, `ASPSESSIONID`) digunakan

---

## WSTG-INFO-10 — Pemetaan Arsitektur Aplikasi

Pemetaan arsitektur aplikasi melibatkan identifikasi teknologi yang mendasari, komponen infrastruktur, dan jalur aliran data yang mendukung aplikasi web. Dengan menganalisis header respons HTTP, pesan kesalahan, format cookie, dan ekstensi berkas, penguji dapat menyusun kembali skema server web, server aplikasi, database, dan integrasi pihak ketiga yang digunakan. Pengetahuan ini sangat mendasar untuk menyesuaikan serangan berikutnya, karena memungkinkan pemilihan platform-specific exploits dan identifikasi potensi bottleneck atau perangkat perantara yang salah konfigurasi seperti Load Balancer dan WAF. Seorang penyerang memanfaatkan peta struktural ini untuk berpindah dari pemindaian generik ke eksploitasi terarah pada software stack tertentu dan komponen-komponennya yang saling terhubung.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INFO-10 |
| **CWE** | CWE-200 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Informasional / Rendah* |

> *Keparahan menjadi Menengah jika topologi jaringan internal atau alamat IP privat terungkap.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/10-Map_Application_Architecture  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Wappalyzer`, `BuiltWith`, `nmap`, `WhatWeb`, `Burp Suite`, `Netcat`, `curl`

### Dapatkah teknologi server web dan server aplikasi diidentifikasi secara akurat?
- [ ] Tidak — identifikasi tidak dimungkinkan karena hardening atau obfuskasi yang efektif  
- [ ] Ya — jenis teknologi diketahui tetapi versi spesifik tidak dapat ditentukan  
- [ ] Ya — teknologi dan versi spesifik diungkapkan sepenuhnya melalui header atau tanda tangan berkas *(Informasional)*  

### Apakah perangkat perantara (WAF, Load Balancer, Proxy) terdeteksi dalam jalur komunikasi?
- [ ] Tidak — tidak ada perangkat perantara yang terdeteksi atau mereka sepenuhnya transparan  
- [ ] Ya — keberadaan dicurigai melalui waktu atau perilaku, tetapi identitas tidak dikonfirmasi  
- [ ] Ya — produk spesifik (misalnya, Cloudflare, Nginx, F5) diidentifikasi melalui header unik atau perilaku  

### Apakah aplikasi membocorkan informasi tentang struktur direktori internal atau topologi jaringan?
- [ ] Tidak — jalur internal dan IP tidak diungkapkan  
- [ ] Ya — jalur internal bocor dalam pesan kesalahan atau stack trace tetapi IP internal tidak  
- [ ] Ya — baik struktur direktori internal maupun alamat IP privat diungkapkan  

### Dapatkah jenis dan versi database backend disimpulkan dari perilaku aplikasi?
- [ ] Tidak — rincian database tidak dapat ditentukan  
- [ ] Ya — jenis database (misalnya, PostgreSQL, MSSQL) disimpulkan melalui pesan kesalahan atau perilaku  
- [ ] Ya — jenis dan versi database diungkapkan secara eksplisit dalam bodi respons atau header  

### Apakah aplikasi di-host pada penyedia layanan cloud yang dapat diidentifikasi atau CDN pihak ketiga tertentu?
- [ ] Tidak — infrastruktur hosting tidak dapat diidentifikasi  
- [ ] Ya — penyedia cloud (AWS, Azure, GCP) diidentifikasi tetapi layanan spesifik tidak  
- [ ] Ya — layanan cloud spesifik (misalnya, S3 bucket, Lambda, Azure Functions) telah dipetakan dan dapat dijangkau

---

## WSTG-CONF-01 — Test Network Infrastructure Configuration

Pengujian konfigurasi infrastruktur jaringan melibatkan identifikasi kelemahan pada komponen server dan jaringan dasar yang mendukung aplikasi web. Penyerang menargetkan layanan yang salah dikonfigurasi, protokol yang usang, dan port terbuka yang tidak diperlukan untuk mendapatkan pijakan (foothold), melakukan pergerakan lateral (lateral movement), atau melakukan pengintaian (reconnaissance). Temuan umum mencakup versi TLS yang tidak aman, banner layanan yang bersifat verbose, dan antarmuka administratif yang terekspos yang seharusnya dibatasi pada jaringan internal. Dengan mengeksploitasi cacat di tingkat infrastruktur ini, penyerang dapat mengompromikan kerahasiaan data saat transit atau mendapatkan akses tidak sah ke sistem operasi host.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CONF-01 |
| **CWE** | CWE-16 |
| **Status Pengujian** | Not Performed |
| **Keparahan** | Low / Medium |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/01-Test_Network_Infrastructure_Configuration  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/pentesting-methodology.html  

**Alat:** `nmap`, `sslyze`, `testssl.sh`, `Nikto`, `OpenVAS`, `Nessus`

### Apakah terdapat port terbuka atau layanan yang tidak diperlukan yang terekspos pada host aplikasi?
- [ ] Tidak — hanya port yang diperlukan (misalnya, 443) yang **dapat diakses**  
- [ ] Ya — layanan tambahan terekspos tetapi mengikuti konfigurasi yang **aman**  
- [ ] Ya — layanan yang tidak esensial (misalnya, FTP, Telnet, SMB) **terekspos** secara publik  

### Apakah banner atau header layanan membocorkan informasi versi yang sensitif?
- [ ] Tidak — banner dihapus atau bersifat umum (generic)  
- [ ] Ya — banner mengungkapkan perangkat lunak server (misalnya, Apache/nginx) tetapi **bukan** nomor versi  
- [ ] Ya — banner yang verbose mengungkapkan versi perangkat lunak tertentu, yang membantu **eksploitasi (exploitation)**  

### Apakah konfigurasi TLS mengikuti praktik terbaik keamanan modern?
- [ ] Ya — hanya TLS 1.2/1.3 yang **diaktifkan** dengan cipher suite yang kuat  
- [ ] Ya — protokol lama (TLS 1.0/1.1) **diaktifkan**  
- [ ] Tidak — cipher yang lemah atau protokol yang tidak aman (SSLv2/v3) **diaktifkan**  

### Apakah antarmuka administratif atau manajemen dibatasi hanya untuk jaringan yang berwenang?
- [ ] Ya — antarmuka (misalnya, SSH, RDP, panel kontrol) **tidak dapat diakses** dari internet publik  
- [ ] No — antarmuka **dapat diakses** secara publik tetapi memerlukan autentikasi multifaktor (multi-factor authentication)  
- [ ] No — antarmuka **dapat diakses** secara publik dengan autentikasi faktor tunggal atau kredensial default

---

## WSTG-CONF-02 — Pengujian Konfigurasi Platform Aplikasi

Pengujian konfigurasi platform aplikasi melibatkan audit terhadap pengaturan web server, application server, dan framework yang mendasarinya untuk memastikan semuanya telah diperkuat (hardened) terhadap eksploitasi umum. Penyerang secara aktif mencari kredensial default, kerentanan platform yang belum ditambal (unpatched), dan antarmuka administratif yang terekspos untuk mendapatkan akses tidak sah atau mengeksekusi kode. Kesalahan konfigurasi (misconfiguration) sering kali mengakibatkan pengungkapan variabel lingkungan yang sensitif, jalur sistem internal, atau keberadaan aplikasi contoh yang tidak perlu yang memperluas permukaan serangan (attack surface). Dari sudut pandang profesional, platform yang dikonfigurasi dengan buruk berfungsi sebagai titik masuk dengan probabilitas tinggi untuk mendapatkan akses awal ke lingkungan target.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CONF-02 |
| **CWE** | CWE-16 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Sedang / Tinggi* |

> *Tingkat keparahan menjadi Tinggi jika antarmuka administratif dapat diakses dengan kredensial default atau jika skrip contoh memungkinkan Remote Code Execution (RCE).

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/02-Test_Application_Platform_Configuration  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Nmap`, `Nikto`, `WhatWeb`, `Wappalyzer`, `Burp Suite`, `Curl`

### Apakah kredensial atau akun default aktif pada platform?
- [ ] Tidak — semua akun default **dinonaktifkan** atau kata sandi telah diubah  
- [ ] Ya — akun default ada tetapi **tidak dapat diakses** dari jaringan eksternal  
- [ ] Ya — akun default **aktif** dan dapat diakses melalui internet *(Kritis)*  

### Apakah platform mengungkapkan informasi versi yang sensitif melalui header atau halaman kesalahan?
- [ ] Tidak — string versi **disembunyikan** atau bersifat umum (generic)  
- [ ] Ya — informasi versi yang terperinci **diungkapkan** dalam header HTTP (misalnya, `Server`, `X-Powered-By`)  
- [ ] Ya — full stack traces dan jalur sistem internal **terekspos** dalam pesan kesalahan yang verbose  

### Apakah antarmuka administratif atau manajemen dibatasi dengan benar?
- [ ] Tidak — antarmuka administratif **tidak terekspos**  
- [ ] Ya — antarmuka tersedia tetapi **dibatasi** oleh allowlisting IP atau Multi-Factor Authentication (MFA)  
- [ ] Ya — antarmuka (misalnya, `/admin`, `/manager`, `/console`) **dapat diakses secara publik** dengan autentikasi faktor tunggal  

### Apakah file contoh, skrip, atau dokumentasi ada di server produksi?
- [ ] Tidak — semua file yang tidak penting telah **dihapus**  
- [ ] Ya — file contoh atau dokumentasi **ada** tetapi tidak membocorkan informasi sensitif  
- [ ] Ya — skrip contoh (misalnya, `info.php`, `examples/`) **ada** dan menyediakan vektor serangan langsung  

### Apakah server dikonfigurasi untuk mendukung metode HTTP yang tidak aman?
- [ ] Tidak — hanya `GET` dan `POST` yang **diaktifkan**  
- [ ] Ya — metode yang berpotensi berisiko seperti `PUT`, `DELETE`, atau `TRACE` **diaktifkan** tetapi dibatasi  
- [ ] Ya — metode berisiko **diaktifkan** dan memungkinkan modifikasi file yang tidak sah atau pencurian kredensial

---

## WSTG-CONF-03 — Menguji Penanganan Ekstensi Berkas untuk Informasi Sensitif

Pengujian penanganan ekstensi berkas melibatkan identifikasi apakah server web atau server aplikasi mengungkapkan informasi sensitif dengan menyajikan berkas yang seharusnya dibatasi atau dieksekusi. Penyerang sering kali mencari berkas cadangan (backup), berkas konfigurasi, dan fragmen kode sumber (misalnya, `.bak`, `.old`, `.env`, `.inc`) yang mungkin tertinggal di *web root* dan disajikan sebagai teks biasa karena kurangnya pemetaan *handler* yang spesifik. Kerentanan ini penting karena dapat menyebabkan terbukanya kredensial basis data, API keys, dan logika bisnis internal, yang memfasilitasi eksploitasi infrastruktur lebih lanjut. Paparan ini biasanya terjadi di direktori publik, folder unggahan, atau melalui pengaturan MIME-type yang salah dikonfigurasi pada server.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CONF-03 |
| **CWE** | CWE-552 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Sedang / Tinggi* |

> *Tingkat keparahan menjadi Tinggi jika berkas konfigurasi yang berisi kredensial atau kode sumber lengkap dapat diakses.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/03-Test_File_Extensions_Handling_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `ffuf`, `gobuster`, `dirsearch`, `Burp Suite (Intruder)`, `Wfuzz`

### Apakah ekstensi berkas cadangan atau sementara yang sensitif (misalnya, `.bak`, `.old`, `.swp`, `~`) dapat diakses?
- [ ] Tidak — server mengembalikan 403 Forbidden atau 404 Not Found untuk ekstensi cadangan yang umum  
- [ ] Ya — server mengizinkan pengunduhan berkas cadangan tetapi **tidak** mengandung data sensitif  
- [ ] Ya — server mengizinkan pengunduhan berkas cadangan yang berisi data sensitif atau kode sumber *(Tinggi)*  

### Apakah server mengekspos berkas lingkungan atau konfigurasi sistem (misalnya, `.env`, `.config`, `.yml`, `.ini`)?
- [ ] Tidak — berkas konfigurasi yang dibatasi **tidak dapat** diakses dan mengembalikan 403/404  
- [ ] Ya — berkas konfigurasi sensitif **dapat** diakses dan mengungkapkan rahasia internal atau kredensial  

### Apakah kode sumber dapat diambil dengan menambahkan ekstensi alternatif (misalnya, `.php.txt`, `.jsp.old`, `.aspx.bak`)?
- [ ] Tidak — kode aplikasi **tidak** ditampilkan sebagai teks biasa terlepas dari manipulasi ekstensi  
- [ ] Ya — kode sumber terungkap karena server **tidak** memiliki *handler* untuk ekstensi yang ditambahkan  

### Apakah direktori administratif atau metadata (misalnya, `.git/`, `.svn/`, `.DS_Store`) diblokir dari akses publik?
- [ ] Tidak — direktori/berkas ini **tidak** ada di *web root*  
- [ ] Ya — akses **tidak dimungkinkan** karena aturan *rewrite* di sisi server atau izin akses  
- [ ] Ya — direktori metadata **dapat** diakses dan memungkinkan rekonstruksi kode sumber secara lengkap  

### Bagaimana server menangani permintaan untuk berkas dengan banyak ekstensi (misalnya, `file.php.jpg`)?
- [ ] Tidak — server memprioritaskan keamanan ekstensi internal dengan benar atau memblokir permintaan tersebut  
- [ ] Ya — server memproses berkas sebagai ekstensi pertama, tetapi *bypass* **tidak dimungkinkan** untuk eksekusi  
- [ ] Ya — server mengabaikan ekstensi terakhir, sehingga eksekusi kode atau pengungkapan sumber **dimungkinkan**

---

## WSTG-CONF-04 — Meninjau Cadangan Lama dan Berkas Tidak Terreferensi untuk Informasi Sensitif

Meninjau berkas cadangan (backup) lama dan berkas yang tidak terreferensi melibatkan identifikasi berkas yang terlupakan, bersifat sementara, atau tersembunyi pada server web yang tidak ditujukan untuk akses publik. Berkas-berkas ini sering kali mencakup cadangan kode sumber (`.zip`, `.bak`), berkas swap editor teks (`.swp`, `~`), atau metadata kontrol sumber (`.git`, `.svn`) yang dapat membocorkan informasi sensitif. Penyerang menggunakan daftar kata (wordlist) otomatis dan alat penemuan untuk melakukan Brute Force pada konvensi penamaan umum, dengan tujuan mengeksfiltrasi kredensial basis data, API keys yang tertanam (hardcoded), atau logika yang memfasilitasi eksploitasi lebih lanjut. Kerentanan ini biasanya berasal dari pemeliharaan server secara manual atau alur kerja CI/CD yang cacat sehingga gagal melakukan sanitasi pada direktori utama (web root) produksi.

| Kolom | Nilai |
|---|---|
| **WSTG ID** | WSTG-CONF-04 |
| **CWE** | CWE-530 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Sedang / Tinggi* |

> *Keparahan menjadi Tinggi jika berkas konfigurasi, arsip kode sumber, atau kredensial ditemukan.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/04-Review_Old_Backup_and_Unreferenced_Files_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `ffuf`, `dirsearch`, `gobuster`, `GitTools`, `Burp Suite (Engagement Tools)`, `Wfuzz`

### Apakah daftar direktori (directory listing) diaktifkan pada server web?
- [ ] Tidak — daftar direktori **dinonaktifkan** dan mengembalikan kesalahan 403 Forbidden atau kesalahan khusus  
- [ ] Ya — daftar direktori **diaktifkan** pada direktori non-sensitif  
- [ ] Ya — daftar direktori **diaktifkan** pada direktori yang berisi kode sumber atau berkas sensitif *(Kritis)*  

### Apakah berkas cadangan dengan ekstensi umum (misalnya, .bak, .old, .save) dapat ditemukan?
- [ ] Tidak — ekstensi cadangan umum **tidak ditemukan** atau diblokir oleh konfigurasi server  
- [ ] Ya — berkas cadangan ada tetapi **tidak** berisi informasi sensitif  
- [ ] Ya — berkas cadangan untuk konfigurasi atau kode sumber **dapat** diakses *(Tinggi)*  

### Apakah direktori kontrol sumber (misalnya, .git, .svn) atau metadata terpapar?
- [ ] Tidak — metadata kontrol sumber **tidak ada** atau diblokir dengan benar  
- [ ] Ya — metadata ada tetapi repositori lengkap **tidak dapat** dikonstruksi ulang  
- [ ] Ya — seluruh repositori kode sumber **dapat** dieksfiltrasi melalui metadata yang terpapar  

### Apakah arsip terkompresi (misalnya, .zip, .tar.gz, .rar) dari aplikasi tersedia di web root?
- [ ] Tidak — tidak ada berkas arsip sensitif yang ditemukan melalui brute-force atau enumerasi  
- [ ] Ya — arsip ditemukan tetapi dilindungi kata sandi atau berisi aset publik  
- [ ] Ya — arsip yang tidak terenkripsi yang berisi kode sumber atau data aplikasi **dapat** diakses secara publik  

### Apakah aplikasi membocorkan berkas sementara yang dibuat oleh editor teks atau IDE?
- [ ] Tidak — berkas sementara seperti `.swp`, `~`, atau `.DS_Store` **tidak ada**  
- [ ] Ya — berkas sementara yang tidak sensitif **ditemukan**  
- [ ] Ya — berkas sementara mengungkapkan potongan kode sumber atau jalur (path) internal

---

## WSTG-CONF-05 — Melakukan Enumerasi Antarmuka Admin Infrastruktur dan Aplikasi

Antarmuka administratif mewakili kontrol manajemen (control plane) dari sebuah aplikasi dan infrastruktur yang mendasarinya, yang sering kali memberikan akses istimewa ke konfigurasi sistem, data pengguna, dan operasi sisi server (server-side operations). Penyerang memprioritaskan enumerasi antarmuka ini melalui directory brute-forcing, analisis `robots.txt`, atau mengamati pola URL yang dapat diprediksi untuk menemukan titik masuk (entry points) yang mungkin kurang memiliki autentikasi yang kuat atau mengandalkan kredensial default. Penemuan panel admin yang terekspos secara signifikan meningkatkan risiko kompromi sistem secara menyeluruh, modifikasi data yang tidak sah, atau gangguan layanan. Antarmuka ini sering ditemukan pada port non-standar atau subdomain, menjadikannya target utama bagi pemindai otomatis dan pengintaian manual (manual reconnaissance).


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CONF-05 |
| **CWE** | CWE-425 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Sedang / Tinggi* |

> *Tingkat keparahan menjadi Tinggi jika antarmuka memungkinkan perubahan konfigurasi di seluruh sistem, manajemen pengguna, atau eksfiltrasi data tanpa autentikasi multifaktor (MFA).

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/05-Enumerate_Infrastructure_and_Application_Admin_Interfaces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `ffuf`, `dirsearch`, `Gobuster`, `Nmap`, `Wappalyzer`, `Burp Suite (Intruder)`

### Apakah antarmuka administratif dapat ditemukan melalui directory fuzzing atau pengintaian (reconnaissance)?
- [ ] Tidak — tidak ada antarmuka administratif yang terekspos atau dapat ditemukan  
- [ ] Ya — antarmuka dapat ditemukan tetapi akses **dibatasi** pada rentang IP internal  
- [ ] Ya — antarmuka **dapat** ditemukan dan dapat diakses secara publik  

### Apakah autentikasi diterapkan pada portal administratif yang ditemukan?
- [ ] Ya — autentikasi multifaktor (MFA) **diterapkan**  
- [ ] Ya — autentikasi faktor tunggal (single-factor authentication) **diterapkan**  
- [ ] Tidak — tidak diperlukan autentikasi untuk mengakses antarmuka *(Kritis)*  

### Apakah jalur administratif disembunyikan atau menggunakan standar non-umum untuk mencegah penemuan?
- [ ] Ya — jalur menggunakan penamaan non-standar, acak, atau tidak dapat diprediksi  
- [ ] Tidak — jalur menggunakan konvensi penamaan umum (misalnya, `/admin`, `/manager`, `/console`) dan **dapat** ditebak dengan mudah  

### Apakah antarmuka menyediakan akses ke fungsionalitas sistem atau aplikasi yang sensitif?
- [ ] Tidak — antarmuka adalah dasbor status "read-only" dengan dampak minimal  
- [ ] Ya — antarmuka **dapat** memodifikasi konfigurasi aplikasi atau data pengguna  
- [ ] Ya — antarmuka **dapat** mengeksekusi perintah tingkat sistem atau melakukan manajemen basis data

---

## WSTG-CONF-06 — Test HTTP Methods

Pengujian metode HTTP melibatkan identifikasi kata kerja (verbs) mana yang didukung oleh server web dan aplikasi untuk memastikan hanya fungsionalitas yang diperlukan yang diekspos. Selain `GET` dan `POST` standar, server mungkin secara tidak sengaja mengaktifkan metode berbahaya seperti `PUT`, `DELETE`, atau `TRACE`, yang dapat memungkinkan penyerang untuk mengunggah file berbahaya, menghapus konten yang ada, atau mengeksfiltrasi cookie sesi melalui Cross-Site Tracing (XST). Pentester memeriksa header respons seperti `Allow` atau `Public` dan mencoba melewati batasan menggunakan teknik method overriding atau verb tampering untuk mengakses fungsionalitas yang tidak sah. Pengujian ini sangat penting untuk memperkuat (hardening) permukaan serangan dan mencegah kesalahan konfigurasi (misconfiguration) yang dapat menyebabkan kompromi server atau manipulasi data yang tidak sah.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CONF-06 |
| **CWE** | CWE-650 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Rendah / Sedang* |

> *Tingkat keparahan menjadi Tinggi jika `PUT` atau `DELETE` memungkinkan manipulasi file tanpa izin atau jika `TRACE` memfasilitasi eksfiltrasi token sesi.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/06-Test_HTTP_Methods  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `curl`, `nmap`, `Burp Suite (Repeater)`, `ZAP`, `Metasploit`

### Apakah metode HTTP berbahaya seperti `PUT` atau `DELETE` dinonaktifkan di seluruh aplikasi?
- [ ] Ya — hanya metode aman (GET, POST, HEAD) yang **diaktifkan**  
- [ ] Tidak — `PUT` atau `DELETE` **diaktifkan** tetapi memerlukan autentikasi yang valid  
- [ ] Tidak — `PUT` atau `DELETE` **diaktifkan** dan dapat diakses tanpa autentikasi *(Kritis)*  

### Apakah metode `TRACE` dinonaktifkan untuk mencegah Cross-Site Tracing (XST)?
- [ ] Ya — metode `TRACE` **dinonaktifkan** atau mengembalikan 405 Method Not Allowed  
- [ ] Tidak — metode `TRACE` **diaktifkan** tetapi tidak merefleksikan header sensitif  
- [ ] Tidak — metode `TRACE` **diaktifkan** dan merefleksikan header `Cookie` atau `Authorization` *(Sedang)*  

### Apakah HTTP Method Overriding (misalnya, `X-HTTP-Method-Override`) didukung oleh server?
- [ ] Tidak — server **tidak** memproses header method override  
- [ ] Ya — server memproses header override tetapi kontrol akses (access control) **tetap diterapkan**  
- [ ] Ya — server memproses header override dan bypass kontrol akses **mungkin terjadi**  

### Apakah server merespons dengan aman terhadap metode HTTP yang arbitrer atau cacat (malformed)?
- [ ] Ya — server mengembalikan 405 Method Not Allowed atau 501 Not Implemented  
- [ ] Tidak — server mengembalikan 200 OK atau 500 Error untuk metode arbitrer seperti `JEFF` atau `TEST`  
- [ ] Tidak — menggunakan metode arbitrer memungkinkan bypass filter autentikasi atau otorisasi  

### Apakah metode `OPTIONS` dibatasi atau dikonfigurasi untuk meminimalkan pengungkapan informasi (information disclosure)?
- [ ] Ya — `OPTIONS` dinonaktifkan atau mengembalikan header `Allow` yang minimal  
- [ ] Tidak — `OPTIONS` mengungkapkan berbagai macam metode yang diaktifkan, termasuk kata kerja DAV atau debugging

---

## WSTG-CONF-07 — Menguji HTTP Strict Transport Security

HTTP Strict Transport Security (HSTS) adalah mekanisme keamanan yang memungkinkan server web untuk memberitahu peramban (browser) bahwa server tersebut hanya boleh diakses melalui HTTPS, mencegah serangan penurunan protokol (protocol downgrade attacks) dan pembajakan cookie (cookie hijacking). Dengan mewajibkan koneksi terenkripsi, HSTS memitigasi serangan Man-in-the-Middle (MitM) seperti SSLStrip, di mana penyerang mencegat permintaan HTTP awal yang tidak aman untuk mencegah transisi ke TLS. Dari perspektif penyerang, tidak adanya header ini atau durasi `max-age` yang singkat memberikan celah kesempatan untuk mencegat token sesi sensitif atau kredensial selama proses jabat tangan (handshake) teks polos awal. Pengujian ini berfokus pada verifikasi keberadaan, durasi, dan cakupan header `Strict-Transport-Security` di semua titik akhir (endpoints) aplikasi yang sensitif.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CONF-07 |
| **CWE** | CWE-319 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan (Severity)** | Rendah / Menengah* |

> *Keparahan menjadi Menengah jika aplikasi menangani PII, token sesi, atau kredensial, karena kurangnya HSTS meningkatkan tingkat keberhasilan serangan MitM.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/07-Test_HTTP_Strict_Transport_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `curl`, `Burp Suite`, `nmap`, `SSLLabs`, `hsts-preload-checker`

### Apakah header `Strict-Transport-Security` ada dalam respons HTTPS?
- [ ] Ya — header **tersedia** di semua titik akhir sensitif  
- [ ] Ya — header **tersedia** tetapi hanya pada halaman landas (landing page)  
- [ ] Tidak — header **tidak ditemukan** di aplikasi sepenuhnya  

### Apakah direktif `max-age` diatur ke durasi yang memadai?
- [ ] Ya — `max-age` diatur ke satu tahun atau lebih (misalnya, `31536000`)  
- [ ] Ya — `max-age` diatur tetapi **terlalu singkat** (misalnya, kurang dari 6 bulan)  
- [ ] Tidak — direktif `max-age` **tidak ditemukan** atau diatur ke `0` *(Kritis)*  

### Apakah kebijakan HSTS mencakup semua subdomain?
- [ ] Ya — direktif `includeSubDomains` **diterapkan**  
- [ ] Tidak — direktif `includeSubDomains` **tidak ditemukan**, membuat subdomain rentan terhadap penurunan protokol (downgrade)  
- [ ] Tidak — fitur tidak berlaku karena tidak ada subdomain  

### Apakah aplikasi terdaftar untuk HSTS Preloading?
- [ ] Ya — direktif `preload` **tersedia** dan domain ada dalam daftar preload HSTS  
- [ ] Ya — direktif `preload` **tersedia** tetapi domain **belum** masuk dalam daftar preload  
- [ ] Tidak — direktif `preload` **tidak ditemukan**, memberikan celah untuk MitM pada kunjungan pertama  

### Apakah header HSTS salah dikirim melalui HTTP teks polos (plaintext)?
- [ ] Tidak — header **hanya** dikirim melalui koneksi HTTPS yang aman  
- [ ] Ya — header **dikirim** melalui HTTP, yang diabaikan oleh peramban dan dapat membocorkan detail konfigurasi

---

## WSTG-CONF-08 — Test RIA Cross Domain Policy

Kebijakan lintas-domain (cross-domain) Rich Internet Application (RIA) melibatkan berkas seperti `crossdomain.xml` dan `clientaccesspolicy.xml` yang menentukan bagaimana klien web—khususnya Adobe Flash dan Microsoft Silverlight—menangani permintaan lintas-asal (cross-origin). Berkas-berkas ini sangat penting bagi keamanan karena secara eksplisit mengesampingkan Same-Origin Policy (SOP), yang menentukan domain eksternal mana yang diizinkan untuk berinteraksi dengan aplikasi dan membaca responsnya. Konfigurasi yang terlalu permisif, seperti penggunaan wildcard pada atribut `domain`, memungkinkan penyerang untuk menghosting objek RIA berbahaya di situs pihak ketiga yang dapat mengeksfiltrasi data sensitif atau melakukan tindakan atas nama pengguna yang terautentikasi. Pentester biasanya mencari berkas-berkas ini di root web untuk mengevaluasi tingkat kepercayaan yang diberikan kepada asal (origin) pihak ketiga dan mengidentifikasi potensi kebocoran data lintas-asal.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CONF-08 |
| **CWE** | CWE-942 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan (Severity)** | Sedang / Tinggi* |

> *Keparahan menjadi Tinggi jika aplikasi menangani data pengguna yang sensitif atau pengidentifikasi sesi yang dapat dieksfiltrasi melalui kebijakan yang permisif.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/08-Test_RIA_Cross_Domain_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `curl`, `Burp Suite`, `FFUF`, `dirsearch`, `Wget`

### Apakah berkas kebijakan lintas-domain (`crossdomain.xml` atau `clientaccesspolicy.xml`) tersedia di root web?
- [ ] Tidak — berkas **tidak dapat** ditemukan di direktori root  
- [ ] Ya — `crossdomain.xml` (Flash) **tersedia**  
- [ ] Ya — `clientaccesspolicy.xml` (Silverlight) **tersedia**  
- [ ] Ya — kedua berkas kebijakan RIA **tersedia**  

### Apakah atribut domain `allow-access-from` dibatasi pada asal (origin) yang terpercaya?
- [ ] Tidak — berkas ada tetapi tidak berisi entri `allow-access-from`  
- [ ] Ya — domain tertentu yang terpercaya secara eksplisit dimasukkan ke dalam daftar putih (whitelisted) dan bypass **tidak dimungkinkan**  
- [ ] Ya — domain dimasukkan ke dalam daftar putih tetapi mencakup pihak ketiga yang tidak terpercaya atau sub-domain  
- [ ] Ya — wildcard `*` **digunakan**, mengizinkan akses dari asal **manapun** *(Kritis)*  

### Apakah kebijakan mengizinkan komunikasi tidak aman melalui HTTP untuk sumber daya HTTPS?
- [ ] Tidak — `secure="true"` telah diatur (atau menjadi nilai default), mencegah asal HTTP mengakses data HTTPS  
- [ ] Ya — `secure="false"` diatur secara eksplisit, memungkinkan penyerang MITM mengeksfiltrasi data aman  

### Apakah header HTTP terlalu permisif dalam konfigurasi lintas-domain?
- [ ] Tidak — `allow-http-request-headers-from` dibatasi atau **tidak diaktifkan**  
- [ ] Ya — header tertentu diizinkan untuk domain terpercaya  
- [ ] Ya — wildcard `*` digunakan untuk header, memungkinkan penyerang mengirim header khusus dari domain manapun  

### Apakah konfigurasi `site-control` (master-policy) bersifat restriktif?
- [ ] Tidak — `permitted-cross-domain-policies` diatur ke `none` atau `master-only`  
- [ ] Ya — kebijakan mengizinkan berkas lain di server untuk menentukan aturan mereka sendiri (`all`)

---

## WSTG-CONF-09 — Test File Permission

Pengujian izin berkas (file permission) melibatkan audit terhadap tingkat kontrol akses yang diberikan pada berkas dan direktori di server web untuk memastikan bahwa sumber daya sensitif tidak terekspos secara berlebihan kepada pengguna atau proses yang tidak sah. Izin yang tidak dikonfigurasi dengan benar dapat memungkinkan penyerang untuk membaca berkas konfigurasi sensitif yang berisi kredensial basis data, melihat kode sumber (source code) eksklusif, atau memodifikasi skrip sisi server untuk mencapai Remote Code Execution (RCE). Kerentanan ini biasanya terjadi selama fase deployment ketika flag "world-readable" atau "world-writable" default dibiarkan pada direktori aplikasi yang kritis, seperti jalur konfigurasi, folder log, atau repositori unggahan. Dari sudut pandang penyerang, menemukan berkas yang tidak diamankan dengan benar sering kali menjadi katalis utama untuk eskalasi hak istimewa (privilege escalation) atau kompromi sistem secara penuh.


| Kolom | Nilai |
|---|---|
| **WSTG ID** | WSTG-CONF-09 |
| **CWE** | CWE-732 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Sedang / Tinggi* |

> *Tingkat keparahan menjadi Tinggi jika berkas konfigurasi sensitif (misalnya, `.env`, `web.config`) dapat dibaca atau jika akses tulis ke web root dimungkinkan.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/09-Test_File_Permission  
* https://hacktricks.wiki/en/linux-hardening/privilege-escalation/index.html  

**Tools:** `ls`, `find`, `LinPeas`, `WinPeas`, `ffuf`, `dirsearch`, `Nmap`

### Apakah berkas konfigurasi aplikasi yang sensitif terlindungi dari akses yang tidak sah?
- [ ] Ya — berkas konfigurasi (misalnya, `.env`, `config.php`) **tidak dapat diakses** melalui permintaan web  
- [ ] Ya — berkas tersedia tetapi akses **dibatasi** hanya untuk pengguna lokal yang berwenang  
- [ ] Tidak — berkas konfigurasi sensitif **dapat diakses** dan mengungkapkan kredensial atau rahasia  

### Dapatkah penyerang memodifikasi berkas atau direktori di dalam web root?
- [ ] Tidak — semua direktori web root bersifat **read-only** bagi pengguna web server  
- [ ] Ya — direktori yang dapat ditulis (writable) tersedia tetapi eksekusi skrip **dinonaktifkan**  
- [ ] Ya — direktori world-writable tersedia dan **memungkinkan** pengunggahan serta eksekusi skrip *(Kritis)*  

### Apakah metadata kontrol versi atau berkas cadangan terekspos karena izin yang longgar?
- [ ] Tidak — `.git`, `.svn`, dan berkas cadangan (misalnya, `.bak`, `~`) **tidak ada** atau terlindungi  
- [ ] Ya — direktori metadata tersedia tetapi directory listing **dinonaktifkan**  
- [ ] Ya — metadata atau cadangan sensitif **dapat diakses sepenuhnya**, memungkinkan pemulihan kode sumber  

### Apakah pengguna web server beroperasi dengan prinsip hak istimewa terendah (principle of least privilege)?
- [ ] Ya — proses web server berjalan sebagai pengguna **berhak istimewa rendah yang khusus (dedicated low-privilege)**  
- [ ] Tidak — proses web server berjalan dengan **hak istimewa yang tidak perlu** (misalnya, `root` atau `Administrator`)  

### Apakah berkas log diamankan dari pembacaan atau modifikasi yang tidak sah?
- [ ] Ya — berkas log **dibatasi** hanya untuk pengguna administratif  
- [ ] Ya — berkas log dapat dibaca tetapi **tidak** mengandung token sesi atau PII (Personally Identifiable Information)  
- [ ] Tidak — berkas log **dapat dibaca secara publik** dan mengungkapkan data transaksi sensitif atau informasi pengguna

---

## WSTG-CONF-10 — Subdomain Takeover

Pengambilalihan subdomain (Subdomain Takeover) terjadi ketika entri DNS (biasanya CNAME, tetapi terkadang record A atau MX) mengarah ke sumber daya yang sudah tidak digunakan atau tidak ada pada penyedia cloud pihak ketiga atau layanan hosting. Penyerang mengidentifikasi record DNS yang "menggantung" (dangling) ini dengan mencari pesan kesalahan spesifik dari penyedia seperti AWS, GitHub, atau Azure yang menunjukkan bahwa suatu sumber daya tidak lagi diklaim. Dengan mendaftarkan nama sumber daya yang ditinggalkan tersebut di platform penyedia, penyerang mendapatkan kendali penuh atas subdomain tersebut, memungkinkan mereka untuk melakukan hosting konten berbahaya, mengeksfiltrasi cookie sesi yang dicakupkan ke domain utama, serta mem-bypass perlindungan Content Security Policy (CSP) atau CORS. Kerentanan ini sangat berbahaya karena memanfaatkan kepercayaan yang terkait dengan struktur domain resmi organisasi untuk memfasilitasi phishing berdampak tinggi dan pencurian data.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CONF-10 |
| **CWE** | CWE-1329 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi / Kritis* |

> *Keparahan menjadi Kritis jika subdomain dapat digunakan untuk membajak cookie sesi dari domain utama atau mem-bypass redirect SSO/OAuth.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/10-Test_for_Subdomain_Takeover  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://github.com/EdOverflow/can-i-take-over-xyz  

**Peralatan:** `Subjack`, `SubOver`, `dnscan`, `Amass`, `Nuclei`, `dig`, `nslookup`

### Apakah aplikasi menggunakan hosting pihak ketiga atau layanan cloud melalui subdomain?
- [ ] Tidak — semua subdomain mengarah ke ruang IP yang dikontrol organisasi  
- [ ] Ya — subdomain mengarah ke penyedia pihak ketiga (S3, GitHub Pages, Heroku, dll.)  

### Apakah terdapat record DNS yang "menggantung" (dangling) yang mengarah ke sumber daya yang tidak ada?
- [ ] Tidak — semua record DNS yang teridentifikasi mengarah ke sumber daya aktif yang terkontrol  
- [ ] Ya — record CNAME atau A ada untuk sumber daya yang **sudah tidak ada lagi** pada penyedia  
- [ ] Ya — record DNS mengarah ke NXDOMAIN atau halaman kesalahan spesifik penyedia *(misalnya, "NoSuchBucket")*  

### Apakah sumber daya menggantung yang teridentifikasi dapat berhasil diklaim?
- [ ] Tidak — penyedia memiliki perlindungan terhadap pengklaiman nama yang ditinggalkan atau diperlukan verifikasi akun  
- [ ] Ya — nama sumber daya **dapat** didaftarkan pada platform pihak ketiga oleh pengguna mana pun  

### Apakah domain utama menggunakan cookie dengan cakupan "Domain" yang dapat dikompromikan?
- [ ] Tidak — cookie dibatasi secara ketat pada subdomain tertentu atau menggunakan flag `Host-Only`  
- [ ] Ya — cookie sensitif (ID sesi, JWT) dicakupkan ke domain induk dan **dapat** dieksfiltrasi melalui subdomain yang dibajak  

### Apakah subdomain dapat digunakan untuk mem-bypass header keamanan atau alur autentikasi?
- [ ] Tidak — tidak ada ketergantungan keamanan pada subdomain yang teridentifikasi  
- [ ] Ya — subdomain masuk dalam daftar putih (whitelisted) dalam direktif `script-src` atau `connect-src` CSP  
- [ ] Ya — subdomain merupakan URI pengalihan (redirect URI) yang valid untuk konfigurasi OAuth atau SSO

---

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

## WSTG-CONF-12 — Content Security Policy (CSP)

Content Security Policy (CSP) adalah mekanisme pertahanan mendalam (defense-in-depth) kritis yang diimplementasikan melalui header respons HTTP untuk memitigasi risiko seperti Cross-Site Scripting (XSS), clickjacking, dan serangan data injection. Dengan mendefinisikan sumber daya dinamis mana yang diizinkan untuk dimuat, CSP yang dikonfigurasi dengan baik membatasi kemampuan penyerang untuk mengeksekusi skrip yang tidak sah atau mengeksfiltrasi data sensitif ke domain eksternal. Pentester mengevaluasi kebijakan untuk direktif yang terlalu permisif, penggunaan kata kunci yang tidak aman seperti `'unsafe-inline'`, dan ketergantungan pada CDN yang tidak aman yang mungkin memfasilitasi bypass. CSP yang lemah atau tidak ada tidak menciptakan kerentanan secara langsung tetapi secara signifikan meningkatkan dampak dari serangan injeksi yang berhasil karena gagal menyediakan lapisan perlindungan sekunder.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CONF-12 |
| **CWE** | CWE-693 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Rendah / Sedang |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/12-Test_for_Content_Security_Policy  
* https://hacktricks.wiki/en/pentesting-web/content-security-policy-csp-bypass/index.html  

**Alat:** `Google CSP Evaluator`, `Burp Suite (CSP Pro)`, `Mozilla Observatory`, `ZAP`, `CSP Mitigator`

### Apakah header Content Security Policy (CSP) terdapat dalam respons aplikasi?
- [ ] Ya — header `Content-Security-Policy` **ada** dan **diterapkan**  
- [ ] Ya — `Content-Security-Policy-Report-Only` **ada** untuk pengujian  
- [ ] Tidak — tidak ada header CSP yang **ada** dalam respons  

### Apakah direktif `script-src` atau `default-src` dibatasi dengan benar?
- [ ] Ya — direktif menggunakan whitelisting yang ketat, nonce, atau hash dan bypass **tidak dimungkinkan**  
- [ ] Ya — direktif **dikonfigurasi dengan benar** tetapi whitelist mencakup CDN yang diketahui dapat di-bypass  
- [ ] Tidak — direktif menggunakan wildcard (`*`) atau skema `data:`, sehingga membuat bypass **mungkin dilakukan**  

### Apakah kebijakan mengizinkan eksekusi skrip inline yang tidak aman atau eval?
- [ ] Tidak — `'unsafe-inline'` dan `'unsafe-eval'` **tidak ada**  
- [ ] Ya — `'unsafe-inline'` **ada** tetapi dilindungi oleh `nonce` atau `hash`  
- [ ] Ya — `'unsafe-inline'` atau `'unsafe-eval'` **diaktifkan** tanpa perlindungan tambahan *(Kritis)*  

### Apakah aplikasi terlindungi dari clickjacking melalui direktif `frame-ancestors`?
- [ ] Ya — `frame-ancestors` diatur ke `'none'` atau `'self'`  
- [ ] Ya — `frame-ancestors` **diaktifkan** tetapi mengizinkan domain pihak ketiga tepercaya tertentu  
- [ ] Tidak — `frame-ancestors` **tidak ada**, mengandalkan `X-Frame-Options` warisan atau tanpa perlindungan  

### Apakah terdapat mekanisme untuk melaporkan pelanggaran CSP?
- [ ] Ya — `report-uri` atau `report-to` **dikonfigurasi** dan aktif  
- [ ] Tidak — pelaporan pelanggaran **dinonaktifkan** atau **tidak dikonfigurasi**

---

## WSTG-CONF-13 — Path Confusion

Path Confusion muncul dari ketidaksesuaian dalam bagaimana berbagai komponen web, seperti reverse proxy, load balancer, dan server aplikasi backend, mengurai (parse) dan menafsirkan (interpret) URL path. Penyerang mengeksploitasi inkonsistensi ini dengan menyuntikkan karakter tertentu seperti titik koma, slash yang di-encode, atau segmen titik (dot-segments) untuk mengelabui filter keamanan dan mengakses endpoint yang dibatasi atau sumber daya statis. Kerentanan ini dapat menyebabkan kegagalan keamanan kritis, termasuk bypass autentikasi, akses data yang tidak sah, dan Web Cache Poisoning, karena front-end mungkin menerapkan aturan keamanan pada jalur yang ditafsirkan secara berbeda oleh back-end. Eksploitasi yang sukses biasanya terjadi pada batas arsitektural di mana perutean permintaan dan logika normalisasi berbeda di seluruh tech stack.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CONF-13 |
| **CWE** | CWE-444 |
| **Status Pengujian** | Belum Dilakukan |
| **Severitas** | Sedang / Tinggi* |

> *Severitas menjadi Tinggi jika path confusion menghasilkan bypass autentikasi atau kontrol akses untuk endpoint administratif yang sensitif.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/13-Test_for_Path_Confusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite`, `Dirsearch`, `FFUF`, `ParamMiner`, `Arjun`

### Apakah lapisan arsitektur yang berbeda menafsirkan pembatas jalur (path delimiters) (misalnya, `;`, `#`, `?`) secara konsisten?
- [ ] Ya — semua lapisan menormalisasi dan menafsirkan pembatas jalur secara identik  
- [ ] Tidak — terdapat inkonsistensi tetapi **tidak dapat** digunakan untuk mengakses sumber daya yang dibatasi  
- [ ] Tidak — perbedaan memungkinkan penyerang untuk melewati filter front-end dan mencapai logika back-end **adalah mungkin**  

### Apakah kontrol akses dapat di-bypass menggunakan urutan path traversal atau karakter yang di-encode?
- [ ] Tidak — normalisasi diterapkan secara konsisten sebelum pemeriksaan keamanan  
- [ ] Ya — bypass **mungkin dilakukan** melalui urutan segmen titik (misalnya, `/admin/..;/`)  
- [ ] Ya — bypass **mungkin dilakukan** melalui karakter URL-encoded (misalnya, `%2f`, `%2e%2e%2f`)  

### Apakah aplikasi rentan terhadap Web Cache Poisoning melalui path confusion?
- [ ] Tidak — logika caching tidak terpengaruh oleh ambiguitas jalur atau informasi jalur tambahan  
- [ ] Ya — konten berbahaya **dapat** di-cache untuk jalur yang sah karena ketidaksesuaian pemetaan  
- [ ] Ya — informasi sensitif **dapat** di-cache di direktori publik melalui teknik `RCD` (Relative Path Overwrite)  

### Apakah server menangani "informasi jalur tambahan" (PathInfo) secara aman?
- [ ] Tidak — fitur **dinonaktifkan** atau tidak ada  
- [ ] Ya — `PathInfo` ditangani dan **tidak** mengganggu filter keamanan  
- [ ] Tidak — `PathInfo` memungkinkan penyerang untuk menambahkan data yang membingungkan logika perutean aplikasi

---

## WSTG-CONF-14 — Pengujian Kesalahan Konfigurasi Header Keamanan HTTP Lainnya

Pengujian untuk kesalahan konfigurasi (misconfiguration) header keamanan HTTP melibatkan verifikasi keberadaan dan implementasi header pertahanan yang tepat, yang memberikan perlindungan esensial terhadap vektor serangan berbasis web yang umum. Meskipun header ini tidak memperbaiki kerentanan kode yang mendasarinya, ketiadaannya secara signifikan meningkatkan risiko serangan man-in-the-middle (MITM), eksploitasi MIME-sniffing, dan kebocoran data lintas-asal (cross-origin) yang tidak disengaja melalui referrer. Dari sudut pandang penyerang, header keamanan yang hilang adalah indikator visibilitas tinggi dari lingkungan yang tidak diperkuat dengan baik (poorly hardened), yang sering kali memfasilitasi rantai eksploitasi yang lebih kompleks seperti session hijacking atau eksfiltrasi informasi. Konfigurasi ini biasanya dievaluasi pada tingkat server dan harus diterapkan secara konsisten di semua jenis respons, termasuk endpoint API dan sumber daya statis.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CONF-14 |
| **CWE** | CWE-16 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Rendah / Sedang |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/14-Test_Other_HTTP_Security_Header_Misconfigurations  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `curl`, `Burp Suite (Repeater/Scanner)`, `OWASP ZAP`, `Hardenize`, `securityheaders.com`

### Audit Keberadaan Header Keamanan
| Header | Ada | Tidak Ada | Konfigurasi yang Direkomendasikan |
|--------|:-------:|:-------:|---------------------------|
| `Strict-Transport-Security` |  |  | `max-age=63072000; includeSubDomains; preload` |
| `X-Content-Type-Options` |  |  | `nosniff` |
| `Referrer-Policy` |  |  | `no-referrer` atau `strict-origin-when-cross-origin` |
| `Permissions-Policy` |  |  | (Spesifik-fitur, misal, `geolocation=()`) |
| `Cross-Origin-Opener-Policy` |  |  | `same-origin` |

### Bagaimana header keamanan ditegakkan di seluruh cakupan aplikasi?
- [ ] Ya — header diterapkan secara global melalui load balancer pusat atau reverse proxy dan **tidak dapat** ditimpa (overridden)  
- [ ] Ya — header diterapkan pada respons dokumen utama tetapi **tidak ada** pada respons API atau aset statis  
- [ ] No — header diterapkan secara tidak konsisten atau **tidak ada** di seluruh domain aplikasi  

### Apakah header Strict-Transport-Security (HSTS) dikonfigurasi dengan benar?
- [ ] Ya — `max-age` diatur ke durasi yang lama (misal, 2 tahun) dan `includeSubDomains` **diaktifkan**  
- [ ] Ya — `max-age` diatur tetapi `includeSubDomains` **dinonaktifkan**, membiarkan subdomain rentan  
- [ ] No — header HSTS **tidak ada** atau `max-age` diatur ke 0, memungkinkan serangan protocol downgrade  

### Apakah Referrer-Policy mencegah kebocoran parameter URL yang sensitif?
- [ ] Ya — kebijakan diatur ke `no-referrer` atau `same-origin` *(Paling aman)*  
- [ ] Ya — kebijakan diatur ke `strict-origin-when-cross-origin`  
- [ ] No — kebijakan **tidak diatur** atau diatur ke `unsafe-url`, berpotensi membocorkan token atau PII sensitif dalam header referer  

### Apakah header X-Content-Type-Options mencegah MIME-sniffing?
- [ ] Ya — direktif `nosniff` **ada** dan diimplementasikan dengan benar  
- [ ] No — header **tidak ada**, berpotensi memungkinkan browser untuk mengeksekusi file yang tidak dapat dieksekusi (misal, gambar) sebagai skrip

---

## WSTG-IDNT-01 — Pengujian Definisi Peran (Test Role Definitions)

Pengujian untuk definisi peran melibatkan identifikasi dan dokumentasi berbagai peran pengguna dan tingkat izin (permission levels) yang ditentukan dalam aplikasi untuk memastikan mereka dipisahkan secara logis dan mematuhi prinsip hak istimewa terendah (principle of least privilege). Seorang penyerang akan menganalisis aplikasi untuk memetakan peran dengan hak istimewa tinggi (high-privilege roles) versus peran pengguna standar, mencari ambiguitas dalam batasan izin atau fungsi administratif yang tidak terdokumentasi. Mengidentifikasi peran-peran ini adalah langkah pertama dalam menemukan jalur eskalasi hak istimewa (privilege escalation paths), karena ketidakkonsistenan dalam definisi peran sering kali menyebabkan akses tidak sah ke data sensitif atau fitur administratif. Fase ini biasanya terjadi selama pengintaian awal (initial reconnaissance) dan pemetaan logika bisnis (business logic mapping), yang berfokus pada profil pengguna, dasbor administratif, dan struktur izin API.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-IDNT-01 |
| **CWE** | CWE-285 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Rendah / Sedang |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/01-Test_Role_Definitions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite (Compare Site Maps)`, `Postman`, `AuthMatrix`, `Authz`, `Browser Developer Tools`

### Apakah peran didefinisikan dan didokumentasikan dengan jelas di dalam aplikasi atau dokumentasinya?
- [ ] Ya — peran didokumentasikan sepenuhnya dan mengikuti prinsip **least privilege**  
- [ ] Ya — peran didokumentasikan tetapi mengandung **izin yang tumpang tindih (overlapping permissions)**  
- [ ] Tidak — tidak ada dokumentasi atau definisi peran formal yang tersedia  

### Apakah terdapat pemisahan yang jelas antara fungsi administratif dan fungsi pengguna standar?
- [ ] Ya — fungsi administratif **terisolasi sepenuhnya** dari tampilan pengguna standar  
- [ ] Ya — terdapat pemisahan tetapi endpoint administratif bersifat **dapat diprediksi atau ditebak (predictable or guessable)**  
- [ ] Tidak — fungsi administratif dan standar **tercampur** dalam antarmuka yang sama  

### Apakah aplikasi menggunakan mekanisme terpusat untuk Role-Based Access Control (RBAC)?
- [ ] Ya — RBAC atau ABAC terpusat telah **diimplementasikan** dan ditegakkan secara konsisten  
- [ ] Ya — terdapat kontrol terdesentralisasi tetapi **diterapkan secara tidak konsisten** di berbagai modul  
- [ ] Tidak — logika kontrol akses **dihardcode** ke dalam halaman atau komponen individual  

### Apakah terdapat peran yang tidak terdokumentasi atau tersembunyi di dalam sistem?
- [ ] Tidak — semua peran yang ditemukan sesuai dengan fungsionalitas yang didokumentasikan  
- [ ] Ya — peran tersembunyi atau "shadow" (misalnya, `super_admin`, `support`, `test`) **tersedia**  

### Dapatkah pengguna dengan hak istimewa rendah melakukan enumerasi izin atau peran pengguna lain?
- [ ] Tidak — pengguna **tidak dapat** melihat atau melakukan enumerasi peran/izin dari entitas lain  
- [ ] Ya — pengguna **dapat** melakukan enumerasi peran melalui halaman profil, metadata, atau respons API *(Sedang)*

---

## WSTG-IDNT-02 — Menguji Proses Registrasi Pengguna

Menguji proses registrasi pengguna melibatkan evaluasi alur kerja di mana identitas baru dibuat untuk memastikan proses tersebut tidak dapat disalahgunakan untuk akses yang tidak sah atau eksploitasi otomatis. Kerentanan dalam proses ini sering kali bermanifestasi sebagai kurangnya verifikasi identitas (email/SMS), kerentanan terhadap pembuatan akun massal melalui bot, atau pengungkapan informasi melalui pesan kesalahan yang bertele-tele yang mengungkapkan nama pengguna yang sudah ada. Penyerang mengeksploitasi celah ini untuk melakukan User Enumeration, melakukan kampanye spam skala besar, atau memanipulasi parameter registrasi untuk mendapatkan hak akses yang lebih tinggi (Privilege Escalation) selama fase pembuatan akun. Pengujian ini sangat penting untuk melindungi integritas aplikasi dan mencegah penyerang mengisi sistem dengan akun palsu atau berbahaya.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-IDNT-02 |
| **CWE** | CWE-836 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Sedang |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/02-Test_User_Registration_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite`, `OWASP ZAP`, `Python`, `ffuf`, `Cuppa`

### Apakah verifikasi identitas diperlukan sebelum akun baru menjadi aktif?
- [ ] Ya — registrasi memerlukan token unik dengan batas waktu yang dikirim melalui email atau SMS  
- [ ] Ya — verifikasi diperlukan tetapi token bersifat **lemah** atau **dapat diprediksi**  
- [ ] Tidak — akun diaktifkan secara **langsung** tanpa langkah verifikasi apa pun  

### Apakah proses registrasi mencegah User Enumeration?
- [ ] Ya — aplikasi mengembalikan respons yang **identik** baik untuk email/username baru maupun yang sudah ada  
- [ ] Tidak — pesan kesalahan (misalnya, "Email sudah digunakan") **memungkinkan** penyerang untuk memetakan pengguna yang terdaftar  
- [ ] Tidak — perbedaan waktu dalam respons server **mengungkapkan** apakah seorang pengguna ada atau tidak  

### Apakah kontrol anti-otomatisasi diterapkan untuk mencegah registrasi massal?
- [ ] Ya — mekanisme CAPTCHA atau proof-of-work **tersedia** dan **efektif**  
- [ ] Ya — kontrol tersedia tetapi bypass **mungkin dilakukan** melalui endpoint API atau manipulasi header  
- [ ] Tidak — tidak ada Rate Limiting atau CAPTCHA yang **diterapkan** pada endpoint registrasi  

### Apakah parameter registrasi dapat dimanipulasi untuk meningkatkan hak akses?
- [ ] Tidak — peran pengguna ditetapkan **secara ketat** oleh logika sisi server  
- [ ] Ya — parameter seperti `role`, `admin`, atau `group_id` **dapat** dimodifikasi dalam request untuk mendapatkan akses yang lebih tinggi  

### Apakah kebijakan kata sandi (Password Policy) ditegakkan selama fase registrasi?
- [ ] Ya — persyaratan kata sandi yang kuat **ditegakkan** di sisi server  
- [ ] Tidak — kata sandi yang lemah (misalnya, "password123") **diterima** oleh sistem

---

## WSTG-IDNT-03 — Pengujian Proses Penyediaan Akun (Account Provisioning)

Proses penyediaan akun (Account Provisioning) mengatur bagaimana identitas baru dibuat, diverifikasi, dan diberi hak istimewa dalam lingkungan aplikasi. Penyerang menargetkan alur kerja ini untuk melakukan pendaftaran massal otomatis untuk spam atau Denial-of-Service (DoS), melewati langkah verifikasi identitas untuk membuat akun palsu, atau memanipulasi parameter untuk memberikan diri mereka izin yang ditingkatkan selama fase pendaftaran. Kerentanan sering kali muncul dalam formulir pendaftaran mandiri (self-service), sistem khusus undangan (invitation-only), atau konsol penyediaan administratif di mana kelemahan logika bisnis (business logic flaws) memungkinkan pembuatan akun yang tidak sah atau eskalasi hak istimewa (Privilege Escalation). Dari sudut pandang penyerang, mengompromikan alur penyediaan memberikan pijakan yang sah yang dapat digunakan sebagai basis untuk eksploitasi lebih dalam, eksfiltrasi data, atau serangan rekayasa sosial (Social Engineering).


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-IDNT-03 |
| **CWE** | CWE-285 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Menengah / Tinggi* |

> *Tingkat keparahan menjadi Kritis jika penyerang dapat menyediakan akun administratif atau melewati batasan pendaftaran global.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/03-Test_Account_Provisioning_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite`, `FFUF`, `Postman`, `Python`, `MailHog`

### Apakah pendaftaran mandiri (self-registration) dikontrol secara ketat atau dibatasi hanya untuk pengguna yang berwenang?
- [ ] Ya — pendaftaran **dinonaktifkan** atau memerlukan persetujuan administratif manual  
- [ ] Ya — pendaftaran terbuka tetapi dibatasi untuk **domain email yang berwenang** atau token undangan  
- [ ] Tidak — pendaftaran **terbuka** untuk semua pengguna tanpa batasan domain atau token  

### Apakah mekanisme verifikasi identitas yang kuat (misalnya, email/SMS) diperlukan sebelum aktivasi akun?
- [ ] Ya — verifikasi **diperlukan** dan **tidak dapat** dilewati  
- [ ] Ya — verifikasi **diperlukan** tetapi **dapat** dilewati melalui manipulasi parameter (Parameter Tampering) atau token yang dapat diprediksi  
- [ ] Tidak — akun **langsung aktif** segera setelah pengiriman tanpa verifikasi apa pun  

### Dapatkah pengguna memanipulasi parameter peran (Role) atau izin selama permintaan pendaftaran?
- [ ] Tidak — peran **ditetapkan di sisi server (Server-Side)** dan tidak ada parameter di sisi klien (Client-Side)  
- [ ] Ya — parameter peran ada tetapi manipulasi **tidak dimungkinkan** karena adanya validasi sisi server  
- [ ] Ya — parameter peran/izin (misalnya, `role=admin`, `is_staff=true`) **dapat** dimodifikasi untuk mendapatkan akses yang ditingkatkan *(Kritis)*  

### Apakah pembatasan laju (Rate Limiting) atau kontrol anti-otomatisasi diterapkan untuk mencegah pembuatan akun massal?
- [ ] Ya — Rate Limiting dan CAPTCHA **aktif** dan efektif  
- [ ] Ya — kontrol tersedia tetapi bypass **dimungkinkan** melalui spoofing header atau layanan pemecah CAPTCHA  
- [ ] Tidak — tidak ada Rate Limiting atau kontrol anti-otomatisasi yang **diterapkan**  

### Apakah proses penyediaan membocorkan informasi tentang akun yang ada (User Enumeration)?
- [ ] No — pesan respons **identik** untuk pengguna yang sudah ada dan yang tidak ada  
- [ ] Yes — perbedaan respons atau variasi waktu **memungkinkan** dilakukannya enumerasi (User Enumeration) pengguna yang sudah ada

---

## WSTG-IDNT-04 — Testing for Account Enumeration and Guessable User Account

Enumerasi Akun (Account Enumeration) terjadi ketika sebuah aplikasi mengungkapkan keberadaan atau ketidakberadaan akun pengguna melalui variasi dalam respons HTTP, pesan kesalahan, atau perbedaan waktu (timing differences) yang terukur. Penyerang mengeksploitasi perilaku ini dengan secara sistematis mengirimkan daftar nama pengguna untuk mengidentifikasi akun yang valid, yang selanjutnya ditargetkan untuk Credential Stuffing, serangan Brute Force, atau Social Engineering. Kerentanan ini umumnya muncul di portal login, fitur reset kata sandi, formulir registrasi, dan endpoint API yang menangani pengidentifikasi spesifik pengguna. Dari perspektif penyerang, enumerasi yang berhasil secara signifikan mempersempit permukaan serangan (attack surface) dengan mengubah upaya Brute Force buta menjadi eksploitasi terarah terhadap identitas valid yang telah diketahui.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-IDNT-04 |
| **CWE** | CWE-204 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Rendah / Menengah |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/04-Testing_for_Account_Enumeration_and_Guessable_User_Account  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite (Intruder/Comparer)`, `ffuf`, `Wfuzz`, `GoBuster`, `Hashcat`

### Apakah pesan kesalahan autentikasi konsisten di semua skenario?
- [ ] Ya — pesan kesalahan generik digunakan baik untuk nama pengguna yang tidak valid maupun kata sandi yang tidak valid  
- [ ] Ya — pesan serupa, tetapi terdapat sedikit variasi dalam tanda baca atau penggunaan huruf besar/kecil **ditemukan**  
- [ ] Tidak — pesan kesalahan secara eksplisit menyatakan "User not found" atau "Incorrect password" *(Rentan)*  

### Apakah alur reset kata sandi atau "forgot password" membocorkan keberadaan akun?
- [ ] Tidak — aplikasi mengembalikan pesan generik terlepas dari apakah email/username tersebut ada atau tidak  
- [ ] Ya — kontrol telah diterapkan, tetapi bypass **memungkinkan** melalui panjang respons atau kode status  
- [ ] Ya — aplikasi secara eksplisit mengonfirmasi jika email reset telah dikirim atau jika akun **tidak ditemukan**  

### Apakah proses registrasi terlindungi dari enumerasi pengguna?
- [ ] Tidak — registrasi tidak membocorkan informasi atau menggunakan CAPTCHA/Rate Limiting untuk mencegah pengecekan massal  
- [ ] Ya — kontrol telah diterapkan, tetapi bypass **memungkinkan** melalui perbedaan waktu atau respons API  
- [ ] Ya — aplikasi segera memberi tahu pengguna jika username atau email **sudah digunakan**  

### Apakah perbedaan waktu (timing differences) dapat diukur saat membandingkan username yang valid vs. tidak valid?
- [ ] Tidak — waktu respons konsisten terlepas dari validitas akun karena penggunaan perbandingan waktu konstan (constant-time comparisons)  
- [ ] Ya — terdapat perbedaan waktu tetapi terlalu kecil untuk diukur secara andal melalui jaringan  
- [ ] Ya — perbedaan waktu yang terukur **ditemukan** (misalnya, karena eksekusi hashing kata sandi hanya dilakukan untuk pengguna yang valid)  

### Apakah aplikasi menggunakan konvensi penamaan akun yang dapat diprediksi atau ditebak?
- [ ] Tidak — pengidentifikasi akun bersifat acak (UUID) atau ditentukan oleh pengguna dan kompleks  
- [ ] Ya — pengidentifikasi akun mengikuti pola (misalnya, `emp001`, `emp002`) dan enumerasi **memungkinkan**  
- [ ] Ya — pengidentifikasi berupa integer berurutan, sehingga memungkinkan enumerasi database secara penuh

---

## WSTG-IDNT-05 — Pengujian Kebijakan Nama Pengguna (Username) yang Lemah atau Tidak Diterapkan

Pengujian terhadap kebijakan nama pengguna (username) yang lemah atau tidak diterapkan berfokus pada aturan yang mengatur pembuatan pengenal akun dan kerentanannya terhadap enumerasi atau spoofing. Kebijakan yang lemah sering kali mengizinkan urutan yang dapat diprediksi, kata-kata kamus umum, atau pengenal yang mencerminkan ID karyawan internal, yang secara signifikan mengurangi ruang pencarian untuk serangan Brute Force dan Credential Stuffing. Dari sudut pandang penyerang, mengidentifikasi pola-pola ini adalah langkah pertama dalam penemuan akun skala besar, yang sering kali dicapai dengan menganalisis pesan kesalahan pendaftaran, perilaku pengaturan ulang kata sandi (password reset), atau metadata dalam profil publik. Memastikan kebijakan yang kuat mencegah penyerang memetakan basis pengguna secara sistematis dan memfasilitasi pertahanan terhadap Phishing yang ditargetkan serta upaya bypass autentikasi otomatis.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-IDNT-05 |
| **CWE** | CWE-521 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Rendah / Sedang |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/05-Testing_for_Weak_or_Unenforced_Username_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite (Intruder)`, `ffuf`, `Custom Python Scripts`, `theHarvester`

### Apakah nama pengguna didasarkan pada pola yang sangat mudah diprediksi atau berurutan?
- [ ] Tidak — nama pengguna bersifat acak, ditentukan oleh pengguna, atau merupakan string dengan entropi tinggi  
- [ ] Ya — nama pengguna mengikuti format yang dapat diprediksi (misalnya, `user1001`, `user1002`) namun autentikasi bersifat kuat  
- [ ] Ya — nama pengguna bersifat **sangat berurutan** atau mengikuti format korporat yang diketahui, sehingga memudahkan enumerasi  

### Apakah aplikasi menerapkan persyaratan panjang minimum dan kompleksitas untuk nama pengguna?
- [ ] Ya — kebijakan panjang dan set karakter yang ketat **diterapkan** selama pendaftaran  
- [ ] Ya — kebijakan tersedia tetapi mengizinkan nama pengguna yang sangat pendek (misalnya, 1-2 karakter) atau terlalu sederhana  
- [ ] Tidak — **tidak ada kebijakan** yang diterapkan terkait panjang nama pengguna atau jenis karakter  

### Dapatkah nama pengguna yang valid dienumerasi melalui respons aplikasi?
- [ ] Tidak — aplikasi mengembalikan pesan kesalahan generik dan menunjukkan waktu respons yang konsisten untuk semua upaya  
- [ ] Ya — aplikasi mengembalikan pesan kesalahan yang berbeda (misalnya, "Nama pengguna sudah digunakan") tetapi Rate Limiting **diterapkan**  
- [ ] Ya — aplikasi **membocorkan** nama pengguna yang valid melalui kesalahan pendaftaran, pengaturan ulang kata sandi (password reset), atau perbedaan waktu (timing differences)  

### Apakah kebijakan mencegah pendaftaran nama pengguna administratif yang umum atau dicadangkan (reserved)?
- [ ] Ya — aplikasi memblokir nama-nama umum (misalnya, `admin`, `root`, `support`) dan istilah yang dicadangkan oleh sistem  
- [ ] Tidak — pengguna **dapat** mendaftarkan akun dengan nama yang sensitif atau administratif  

### Apakah kebijakan nama pengguna konsisten di semua titik masuk (API, Mobile, Web)?
- [ ] Ya — kebijakan **diterapkan** secara konsisten di semua antarmuka dan versi  
- [ ] Tidak — endpoint lama (legacy) atau versi API tertentu **tidak** menerapkan batasan nama pengguna yang sama

---

## WSTG-ATHN-01 — Pengujian Kredensial yang Dikirim melalui Saluran Terenkripsi

Pengujian kredensial yang dikirim melalui saluran terenkripsi memastikan bahwa data autentikasi sensitif, seperti username, password, dan session token, terlindungi dari intersepsi selama transit. Penyerang yang berada di jaringan—misalnya melalui serangan Man-in-the-Middle (MitM) pada Wi-Fi publik atau jaringan internal yang terkompromi—dapat menggunakan alat packet sniffing untuk menangkap kredensial cleartext jika TLS/SSL tidak diterapkan dengan benar. Kerentanan ini biasanya terjadi pada endpoint login, formulir reset password, dan header autentikasi API di mana aplikasi gagal mewajibkan HTTPS atau melakukan pengalihan (redirect) yang tidak tepat dari HTTP. Eksploitasi yang sukses memberikan penyerang akses penuh ke akun pengguna, yang menyebabkan kompromi total pada data pengguna dan potensi lateral movement di dalam aplikasi.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-ATHN-01 |
| **CWE** | CWE-319 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/01-Testing_for_Credentials_Transported_over_an_Encrypted_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Alat:** `Wireshark`, `tcpdump`, `Burp Suite`, `OWASP ZAP`, `testssl.sh`, `sslyze`, `nmap`

### Apakah HTTPS diwajibkan untuk seluruh proses autentikasi?
- [ ] Ya — HSTS **diaktifkan** dan semua trafik dipaksa secara ketat melalui HTTPS  
- [ ] Ya — pengalihan (redirection) ke HTTPS terjadi, tetapi HSTS **tidak diaktifkan**  
- [ ] Tidak — kredensial **dapat** dikirimkan melalui HTTP yang tidak terenkripsi  

### Apakah halaman login dan formulir disajikan melalui saluran terenkripsi?
- [ ] Ya — halaman yang berisi formulir login dikirimkan melalui HTTPS  
- [ ] Tidak — halaman login disajikan melalui HTTP, memungkinkan pembajakan form-action atau sniffing kredensial  

### Apakah `Secure` flag diterapkan pada semua cookie sesi yang sensitif?
- [ ] Ya — `Secure` flag **diterapkan** pada semua cookie autentikasi dan sesi  
- [ ] Ya — `Secure` flag **diterapkan** hanya pada beberapa cookie saja  
- [ ] Tidak — `Secure` flag **tidak diterapkan**, memungkinkan cookie untuk dieksfiltrasi melalui permintaan yang tidak terenkripsi  

### Apakah server mendukung versi TLS yang lemah atau cipher suites yang tidak aman?
- [ ] Tidak — hanya TLS modern (1.2+) dan cipher yang kuat yang **diaktifkan**  
- [ ] Ya — versi lama (TLS 1.0/1.1) atau cipher yang lemah (RC4, DES, CBC) masih **didukung**  

### Apakah aplikasi mencegah transmisi kredensial ke domain pihak ketiga melalui saluran yang tidak terenkripsi?
- [ ] Ya — tidak ada data sensitif yang dikirim ke domain eksternal atau semua panggilan eksternal dipaksa melalui HTTPS  
- [ ] Tidak — header autentikasi atau kredensial dikirim ke endpoint pihak ketiga (misalnya, analitik atau SSO) melalui HTTP

---

## WSTG-ATHN-02 — Testing for Default Credentials

Pengujian untuk *default credentials* melibatkan identifikasi antarmuka administratif, layanan, atau komponen aplikasi yang masih menggunakan nama pengguna (*username*) dan kata sandi (*password*) yang ditetapkan dari pabrik atau disediakan oleh vendor. Kerentanan ini merupakan target prioritas tinggi bagi penyerang karena sering kali menyediakan jalur langsung menuju kompromi sistem secara penuh, akses administratif, atau *remote code execution* (RCE) dengan upaya minimal. Hal ini biasanya terjadi pada perangkat lunak *off-the-shelf*, platform CMS, alat manajemen basis data, dan perangkat keras terintegrasi seperti perangkat IoT atau peralatan jaringan. Eksploitasi biasanya dilakukan dengan mencocokkan versi perangkat lunak yang diidentifikasi dengan basis data kata sandi bawaan publik atau menggunakan alat *credential stuffing* otomatis selama fase pengintaian (*reconnaissance*) dan eksploitasi awal.


| Kolom | Nilai |
|---|---|
| **WSTG ID** | WSTG-ATHN-02 |
| **CWE** | CWE-1392 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Kritis / Tinggi |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/02-Testing_for_Default_Credentials  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Alat:** `Hydra`, `Medusa`, `Burp Suite (Intruder)`, `Nmap`, `Metasploit`, `DefaultPassword.com`

### Apakah antarmuka administratif atau manajemen terpapar ke internet atau segmen jaringan yang tidak tepercaya?
- [ ] Tidak — tidak ada antarmuka manajemen yang dapat diakses  
- [ ] Ya — antarmuka tersedia tetapi dibatasi oleh kontrol tingkat jaringan (misalnya, VPN, IP Allowlist)  
- [ ] Ya — antarmuka **dapat** diakses secara publik dan memerlukan autentikasi  

### Apakah default credentials yang disediakan vendor telah diubah untuk semua komponen yang diidentifikasi?
- [ ] Ya — semua *default credentials* **telah diubah** di seluruh komponen *(Paling aman)*  
- [ ] Ya — kredensial **telah diubah** untuk aplikasi utama, tetapi komponen sekunder (misalnya, CMS, DB) tetap menggunakan bawaan  
- [ ] Tidak — *default credentials* **masih aktif** pada satu atau lebih antarmuka yang dapat diidentifikasi *(Kritis)*  

### Apakah aplikasi mewajibkan perubahan kata sandi pada saat login pertama untuk akun baru atau administratif?
- [ ] Ya — perubahan kata sandi **bersifat wajib** sebelum tindakan administratif apa pun dapat dilakukan  
- [ ] Tidak — aplikasi **mengizinkan** penggunaan *default credentials* tanpa batas waktu  

### Apakah mekanisme penguncian (lockout) atau kontrol pembatasan laju (rate-limiting) aktif untuk mencegah pengujian default credential secara otomatis?
- [ ] Ya — *rate limiting* yang agresif atau penguncian akun (*account lockout*) **diterapkan**  
- [ ] Ya — kontrol tersedia tetapi bypass **mungkin dilakukan** melalui manipulasi *header* atau rotasi IP  
- [ ] Tidak — tidak ada mekanisme *rate limiting* atau penguncian yang **diaktifkan**  

### Apakah plugin pihak ketiga, middleware, atau alat administratif (misalnya, phpMyAdmin, Tomcat Manager) menggunakan kredensial yang unik?
- [ ] Tidak — komponen pihak ketiga tidak ada di dalam lingkungan  
- [ ] Ya — semua komponen pihak ketiga menggunakan kredensial yang unik dan bukan bawaan  
- [ ] Tidak — setidaknya satu komponen pihak ketiga **dapat diakses** melalui *default credentials* yang sudah diketahui

---

## WSTG-ATHN-03 — Testing for Weak Lock Out Mechanism

Mekanisme penguncian akun (account lockout mechanism) adalah kontrol pertahanan berlapis (defense-in-depth) yang krusial yang dirancang untuk memitigasi serangan brute-force dan credential stuffing dengan menonaktifkan akun setelah sejumlah upaya autentikasi yang gagal yang telah ditentukan. Tanpa kebijakan penguncian yang kuat, penyerang dapat menggunakan alat otomatis untuk menebak kata sandi secara sistematis pada beberapa akun (password spraying) atau menargetkan satu akun bernilai tinggi hingga berhasil. Kerentanan ini biasanya muncul pada portal login, formulir reset kata sandi, dan API endpoint yang tidak memiliki pembatasan laju (rate limiting) atau perlindungan tingkat akun. Dari sudut pandang penyerang, mekanisme penguncian yang lemah atau tidak ada memungkinkan upaya menebak kata sandi yang tidak terbatas, sementara mekanisme yang terlalu agresif dapat dieksploitasi untuk menyebabkan Denial of Service (DoS) terdistribusi terhadap pengguna yang sah dengan sengaja mengunci akun mereka.

| Kolom | Nilai |
|---|---|
| **WSTG ID** | WSTG-ATHN-03 |
| **CWE** | CWE-307 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Sedang / Tinggi* |

> *Tingkat keparahan menjadi Tinggi jika aplikasi menangani PII sensitif, data finansial, atau jika akun administratif rentan terhadap brute-force tanpa adanya kontrol sekunder.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/03-Testing_for_Weak_Lock_Out_Mechanism  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Alat:** `Burp Suite Intruder`, `THC-Hydra`, `ffuf`, `Patator`, `wfuzz`

### Apakah mekanisme penguncian akun diterapkan setelah sejumlah upaya gagal yang ditentukan?
- [ ] Ya — akun dikunci setelah ambang batas yang wajar dan ditentukan (misalnya, 5-10 upaya)  
- [ ] Ya — akun dikunci tetapi hanya setelah jumlah upaya yang sangat tinggi  
- [ ] Tidak — akun **tidak dapat** dikunci dan memungkinkan brute-force tanpa batas  

### Apakah mekanisme penguncian dapat dilewati menggunakan teknik penghindaran (evasion) umum?
- [ ] Tidak — penguncian diterapkan di sisi server (server-side) dan **tidak** dipengaruhi oleh perubahan di sisi klien (client-side)  
- [ ] Ya — penguncian **mungkin** dilewati dengan merotasi alamat IP atau menggunakan proxy pool  
- [ ] Ya — penguncian **mungkin** dilewati dengan memanipulasi header HTTP seperti `X-Forwarded-For` atau `User-Agent`  
- [ ] Ya — penguncian **mungkin** dilewati dengan menghapus cookie atau memulai sesi baru  

### Apakah mekanisme penguncian menyediakan jalur untuk pemulihan akun atau kedaluwarsa?
- [ ] Ya — akun dibuka secara otomatis setelah durasi yang ditentukan atau melalui verifikasi email  
- [ ] Ya — akun memerlukan intervensi administratif untuk dibuka  
- [ ] Tidak — penguncian bersifat permanen tanpa jalur pemulihan yang jelas, meningkatkan risiko DoS  

### Apakah aplikasi membedakan antara nama pengguna yang valid dan tidak valid selama penguncian?
- [ ] Tidak — respons aplikasi identik baik untuk pengguna yang ada maupun yang tidak ada  
- [ ] Ya — aplikasi mengungkapkan jika sebuah akun dikunci, memungkinkan terjadinya **enumerasi nama pengguna (username enumeration)**  

### Apakah mekanisme penguncian rentan terhadap serangan Denial of Service (DoS)?
- [ ] Tidak — kontrol seperti CAPTCHA atau rate limiting berbasis IP mencegah penguncian akun secara massal  
- [ ] Ya — penyerang **dapat** secara sistematis mengunci semua pengguna yang diketahui dengan memberikan kata sandi yang salah

---

## WSTG-ATHN-04 — Testing for Bypassing Authentication Schema

Melewati skema autentikasi (Bypassing authentication schema) melibatkan identifikasi dan eksploitasi celah dalam logika aplikasi yang memungkinkan penyerang mendapatkan akses ke sumber daya yang dilindungi tanpa memberikan kredensial yang valid. Kerentanan ini biasanya terjadi ketika aplikasi mengandalkan kontrol sisi klien (client-side controls), gagal menegakkan otorisasi sisi server (server-side authorization) pada setiap permintaan, atau membiarkan "backdoor" administratif dan endpoint debug terekspos di lingkungan produksi. Penyerang mengeksploitasi kelemahan ini dengan melakukan forced browsing ke URL sensitif, memanipulasi parameter HTTP seperti `authenticated=true`, atau melakukan spoofing header untuk menipu aplikasi agar menganggap sesi telah terbentuk. Eksploitasi yang berhasil mengakibatkan runtuhnya perimeter autentikasi secara total, yang berpotensi menyebabkan eksfiltrasi data yang tidak sah atau kompromi sistem secara penuh.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-ATHN-04 |
| **CWE** | CWE-287 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Kritis |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/04-Testing_for_Bypassing_Authentication_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Alat:** `Burp Suite`, `Ffuf`, `Gobuster`, `Dirsearch`, `Postman`

### Apakah halaman internal yang sensitif dapat diakses langsung tanpa sesi aktif?
- [ ] Tidak — akses langsung menghasilkan pengalihan ke login atau respons 403/404  
- [ ] Ya — beberapa halaman sensitif dapat diakses tetapi menyediakan data yang terbatas atau tidak sensitif  
- [ ] Ya — halaman administratif sensitif atau halaman khusus pengguna **dapat** diakses sepenuhnya tanpa autentikasi *(Kritis)*  

### Apakah aplikasi mengandalkan flag atau parameter sisi klien untuk menentukan status autentikasi?
- [ ] Tidak — status autentikasi dikelola secara ketat melalui status sesi sisi server  
- [ ] Ya — flag sisi klien ada tetapi modifikasi **tidak** memberikan akses ke area yang dilindungi  
- [ ] Ya — memodifikasi parameter (contoh: `is_authenticated=true`, `user_role=admin`) **memungkinkan** bypass autentikasi  

### Apakah autentikasi dapat dilewati dengan melakukan spoofing atau manipulasi header HTTP tertentu?
- [ ] Tidak — header seperti `X-Forwarded-For`, `Referer`, atau header khusus tidak berdampak pada autentikasi  
- [ ] Ya — memanipulasi header (contoh: `X-Custom-IP-Authorization`, `X-Remote-User`) **mungkin dilakukan** dan memberikan akses yang tidak sah  

### Apakah terdapat jalur alternatif atau artefak pengembangan yang menghindari alur login standar?
- [ ] Tidak — semua sumber daya yang dilindungi dijaga oleh pemeriksaan autentikasi terpusat yang siap produksi  
- [ ] Ya — endpoint lama (legacy), rute debug (contoh: `/debug/login`), atau versi API yang terlupakan **dapat** diakses tanpa kredensial  

### Apakah aplikasi gagal melakukan autentikasi ulang atau memverifikasi sesi untuk tindakan dengan hak istimewa tinggi?
- [ ] Tidak — tindakan dengan hak istimewa tinggi atau sensitif memerlukan token sesi yang baru atau valid  
- [ ] Ya — setelah bypass ditemukan pada satu endpoint, hal itu **dapat** digunakan untuk melakukan tindakan administratif di seluruh aplikasi

---

## WSTG-ATHN-05 — Testing for Vulnerable Remember Password

Fungsionalitas "Remember Me" atau login persisten dirancang untuk mempertahankan status autentikasi pengguna meskipun peramban (browser) dimulai ulang dengan cara menyimpan token atau kredensial pada sisi klien (client-side). Jika fitur ini diimplementasikan dengan buruk—seperti menyimpan kata sandi dalam teks biasa (plaintext), hash yang lemah, atau token dengan entropi rendah dalam cookie—hal ini akan mengekspos pengguna terhadap pengambilalihan akun (account takeover) melalui Cross-Site Scripting (XSS), akses fisik, atau intersepsi jaringan. Pentester mengevaluasi entropi dari token persisten ini, flag keamanan yang diterapkan pada mekanisme penyimpanan, dan siklus hidup sesi (session lifecycle) untuk memastikan bahwa kenyamanan akses persisten tidak melewati kontrol keamanan kritis seperti Multi-Factor Authentication (MFA) atau pembatalan validitas sesi (session invalidation). Eksploitasi sering kali melibatkan pemanenan token yang berumur panjang ini untuk mendapatkan akses tidak sah ke akun tanpa memerlukan kredensial asli.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-ATHN-05 |
| **CWE** | CWE-522 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan (Severity)** | Sedang / Tinggi* |

> *Keparahan menjadi Tinggi jika kredensial teks biasa (plaintext) atau hash tanpa salt disimpan dalam cookie persisten, atau jika token melewati Multi-Factor Authentication (MFA).

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/05-Testing_for_Vulnerable_Remember_Password  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  
* https://portswigger.net/web-security/authentication  

**Alat:** `Burp Suite (Cookie Editor/Sequencer)`, `Browser Developer Tools`, `EditThisCookie`

### Apakah aplikasi mengimplementasikan fitur "Remember Me" atau "Tetap Masuk"?
- [ ] Tidak — fitur **tidak** ada di dalam aplikasi  
- [ ] Ya — fitur ada dan menggunakan token yang kuat secara kriptografis dan tidak dapat diprediksi  
- [ ] Ya — fitur ada tetapi menggunakan token yang dapat diprediksi atau memiliki entropi rendah *(Sedang)*  
- [ ] Ya — fitur ada dan menyimpan kredensial dalam teks biasa (plaintext) atau kata sandi terenkode base64 *(Tinggi)*  

### Apakah flag keamanan diterapkan dengan benar pada cookie persisten?
- [ ] Ya — flag `HttpOnly`, `Secure`, dan `SameSite` telah **diterapkan**  
- [ ] Ya — beberapa flag diterapkan tetapi `HttpOnly` **tidak ada**  
- [ ] Ya — beberapa flag diterapkan tetapi `Secure` **tidak ada** pada koneksi HTTPS  
- [ ] Tidak — tidak ada flag keamanan yang **diterapkan** pada cookie persisten  

### Apakah token "Remember Me" tetap valid setelah logout manual?
- [ ] Tidak — token segera dibatalkan validitasnya di sisi server (server-side) setelah logout  
- [ ] Ya — token tetap valid tetapi memerlukan kata sandi untuk tindakan sensitif  
- [ ] Ya — token tetap valid dan memungkinkan akses akun penuh **tanpa** autentikasi ulang *(Sedang)*  

### Apakah sesi persisten dihentikan setelah dilakukan perubahan kata sandi?
- [ ] Ya — semua token persisten dicabut (revoked) ketika pengguna mengubah kata sandi mereka  
- [ ] Tidak — token "Remember Me" tetap valid setelah pengaturan ulang/perubahan kata sandi **dilakukan** *(Tinggi)*  

### Apakah token persisten melewati Multi-Factor Authentication (MFA) pada kunjungan berikutnya?
- [ ] Tidak — MFA tetap diperlukan meskipun terdapat token "Remember Me"  
- [ ] Ya — MFA dilewati, tetapi token terikat pada sidik jari perangkat (device fingerprint) tertentu  
- [ ] Ya — MFA **sepenuhnya dilewati** hanya dengan menggunakan cookie persisten dari perangkat apa pun *(Tinggi)*

---

## WSTG-ATHN-06 — Testing for Browser Cache Weakness

Kelemahan cache browser (browser cache weakness) terjadi ketika informasi sensitif disimpan dalam cache browser lokal dan dapat diambil oleh pengguna yang tidak berwenang yang memiliki akses ke mesin fisik yang sama. Kurangnya header kontrol cache yang tepat memungkinkan data yang berpotensi sensitif seperti informasi pribadi, rincian akun, atau pengenal sesi (session identifiers) tetap ada setelah pengguna keluar (logout) atau menutup sesi. Penyerang atau pengguna berikutnya pada terminal bersama atau publik dapat mengeksploitasi hal ini dengan menelusuri riwayat browser, menggunakan tombol "Back", atau memeriksa file cache lokal untuk mengeksfiltrasi respons yang tersimpan dalam cache. Memastikan penerapan direktif yang ketat seperti `Cache-Control: no-store` dan `Pragma: no-cache` sangat penting bagi aplikasi apa pun yang menangani konten terautentikasi untuk memastikan data sensitif tidak pernah ditulis ke disk.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-ATHN-06 |
| **CWE** | CWE-525 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Rendah / Sedang* |

> *Keparahan menjadi Sedang jika aplikasi sering diakses dari kios bersama/publik atau berisi data PII/PHI yang sangat sensitif.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/06-Testing_for_Browser_Cache_Weaknesses  

**Alat:** `Burp Suite (Proxy/Repeater)`, `Browser Developer Tools (Network Tab)`, `Zed Attack Proxy (ZAP)`

### Apakah header cache-control yang sesuai tersedia pada halaman terautentikasi atau sensitif?
- [ ] Ya — `Cache-Control: no-store, no-cache` dan `Pragma: no-cache` **ada** dan **dikonfigurasi dengan benar**  
- [ ] Ya — beberapa header ada tetapi `no-store` **hilang**, memungkinkan penyimpanan cache ke disk  
- [ ] Tidak — tidak ada header terkait cache yang **diterapkan**, bergantung pada perilaku default browser  

### Apakah aplikasi menggunakan direktif `private` untuk data spesifik pengguna?
- [ ] Ya — `Cache-Control: private` **diaktifkan** untuk semua konten yang dipersonalisasi guna mencegah cache oleh proxy  
- [ ] Tidak — konten ditandai sebagai `public` atau tidak memiliki direktif `private`, sehingga dapat **disimpan dalam cache** oleh proxy perantara  

### Apakah informasi sensitif dapat diakses melalui tombol "Back" browser setelah logout berhasil?
- [ ] Tidak — browser meminta autentikasi ulang atau halaman **tidak dimuat** dari cache lokal  
- [ ] Ya — informasi sensitif **masih terlihat** melalui tombol "Back" setelah sesi diakhiri  

### Apakah file non-HTML yang sensitif (misalnya, PDF, ekspor CSV, respons JSON API) terlindungi dari caching?
- [ ] Tidak — tidak ada file non-HTML sensitif yang dihasilkan atau ditangani oleh aplikasi  
- [ ] Ya — header cache-control yang ketat **diterapkan** pada semua unduhan file sensitif dan endpoint API  
- [ ] Tidak — unduhan sensitif atau respons API **disimpan** dalam cache browser lokal atau direktori sementara  

### Apakah aplikasi menggunakan `Expires: 0` atau tanggal di masa lalu untuk mencegah penggunaan data yang sudah usang?
- [ ] Ya — header `Expires` disetel ke `0` atau tanggal historis, **memastikan** browser menganggap konten segera usang  
- [ ] Tidak — header `Expires` **hilang** atau disetel ke tanggal di masa depan untuk halaman sensitif

---

## WSTG-ATHN-07 — Pengujian untuk Kebijakan Kata Sandi yang Lemah

Pengujian untuk kebijakan kata sandi yang lemah melibatkan evaluasi terhadap persyaratan kompleksitas, panjang, dan entropi yang diterapkan oleh aplikasi selama pembuatan akun dan pembaruan kredensial. Kebijakan yang tidak memadai secara langsung membahayakan kerahasiaan akun pengguna dengan membuatnya rentan terhadap automated dictionary attacks, Brute Force attempts, dan Credential Stuffing. Kerentanan ini biasanya terdapat pada registration endpoints, password reset workflows, dan panel manajemen pengguna administratif. Dari sudut pandang penyerang, kebijakan yang permisif secara signifikan mengurangi biaya komputasi dan waktu yang diperlukan untuk berhasil mengompromikan kredensial menggunakan wordlist umum atau pattern-based cracking.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-ATHN-07 |
| **CWE** | CWE-521 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Menengah / Rendah* |

> *Tingkat keparahan menjadi Tinggi jika aplikasi tidak memiliki mekanisme account lockout atau Rate Limiting untuk mencegah penebakan otomatis.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/07-Testing_for_Weak_Authentication_Methods  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**Alat:** `Burp Suite (Intruder)`, `ZAP (Fuzzer)`, `Hydra`, `Hashcat`, `Cewl`

### Apakah panjang kata sandi minimum diterapkan oleh aplikasi?
- [ ] Ya — panjang minimum adalah 12+ karakter *(Paling aman)*  
- [ ] Ya — panjang minimum antara 8 hingga 11 karakter  
- [ ] Ya — panjang minimum **kurang dari** 8 karakter  
- [ ] Tidak — tidak ada panjang minimum yang diterapkan  

### Apakah persyaratan kompleksitas (huruf besar, huruf kecil, angka, simbol) diterapkan?
- [ ] Ya — beberapa kelas karakter bersifat wajib dan Exploit/Bypass **tidak dimungkinkan**  
- [ ] Ya — kelas karakter disarankan tetapi **tidak** diwajibkan  
- [ ] Tidak — tidak ada persyaratan kompleksitas yang diterapkan  

### Apakah aplikasi memblokir kata sandi umum/lemah atau kata sandi yang mengandung data spesifik pengguna?
- [ ] Ya — kata sandi lemah yang diketahui (misal: "password123") dan kata sandi berbasis username **tidak dapat** digunakan  
- [ ] Ya — kata sandi umum diblokir, tetapi data spesifik pengguna (misal: username, email) **dapat** digunakan  
- [ ] Tidak — string apa pun yang memenuhi persyaratan panjang/kompleksitas akan diterima  

### Dapatkah persyaratan kompleksitas atau panjang di-bypass melalui modifikasi client-side?
- [ ] Tidak — kebijakan diterapkan secara ketat pada **server-side**  
- [ ] Ya — kebijakan diterapkan melalui JavaScript dan **dapat** di-bypass dengan melakukan intersepsi request  

### Apakah kebijakan kata sandi dikomunikasikan dengan jelas kepada pengguna tanpa membocorkan detail implementasi?
- [ ] Ya — kebijakan terlihat saat input dan memberikan pesan kegagalan yang bersifat generik  
- [ ] Ya — kebijakan terlihat tetapi pesan kegagalan mengungkapkan regex tertentu atau celah logika  
- [ ] Tidak — kebijakan **tidak** terlihat, memaksa penemuan melalui trial-and-error

---

## WSTG-ATHN-08 — Pengujian Jawaban Pertanyaan Keamanan yang Lemah

Pengujian terhadap jawaban pertanyaan keamanan yang lemah melibatkan evaluasi mekanisme Knowledge-Based Authentication (KBA) yang digunakan untuk memverifikasi identitas pengguna, biasanya selama alur pemulihan kata sandi atau Multi-Factor Authentication (MFA). Hal ini penting karena pertanyaan keamanan sering kali bergantung pada informasi statis dan tidak rahasia yang dapat dengan mudah ditemukan melalui Open-Source Intelligence (OSINT) atau Social Engineering, yang menyebabkan Account Takeover yang tidak sah. Kerentanan ini biasanya terjadi pada modul "Lupa Kata Sandi" atau "Buka Kunci Akun" di mana pengguna memberikan jawaban atas pertanyaan yang telah ditentukan sebelumnya. Dari sudut pandang penyerang, ini adalah target utama untuk Brute Force otomatis atau riset yang ditargetkan, karena banyak pengguna memberikan jawaban yang dapat diprediksi atau dapat diverifikasi secara publik untuk pertanyaan umum seperti "Siapa nama gadis ibu kandung Anda?" atau "Di SMA mana Anda bersekolah?".


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-ATHN-08 |
| **CWE** | CWE-640 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Sedang / Tinggi* |

> *Keparahan menjadi Tinggi jika pertanyaan keamanan merupakan satu-satunya syarat untuk reset kata sandi penuh dan Account Takeover tanpa verifikasi lebih lanjut.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/08-Testing_for_Weak_Security_Question_Answer  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**Alat:** `Burp Suite (Intruder)`, `ffuf`, `Maltego`, `Sherlock`, `Social Engineering Toolkit (SET)`

### Apakah aplikasi menerapkan pertanyaan keamanan untuk verifikasi identitas atau pemulihan kata sandi?
- [ ] Tidak — pertanyaan keamanan **tidak digunakan** untuk autentikasi atau pemulihan  
- [ ] Ya — pertanyaan keamanan **digunakan** sebagai faktor sekunder opsional  
- [ ] Ya — pertanyaan keamanan **digunakan** sebagai mekanisme pemulihan utama/satu-satunya *(Risiko Tinggi)*  

### Apakah pertanyaan keamanan dipilih dari daftar tetap atau ditentukan oleh pengguna?
- [ ] Tidak — pertanyaan sepenuhnya ditentukan oleh pengguna dan memerlukan entropi tinggi  
- [ ] Ya — pertanyaan bersifat tetap tetapi mencakup opsi yang tidak dapat ditemukan melalui OSINT  
- [ ] Ya — pertanyaan berasal dari daftar tetap topik umum yang dapat ditemukan melalui OSINT *(Rentan)*  

### Apakah Rate Limiting atau Account Lockout diterapkan pada upaya menjawab pertanyaan keamanan?
- [ ] Ya — Rate Limiting ketat dan Account Lockout **diterapkan** untuk mencegah Brute Force  
- [ ] Ya — Rate Limiting **diterapkan** tetapi bypass **dimungkinkan** melalui rotasi IP atau manipulasi header  
- [ ] Tidak — **tidak ada Rate Limiting** yang diberlakukan pada upaya menjawab  

### Apakah aplikasi memberlakukan kompleksitas atau validasi pada konten jawaban?
- [ ] Ya — validasi mencegah kata-kata umum dan memastikan kompleksitas/keunikan jawaban  
- [ ] Ya — validasi dasar tersedia tetapi **dapat** di-bypass dengan wordlist umum atau Dictionary Attack  
- [ ] Tidak — **tidak ada validasi** yang dilakukan; jawaban sederhana atau satu karakter **diizinkan**  

### Bisakah tantangan pertanyaan keamanan di-bypass melalui celah logika (Logical Flaws)?
- [ ] Tidak — alur pemulihan secara ketat memerlukan jawaban yang valid untuk melanjutkan  
- [ ] Ya — alur pemulihan **dapat** di-bypass dengan memanipulasi kode respons HTTP atau parameter sesi  
- [ ] Ya — jawaban tercermin dalam kode sumber atau hidden fields pada halaman

---

## WSTG-ATHN-09 — Testing for Weak Password Change or Reset Functionalities

Mekanisme perubahan dan reset kata sandi adalah komponen kritis dari manajemen autentikasi yang, jika diimplementasikan dengan tidak tepat, memungkinkan penyerang mengambil alih akun pengguna. Kerentanan ini biasanya melibatkan reset token yang dapat diprediksi, kurangnya account lockouts selama upaya Brute Force, pengiriman token yang tidak aman, atau kemampuan untuk mengubah kata sandi tanpa memverifikasi kata sandi saat ini. Penyerang mengeksploitasi celah ini dengan mencegat atau menebak tautan pemulihan, melakukan Host Header Injection untuk mengalihkan email reset, atau memanfaatkan Session Fixation untuk mempertahankan akses setelah perubahan kata sandi. Memastikan proses ini memerlukan verifikasi yang kuat dan menggunakan keacakan kriptografis sangat penting untuk menjaga integritas akun dan mencegah akses yang tidak sah.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-ATHN-09 |
| **CWE** | CWE-640 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi / Kritis |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/09-Testing_for_Weak_Password_Change_or_Reset_Functionalities  
* https://hacktricks.wiki/en/pentesting-web/reset-password.html  

**Tools:** `Burp Suite (Repeater/Intruder)`, `Python (Custom Scripts)`, `CyberChef`, `OAST (Interactsh/Burp Collaborator)`

### Apakah kata sandi saat ini diperlukan untuk menetapkan kata sandi baru selama perubahan standar?
- [ ] Ya — kata sandi saat ini **diperlukan** dan diverifikasi  
- [ ] Ya — kata sandi saat ini **diperlukan** tetapi dapat di-bypass melalui Parameter Pollution atau manipulasi  
- [ ] No — kata sandi saat ini **tidak** diminta, memungkinkan pengambilalihan akun melalui Session Hijacking *(Tinggi)*  

### Apakah token reset kata sandi aman secara kriptografis dan tidak dapat diprediksi?
- [ ] Ya — token memiliki entropi tinggi, acak, dan **tidak dapat diprediksi**  
- [ ] Ya — token dihasilkan tetapi menunjukkan entropi rendah atau pola yang dapat diprediksi (misal: berbasis timestamp)  
- [ ] No — token **dapat diprediksi** atau didasarkan pada data pengguna statis seperti email/ID yang dienkode Base64 *(Kritis)*  

### Bisakah tautan reset kata sandi dialihkan melalui Host Header Injection?
- [ ] No — aplikasi mengabaikan atau memvalidasi header `Host` untuk pembuatan tautan  
- [ ] Ya — aplikasi menggunakan header `Host` atau `X-Forwarded-Host` untuk menyusun tautan, tetapi validasi **ada**  
- [ ] Ya — tautan **dapat** dialihkan ke domain yang dikendalikan penyerang melalui manipulasi header *(Tinggi)*  

### Apakah token reset memiliki siklus hidup yang terbatas dan pembatalan segera?
- [ ] Ya — token kedaluwarsa dengan cepat dan dibatalkan **segera** setelah digunakan atau perubahan kata sandi  
- [ ] Ya — token kedaluwarsa setelah durasi yang lama (misal: 24+ jam) atau memungkinkan **penggunaan berulang**  
- [ ] No — token **tidak pernah kedaluwarsa** atau tetap valid setelah kata sandi berhasil diubah  

### Apakah ada Rate Limiting atau penguncian pada endpoint reset dan perubahan kata sandi?
- [ ] Ya — Rate Limiting yang ketat dan CAPTCHA **diterapkan** untuk mencegah enumerasi dan Brute Force  
- [ ] Ya — kontrol terbatas tersedia tetapi bypass **mungkin dilakukan** melalui rotasi IP atau header spoofing  
- [ ] No — tidak ada Rate Limiting yang **diterapkan**, memungkinkan enumerasi akun massal atau Brute Force pada token

---

## WSTG-ATHN-10 — Pengujian Otentikasi yang Lebih Lemah pada Saluran Alternatif

Pengujian otentikasi yang lebih lemah pada saluran alternatif melibatkan identifikasi dan analisis jalur sekunder menuju akses akun—seperti API seluler, alur kerja pemulihan kata sandi, antarmuka help desk, atau subdomain legacy—yang mungkin tidak menerapkan ketegasan keamanan yang sama dengan portal web utama. Saluran alternatif ini seringkali tidak memiliki Multi-Factor Authentication (MFA) yang kuat, Rate Limiting yang ketat, atau persyaratan kata sandi yang kompleks, sehingga menciptakan "titik terlemah" yang merusak seluruh sistem otentikasi. Penyerang secara khusus menargetkan titik masuk yang terabaikan ini untuk melakukan Credential Stuffing atau melewati MFA dengan mengeksploitasi perbedaan dalam cara antarmuka yang berbeda menangani verifikasi identitas. Memastikan kesetaraan (parity) di semua permukaan otentikasi sangat penting untuk mencegah akses yang tidak sah dan menjaga integritas sesi serta data pengguna.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-ATHN-10 |
| **CWE** | CWE-287 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/10-Testing_for_Weaker_Authentication_in_Alternative_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite`, `Postman`, `cURL`, `MobSF`, `Ffuf`

### Apakah saluran otentikasi alternatif ditemukan dan diidentifikasi?
- [ ] Tidak — hanya terdapat satu saluran otentikasi  
- [ ] Ya — beberapa saluran teridentifikasi (misalnya, API aplikasi seluler, desktop client, portal legacy, layanan SOAP)  

### Apakah saluran alternatif menerapkan kesetaraan keamanan dengan saluran utama?
- [ ] Ya — semua saluran menerapkan persyaratan kompleksitas kata sandi dan MFA yang **identik**  
- [ ] Ya — saluran menerapkan kompleksitas kata sandi tetapi MFA **tidak diwajibkan** atau **dapat** dilewati (bypass)  
- [ ] Tidak — saluran alternatif memiliki persyaratan otentikasi yang **jauh lebih lemah**  

### Apakah rate limiting dan penguncian akun (account lockout) konsisten di seluruh saluran?
- [ ] Ya — Rate Limiting dan lockout **diterapkan secara konsisten** di semua endpoint  
- [ ] Ya — kontrol tersedia namun bypass **dimungkinkan** pada saluran tertentu (misalnya, API seluler)  
- [ ] Tidak — Rate Limiting atau lockout **tidak diterapkan** pada jalur otentikasi sekunder  

### Dapatkah MFA dilewati dengan beralih ke saluran alternatif?
- [ ] Tidak — MFA bersifat wajib dan **tidak dapat** dilewati terlepas dari titik masuknya  
- [ ] Ya — MFA diwajibkan pada portal web tetapi **tidak diterapkan** pada API seluler atau legacy  

### Apakah alur pemulihan kata sandi atau "lupa kata sandi" lebih lemah daripada login standar?
- [ ] Tidak — alur pemulihan menggunakan verifikasi out-of-band yang kuat dan tidak membocorkan informasi  
- [ ] Ya — alur pemulihan menggunakan pertanyaan keamanan yang lemah yang **dapat** ditebak atau diriset dengan mudah  
- [ ] Ya — alur pemulihan rentan terhadap Enumeration atau **tidak** memerlukan tingkat pembuktian yang sama dengan login standar

---

## WSTG-ATHN-11 — Testing Multi-Factor Authentication (MFA)

Pengujian Multi-Factor Authentication (MFA) mengevaluasi ketangguhan lapisan keamanan sekunder yang dirancang untuk mencegah akses yang tidak sah bahkan ketika kredensial utama telah terkompromi. Penyerang sering kali mencoba melewati MFA dengan mengidentifikasi endpoint di mana penerapannya tidak konsisten, seperti versi API lama (legacy), backend seluler, atau alur kerja pengaturan ulang kata sandi (password reset workflows). Eksploitasi sering kali melibatkan manipulasi respons server, melakukan brute-force pada kode yang berumur pendek (OTP), atau memanfaatkan celah kondisi balapan (race conditions) dan manajemen sesi yang memungkinkan pengguna bertransisi ke status terautentikasi tanpa memberikan faktor kedua.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-ATHN-11 |
| **CWE** | CWE-287 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Kerentanan** | Tinggi / Kritis |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/11-Testing_Multi-Factor_Authentication  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite (Intruder/Repeater/Turbo Intruder)`, `Postman`, `Mitproxy`

### Apakah MFA diterapkan secara konsisten di seluruh portal autentikasi?
- [ ] Ya — MFA diwajibkan untuk semua upaya masuk (login) berbasis web, seluler, dan API  
- [ ] Ya — tetapi MFA **tidak diterapkan** pada endpoint tertentu (misalnya, `/api/v1/login` lama atau portal khusus seluler)  
- [ ] Tidak — MFA **tidak diimplementasikan** dalam aplikasi *(Kritis)*  

### Dapatkah langkah verifikasi MFA dilewati melalui navigasi URL langsung atau manipulasi respons?
- [ ] Tidak — navigasi langsung atau manipulasi parameter **tidak dapat** melewati tantangan (challenge) tersebut  
- [ ] Ya — menavigasi langsung ke URL dasbor internal **dimungkinkan** tanpa menyelesaikan tantangan MFA  
- [ ] Ya — memanipulasi respons server (misalnya, mengubah `401 Unauthorized` menjadi `200 OK`) **dimungkinkan** untuk mendapatkan akses  

### Apakah proses verifikasi kode MFA dilindungi dari serangan brute-force?
- [ ] Ya — pembatasan laju (Rate Limiting) yang ketat atau penguncian akun (account lockout) **diterapkan** setelah beberapa upaya OTP yang gagal  
- [ ] Ya — pembatasan laju ada tetapi **dimungkinkan** untuk dilewati melalui rotasi IP atau manipulasi header  
- [ ] Tidak — tidak ada pembatasan laju yang diterapkan, dan brute-force kode secara otomatis **dimungkinkan**  

### Apakah aplikasi mempertahankan status sesi yang aman selama transisi MFA?
- [ ] Ya — token sesi dengan hak istimewa tinggi dikeluarkan **hanya setelah** penyelesaian MFA yang berhasil  
- [ ] Tidak — cookie sesi yang berfungsi penuh dikeluarkan **sebelum** MFA selesai, memungkinkan akses ke beberapa fitur  
- [ ] Tidak — aplikasi menggunakan pengidentifikasi statis atau transisi sesi yang dapat diprediksi sehingga **dapat** dibajak  

### Apakah faktor MFA alternatif (SMS, Email, Kode Cadangan) rentan terhadap eksploitasi?
- [ ] Tidak — semua faktor menggunakan nilai yang aman, tidak dapat diprediksi, dan terbatas waktu  
- [ ] Ya — kode cadangan (backup codes) dapat diprediksi atau **dapat** dienumerasi  
- [ ] Ya — fungsionalitas "Kirim Ulang Kode" dapat disalahgunakan untuk melakukan SMS/Email flooding atau mengungkapkan detail kontak parsial

---

## WSTG-ATHZ-01 — Pengujian Directory Traversal File Include

Kerentanan directory traversal dan file inclusion muncul ketika sebuah aplikasi menggunakan input yang dapat dikontrol pengguna untuk menyusun path ke file atau direktori tanpa validasi atau sanitasi yang memadai. Penyerang mengeksploitasi celah ini dengan menyuntikkan sekuens seperti `../` untuk menavigasi ke luar direktori yang dimaksud, yang berpotensi mengakses file sistem yang sensitif, data konfigurasi, atau kode sumber aplikasi. Dalam kasus yang lebih parah yang melibatkan Local File Inclusion (LFI) atau Remote File Inclusion (RFI), penyerang dapat mencapai eksekusi kode jarak jauh (remote code execution) dengan menyertakan skrip berbahaya atau memanfaatkan log poisoning dan PHP wrapper. Kerentanan ini biasanya berada pada parameter yang digunakan untuk pemuatan konten dinamis, template engine, atau endpoint pengambilan gambar di mana logika sisi server menangani path sistem file.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-ATHZ-01 |
| **CWE** | CWE-22 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi / Kritis |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/01-Testing_Directory_Traversal_File_Include  
* https://hacktricks.wiki/en/pentesting-web/file-inclusion/index.html  
* https://portswigger.net/web-security/file-path-traversal  

**Alat:** `Burp Suite`, `FFUF`, `DotDotPwn`, `LFI Suite`, `Wfuzz`

### Apakah parameter menerima nama file atau path untuk pemrosesan di sisi server?
- [ ] Tidak — tidak ada parameter yang terlihat berinteraksi dengan sistem file  
- [ ] Ya — parameter tersedia namun menggunakan daftar izin (allowlist) yang ketat untuk pengenal file  
- [ ] Ya — parameter menerima nama file atau path secara langsung  

### Apakah input disanitasi untuk mencegah sekuens directory traversal?
- [ ] Ya — input divalidasi terhadap allowlist yang ketat dan sekuens traversal **tidak dimungkinkan**  
- [ ] Ya — input disanitasi dengan menghapus sekuens `../`, namun bypass rekursif **mungkin dilakukan**  
- [ ] Tidak — tidak ada sanitasi atau validasi yang **diterapkan** pada input terkait path  

### Apakah mungkin untuk mengakses file di luar direktori yang dibatasi melalui sekuens traversal?
- [ ] Tidak — aplikasi atau OS mencegah akses ke file di luar cakupan yang ditentukan  
- [ ] Ya — akses ke file di dalam web root **dimungkinkan**  
- [ ] Ya — akses ke file sistem yang sensitif (misalnya, `/etc/passwd`, `C:\Windows\win.ini`) **dimungkinkan** *(Kritis)*  

### Apakah server mengizinkan penyertaan URL jarak jauh (RFI)?
- [ ] Tidak — remote file inclusion **dinonaktifkan** pada level server/aplikasi  
- [ ] Ya — file jarak jauh **dapat** disertakan namun eksekusi **tidak dimungkinkan**  
- [ ] Ya — file jarak jauh **dapat** disertakan dan dieksekusi di server *(Kritis)*  

### Apakah filter dapat di-bypass menggunakan encoding atau karakter khusus?
- [ ] Tidak — filter secara efektif menangani berbagai encoding dan null byte  
- [ ] Ya — bypass **dimungkinkan** menggunakan URL encoding, double URL encoding, atau 16-bit Unicode  
- [ ] Ya — bypass **dimungkinkan** menggunakan injeksi null byte (`%00`) atau filesystem wrapper (misalnya, `php://filter`)

---

## WSTG-ATHZ-02 — Pengujian untuk Melewati Skema Otorisasi

Melewati skema otorisasi (Bypassing authorization schemas) terjadi ketika aplikasi gagal menerapkan kontrol akses (access controls), yang memungkinkan penyerang untuk mengakses sumber daya atau fungsi di luar izin yang seharusnya. Kerentanan ini biasanya bermanifestasi sebagai Eskalasi Hak Istimewa Horizontal (Horizontal Privilege Escalation), di mana penyerang mengakses data milik pengguna lain dengan tingkat yang sama, atau Eskalasi Hak Istimewa Vertikal (Vertical Privilege Escalation), di mana pengguna dengan hak istimewa rendah memperoleh kemampuan administratif. Penyerang mengeksploitasi cacat ini dengan memanipulasi parameter permintaan seperti ID sumber daya (Resource ID), memodifikasi token sesi (session tokens), atau memanfaatkan header HTTP seperti `X-Original-URL` untuk menghindari pembatasan berbasis jalur (path-based restrictions). Memastikan otorisasi yang kuat sangat penting untuk menjaga kerahasiaan data dan mencegah tindakan administratif tidak sah yang dapat membahayakan seluruh sistem.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-ATHZ-02 |
| **CWE** | CWE-285 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi / Kritis |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/02-Testing_for_Bypassing_Authorization_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**Alat:** `Burp Suite (Autorize extension)`, `Burp Suite (Repeater/Intruder)`, `Postman`, `cURL`, `OWASP ZAP`

### Apakah eskalasi hak istimewa horizontal dicegah di antara pengguna dengan peran yang sama?
- [ ] Ya — pemeriksaan otorisasi **diterapkan** pada setiap permintaan sumber daya dan bypass **tidak mungkin dilakukan**  
- [ ] Ya — pemeriksaan otorisasi **diterapkan** tetapi dapat dilewati melalui IDOR atau manipulasi parameter (parameter tampering)  
- [ ] Tidak — pengguna **dapat** mengakses data milik pengguna lain hanya dengan mengubah ID sumber daya *(Tinggi)*  

### Apakah eskalasi hak istimewa vertikal dicegah untuk pengguna dengan hak istimewa rendah?
- [ ] Tidak — aplikasi hanya memiliki satu tingkat hak istimewa  
- [ ] Ya — fungsi administratif **tidak dapat** diakses oleh pengguna dengan hak istimewa rendah  
- [ ] Ya — fungsi administratif disembunyikan di UI tetapi **dapat** diakses melalui URL langsung  
- [ ] Tidak — pengguna dengan hak istimewa rendah **dapat** melakukan tindakan administratif dengan memanipulasi peran atau izin *(Kritis)*  

### Apakah otorisasi dapat dilewati menggunakan manipulasi kata kerja HTTP (HTTP verb tampering)?
- [ ] Tidak — kontrol akses diterapkan terlepas dari metode HTTP yang digunakan  
- [ ] Ya — mengubah metode (misalnya, dari `GET` ke `POST` atau `PUT`) **melewati** pemeriksaan otorisasi  

### Apakah titik akhir (endpoints) administratif atau terbatas dapat diakses tanpa autentikasi apa pun?
- [ ] Tidak — semua titik akhir sensitif memerlukan sesi yang valid dan izin yang tepat  
- [ ] Ya — beberapa titik akhir administratif dapat diakses jika penyerang mengetahui URL langsungnya  
- [ ] Ya — akses tanpa autentikasi ke fungsi sensitif **dimungkinkan** *(Kritis)*  

### Apakah header HTTP memungkinkan pengelabuan otorisasi berbasis jalur (path-based authorization)?
- [ ] Tidak — aplikasi **tidak** rentan terhadap bypass berbasis header  
- [ ] Ya — header seperti `X-Original-URL`, `X-Rewrite-URL`, atau `X-Forwarded-For` **dapat** digunakan untuk melewati kontrol akses

---

## WSTG-ATHZ-03 — Testing for Privilege Escalation

Eskalasi hak istimewa (Privilege Escalation) terjadi ketika penyerang mengeksploitasi kerentanan untuk mendapatkan akses ke sumber daya atau fungsionalitas yang disediakan bagi pengguna dengan tingkat otorisasi yang lebih tinggi atau identitas yang berbeda. Dalam eskalasi vertikal, pengguna standar mencoba mengakses fungsi administratif, sementara eskalasi horizontal melibatkan akses ke data milik pengguna lain dengan tingkat hak istimewa yang sama. Celah ini biasanya bermanifestasi dalam Access Control Lists (ACL) yang diimplementasikan dengan buruk, Insecure Direct Object References (IDOR), atau manipulasi parameter (parameter tampering) di dalam sesi atau token identitas. Dari sudut pandang penyerang, hal ini sering dicapai dengan memanipulasi ID numerik, memodifikasi parameter berbasis peran (role-based parameters) dalam cookie atau JWT, atau memanggil secara langsung endpoint API tersembunyi yang tidak memiliki pemeriksaan otorisasi di sisi server.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-ATHZ-03 |
| **CWE** | CWE-269 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi / Kritis |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/03-Testing_for_Privilege_Escalation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**Peralatan:** `Burp Suite`, `Authorize`, `AuthMatrix`, `Authz`, `Postman`, `Ffuf`

### Apakah aplikasi mengimplementasikan beberapa tingkat hak istimewa atau multi-tenancy?
- [ ] Tidak — aplikasi adalah sistem pengguna tunggal atau peran tunggal  
- [ ] Ya — beberapa peran atau penyewa (tenants) **telah** didefinisikan dan memerlukan pengujian otorisasi  

### Apakah eskalasi hak istimewa horizontal (akses tingkat yang sama) dimungkinkan?
- [ ] Tidak — pemeriksaan otorisasi **diterapkan** pada semua pengidentifikasi sumber daya dan pemilik sesi  
- [ ] Ya — akses ke data pengguna lain **dimungkinkan** melalui IDOR atau parameter tampering  
- [ ] Ya — kebocoran data **dimungkinkan** tetapi modifikasi data pengguna lain **tidak dimungkinkan**  

### Apakah eskalasi hak istimewa vertikal (rendah-ke-tinggi) dimungkinkan?
- [ ] Tidak — endpoint administratif secara ketat menegakkan Role-Based Access Controls (RBAC)  
- [ ] Ya — fungsi administratif **dapat** diakses oleh pengguna dengan hak istimewa rendah melalui akses URL langsung  
- [ ] Ya — fungsi administratif **dapat** diakses dengan memanipulasi header terkait peran, cookie, atau klaim JWT  

### Dapatkah peran atau izin pengguna dimodifikasi melalui Mass Assignment atau Parameter Pollution?
- [ ] Tidak — bidang berbasis peran bersifat hanya-baca (read-only) dan **tidak dapat** dimodifikasi oleh pengguna  
- [ ] Ya — izin pengguna **dapat** ditingkatkan dengan menyertakan parameter tersembunyi (misalnya, `role=admin`) dalam pembaruan profil atau registrasi  

### Apakah pemeriksaan otorisasi ditegakkan secara konsisten di semua versi API dan metode HTTP?
- [ ] Ya — kontrol **diterapkan** secara konsisten di semua versi dan metode  
- [ ] Tidak — versi API lama (misalnya, `/v1/`) atau metode HTTP tertentu (misalnya, `PUT`, `DELETE`, `PATCH`) **melewati (bypass)** otorisasi  
- [ ] Tidak — fungsi administratif disembunyikan di UI tetapi **diaktifkan** pada tingkat API untuk semua pengguna

---

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

## WSTG-ATHZ-05 — Menguji Kelemahan OAuth

Kelemahan OAuth mencakup berbagai kecacatan dalam implementasi protokol OAuth 2.0 atau OpenID Connect (OIDC), yang sering kali berakibat pada pengambilalihan akun secara penuh (*account takeover*) atau akses data yang tidak sah. Kerentanan ini biasanya muncul selama alur otorisasi (*authorization flow*), khususnya terkait validasi `redirect_uri`, entropi dan verifikasi parameter `state`, serta penanganan Access Token atau ID Token secara aman. Penyerang mengeksploitasi hal ini dengan mencegat kode otorisasi, mengarahkan token ke domain yang dikendalikan penyerang melalui *open redirect*, atau melakukan Cross-Site Request Forgery (CSRF) untuk menghubungkan sesi korban ke akun penyerang. Karena OAuth sering berfungsi sebagai mekanisme autentikasi utama, satu kesalahan konfigurasi dapat merusak integritas seluruh sistem identitas pengguna.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-ATHZ-05 |
| **CWE** | CWE-287 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Tinggi / Kritis |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/05-Testing_for_OAuth_Weaknesses  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/oauth  

**Alat:** `Burp Suite (Repeater/Collaborator)`, `CyberChef`, `OIDC-Checker`, `OAuth-Scanner`

### Apakah parameter `state` diimplementasikan dan divalidasi untuk mencegah CSRF?
- [ ] Ya — `state` bersifat wajib, unik per sesi, dan divalidasi secara ketat di sisi server  
- [ ] Ya — `state` ada tetapi bersifat statis atau dapat diprediksi di berbagai sesi yang berbeda  
- [ ] Ya — `state` ada tetapi aplikasi **tidak** memvalidasinya saat pemanggilan balik (*callback*)  
- [ ] Tidak — parameter `state` **tidak** digunakan dalam permintaan otorisasi *(Kritis)*  

### Apakah `redirect_uri` divalidasi secara ketat terhadap daftar putih (whitelist)?
- [ ] Ya — hanya kecocokan persis terhadap daftar putih yang telah didaftarkan sebelumnya yang **diterima**  
- [ ] Ya — validasi diterapkan tetapi bypass **memungkinkan** melalui *path traversal* atau manipulasi subdomain  
- [ ] Ya — validasi diterapkan tetapi bypass **memungkinkan** melalui *parameter pollution* atau injeksi fragmen  
- [ ] Tidak — aplikasi **mengizinkan** URL arbitrer dalam parameter `redirect_uri` *(Kritis)*  

### Dapatkah `response_type` dimanipulasi untuk mengubah alur (flow)?
- [ ] Tidak — aplikasi hanya menerima alur yang dimaksudkan (misalnya, `code`) dan menolak yang lain  
- [ ] Ya — beralih dari `code` ke `token` (Implicit flow) **memungkinkan** dan membocorkan token di URL  
- [ ] Ya — alur hibrida (*hybrid flows*) atau nilai `response_type` yang tidak sah **diaktifkan** dan diproses  

### Apakah token akses atau kode otorisasi bocor melalui header Referer atau riwayat browser?
- [ ] Tidak — token/kode ditangani dengan aman dan halaman sensitif menggunakan `Referrer-Policy` yang tepat  
- [ ] Ya — token/kode **bocor** ke domain pihak ketiga melalui header `Referer`  
- [ ] Ya — token/kode **terlihat** di riwayat browser karena caching yang tidak aman atau penggunaan permintaan GET  

### Apakah aplikasi menerapkan prinsip "Least Privilege" untuk cakupan (scopes) OAuth?
- [ ] Ya — aplikasi hanya meminta cakupan minimum yang diperlukan untuk fungsionalitas tersebut  
- [ ] Tidak — aplikasi meminta cakupan yang berlebihan atau cakupan administratif secara default  
- [ ] Tidak — pengguna **tidak dapat** meninjau atau membatalkan pilihan cakupan tertentu selama fase persetujuan (*consent phase*)

---

## WSTG-SESS-01 — Testing for Session Management Schema

Pengujian skema manajemen sesi berfokus pada analisis bagaimana aplikasi menangani siklus hidup dan struktur pengidentifikasi sesi untuk memastikan ketahanannya terhadap prediksi dan pembajakan (hijacking). Penyerang menangkap beberapa token sesi untuk melakukan analisis statistik, mencari pola, entropi rendah, atau kenaikan yang dapat diprediksi yang memungkinkan mereka memalsukan sesi yang valid bagi pengguna lain. Kelemahan dalam skema ini sering kali muncul sebagai kerentanan Session Fixation atau penggunaan nilai yang kurang acak, biasanya ditemukan dalam cookie HTTP, parameter URL, atau implementasi header khusus. Mengompromikan skema sesi adalah serangan berdampak tinggi yang memberikan akses penuh kepada lawan ke status autentikasi korban tanpa memerlukan kredensial utama mereka.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-SESS-01 |
| **CWE** | CWE-330 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/01-Testing_for_Session_Management_Schema  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Alat:** `Burp Suite (Sequencer)`, `OWASP ZAP`, `Python (pandas/numpy for entropy analysis)`, `Cookie Editor`

### Apakah pengidentifikasi sesi (session identifier) sudah cukup kompleks dan acak?
- [ ] Ya — token lolos uji keacakan statistik (entropi tinggi) dan bypass **tidak dimungkinkan**  
- [ ] Ya — token panjang tetapi berisi pola yang dapat diprediksi atau segmen statis  
- [ ] Tidak — token bersifat sekuensial, berdasarkan timestamp yang dapat diprediksi, atau memiliki entropi yang **sangat rendah** *(Kritis)*  

### Di mana pengidentifikasi sesi ditransmisikan dan disimpan?
- [ ] Pengidentifikasi disimpan dalam cookie `Secure` dan `HttpOnly` *(Paling aman)*  
- [ ] Pengidentifikasi disimpan dalam cookie tetapi tidak memiliki flag `Secure` atau `HttpOnly`  
- [ ] Pengidentifikasi ditransmisikan **hanya** melalui parameter URL atau permintaan `GET` *(Risiko Tinggi)*  

### Apakah aplikasi menerbitkan pengidentifikasi sesi baru setelah autentikasi berhasil?
- [ ] Ya — ID sesi baru yang unik **dibuat** segera setelah login  
- [ ] Tidak — ID sesi tetap sama sebelum dan sesudah login *(Memungkinkan terjadinya Session Fixation)*  

### Apakah pengidentifikasi sesi terikat pada alamat IP atau User-Agent tertentu untuk mencegah penggunaan kembali?
- [ ] Ya — sesi dibatalkan jika sidik jari (fingerprint) klien berubah secara signifikan  
- [ ] Tidak — sesi **dapat** digunakan kembali di berbagai alamat IP atau perangkat tanpa verifikasi tambahan  

### Apakah pengidentifikasi sesi dibatalkan dengan benar di sisi server setelah logout atau timeout?
- [ ] Ya — status sesi di sisi server **sepenuhnya dihancurkan** saat logout  
- [ ] Tidak — sesi tetap valid di server meskipun cookie di sisi klien dihapus  
- [ ] Tidak — sesi **tidak pernah kedaluwarsa** atau memiliki periode timeout yang sangat lama

---

## WSTG-SESS-02 — Pengujian Atribut Cookie

Manajemen sesi sering kali mengandalkan cookie HTTP untuk mempertahankan status (state), sehingga atribut keamanan dari cookie ini menjadi target utama bagi penyerang. Dengan mengabaikan atau salah mengonfigurasi atribut seperti `HttpOnly`, `Secure`, dan `SameSite`, aplikasi memaparkan token sesi terhadap pencurian melalui Cross-Site Scripting (XSS), intersepsi melalui saluran yang tidak terenkripsi, atau serangan Cross-Site Request Forgery (CSRF). Pentester memeriksa flag-flag ini untuk menentukan apakah penyerang dapat mengeksfiltrasi pengidentifikasi sesi dari Document Object Model (DOM) browser atau memaksa browser untuk mengirimkannya dalam permintaan lintas asal (cross-origin requests). Konfigurasi atribut yang tepat adalah langkah pertahanan berlapis (defense-in-depth) yang mendasar untuk memastikan bahwa token sensitif tetap terbatas pada konteks yang aman dan berwenang.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-SESS-02 |
| **CWE** | CWE-614 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Sedang |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/02-Testing_for_Cookies_Attributes  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Alat:** `Burp Suite (Proxy/Scanner)`, `OWASP ZAP`, `Browser Developer Tools (Storage Tab)`, `EditThisCookie`, `curl`

### Analisis Implementasi Flag Cookie
| Atribut | Ada | Tidak Ada |
|-----------|:-------:|:-------:|
| `HttpOnly` |  |  |
| `Secure` |  |  |
| `SameSite` |  |  |

### Apakah flag `HttpOnly` diterapkan pada cookie sesi yang sensitif?
- [ ] Ya — diterapkan pada **semua** cookie sesi sensitif *(Paling aman)*  
- [ ] Ya — hanya diterapkan pada beberapa cookie sesi  
- [ ] No — flag **tidak diterapkan**, memungkinkan akses melalui skrip sisi klien (client-side scripts) *(Kritis)*  

### Apakah flag `Secure` dipaksakan untuk cookie yang dikirimkan melalui HTTPS?
- [ ] Ya — diterapkan pada **semua** cookie yang dikirimkan melalui TLS  
- [ ] No — flag **tidak diterapkan**, memungkinkan cookie dikirimkan melalui HTTP teks biasa (plaintext)  

### Apakah atribut `SameSite` digunakan untuk memitigasi risiko CSRF?
- [ ] Ya — diatur ke `Strict` atau `Lax` untuk cookie sesi  
- [ ] Ya — diatur ke `None` tetapi dengan flag `Secure` yang ada  
- [ ] No — atribut **tidak ada** atau diatur ke `None` **tanpa** `Secure`  

### Apakah atribut `Domain` dan `Path` dibatasi pada cakupan minimum yang diperlukan?
- [ ] Ya — dicakupkan ke subdomain dan jalur aplikasi yang **spesifik**  
- [ ] No — cakupan terlalu **luas** (misalnya, diatur ke domain induk atau root `/`), meningkatkan permukaan serangan (attack surface)  

### Apakah cookie sesi yang sensitif kedaluwarsa dengan benar melalui atribut `Expires` atau `Max-Age`?
- [ ] Ya — tanggal kedaluwarsa yang sesuai telah ditetapkan  
- [ ] No — cookie bersifat persisten dengan masa pakai yang terlalu **lama**  
- [ ] No — cookie hanya untuk sesi tetapi **kurang** dalam penegakan batas waktu (timeout) di sisi server

---

## WSTG-SESS-03 — Session Fixation

Session fixation terjadi ketika aplikasi gagal membatalkan validitas atau merotasi identifier sesi (session identifier) setelah pengguna berhasil melakukan autentikasi, yang memungkinkan penyerang untuk memaksakan token sesi yang telah diketahui kepada korban. Jika aplikasi mempertahankan session ID pra-autentikasi setelah login, penyerang dapat memberikan tautan yang dibuat secara khusus berisi session ID tetap kepada korban dan selanjutnya membajak (hijack) sesi terautentikasi tersebut. Kerentanan ini biasanya muncul pada formulir login atau melalui identifier sesi yang diterima melalui parameter URL dan cookies. Dari sudut pandang penyerang, eksploitasi melibatkan tindakan "menetapkan" (fixing) sesi melalui tautan berbahaya atau kerentanan header injection dan menunggu korban memberikan kredensial, sehingga secara efektif melewati (bypassing) kebutuhan untuk mencuri token aktif.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-SESS-03 |
| **CWE** | CWE-384 |
| **Status Pengujian** | Belum Dilakukan |
| **Severity** | High |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/03-Testing_for_Session_Fixation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite (Proxy/Repeater)`, `OWASP ZAP`, `EditThisCookie`, `Curl`

### Apakah identifier sesi berubah setelah autentikasi berhasil?
- [ ] Ya — identifier sesi baru **diterbitkan** dan yang lama dibatalkan validitasnya *(Paling aman)*  
- [ ] Ya — identifier sesi baru **diterbitkan** tetapi yang lama **tetap valid**  
- [ ] Tidak — identifier sesi **tetap sama** sebelum dan sesudah autentikasi *(Kritis)*  

### Apakah aplikasi menerima identifier sesi yang diberikan melalui parameter URL?
- [ ] Tidak — session ID dikelola **secara eksklusif** melalui cookies  
- [ ] Ya — session ID diterima di URL tetapi **diabaikan** atau **dirotasi** saat login  
- [ ] Ya — session ID di URL **diterima** dan **dipertahankan** setelah autentikasi  

### Dapatkah session ID yang ditentukan penyerang dipaksakan ke dalam aplikasi?
- [ ] Tidak — aplikasi **menolak** session ID yang tidak valid atau tidak ada yang diberikan oleh pengguna  
- [ ] Ya — aplikasi **menerima** dan menginisialisasi sesi menggunakan ID sembarang yang diberikan dalam cookie  
- [ ] Ya — aplikasi **menerima** dan menginisialisasi sesi menggunakan ID sembarang yang diberikan dalam parameter URL  

### Apakah sesi pra-autentikasi dihentikan dengan benar?
- [ ] Ya — sesi anonim **dihancurkan sepenuhnya** saat peristiwa login terjadi  
- [ ] Tidak — sesi anonim **tidak dihancurkan**, memungkinkan potensi pemanenan sesi (session harvesting)  

### Apakah cookie sesi terlindungi dari injeksi sisi klien?
- [ ] Ya — flag `HttpOnly` dan `Secure` **diterapkan** untuk mencegah fixation berbasis skrip  
- [ ] Tidak — flag **tidak diterapkan**, memungkinkan session ID ditetapkan melalui Cross-Site Scripting (XSS)

---

## WSTG-SESS-04 — Testing for Exposed Session Variables

Variabel sesi yang terpapar terjadi ketika pengidentifikasi sesi yang sensitif atau data terkait status ditransmisikan melalui saluran yang tidak aman seperti URL query strings, Referer headers, atau log sistem. Paparan ini secara signifikan meningkatkan risiko Session Hijacking, karena pengidentifikasi dapat disimpan dalam cache riwayat peramban (browser history), dicatat oleh proksi perantara (intermediate proxies), atau bocor ke situs pihak ketiga melalui Referer header. Pentester mengidentifikasi paparan ini dengan memantau semua permintaan keluar (outbound requests) dan memeriksa log aplikasi atau konfigurasi server untuk kebocoran data yang tidak disengaja. Dalam skenario dunia nyata, penyerang mengeksploitasi kebocoran ini dengan mengumpulkan token sesi yang valid dari mesin bersama atau menganalisis pola lalu lintas untuk melewati otentikasi dan mendapatkan akses tidak sah ke akun pengguna.


| Kolom | Nilai |
|---|---|
| **WSTG ID** | WSTG-SESS-04 |
| **CWE** | CWE-598 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Sedang / Tinggi* |

> *Tingkat keparahan menjadi Tinggi jika variabel yang terpapar adalah token sesi yang memungkinkan pengambilalihan akun (account takeover) secara langsung.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/04-Testing_for_Exposed_Session_Variables  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Alat:** `Burp Suite (Proxy/Logger)`, `OWASP ZAP`, `Browser DevTools`, `grep`

### Apakah pengidentifikasi sesi ditransmisikan dalam URL query string?
- [ ] Tidak — pengidentifikasi sesi **tidak** ditemukan dalam URL query string  
- [ ] Ya — pengidentifikasi ditemukan tetapi sesi bersifat jangka pendek atau berisiko rendah  
- [ ] Ya — ID sesi unik **ditemukan** dalam URL query string *(Kritis)*  

### Apakah aplikasi membocorkan token sesi melalui Referer header?
- [ ] Tidak — `Referrer-Policy` mencegah kebocoran atau tidak ada tautan pihak ketiga  
- [ ] Ya — token sesi **ditransmisikan** ke domain pihak ketiga melalui Referer header  

### Apakah variabel sesi disimpan dalam log sisi server atau aplikasi?
- [ ] Tidak — variabel sesi disamarkan (masked) atau dikecualikan dari log  
- [ ] Ya — variabel sesi **tercatat** dalam log aplikasi atau server web dalam bentuk teks polos (plain text)  

### Apakah aplikasi menggunakan permintaan GET untuk operasi yang mengubah status (state-changing operations)?
- [ ] Tidak — aplikasi menggunakan `POST` atau metode non-idempotent lainnya untuk operasi sensitif  
- [ ] Ya — `GET` digunakan, menyebabkan data sesi tersimpan dalam cache riwayat peramban dan proksi perantara  

### Apakah header `Cache-Control` dikonfigurasi untuk mencegah caching data terkait sesi?
- [ ] Ya — `Cache-Control: no-store` (atau serupa) **diterapkan** pada respons sensitif  
- [ ] Ya — kontrol telah diterapkan tetapi bypass **memungkinkan** melalui kasus tepi (edge cases) peramban  
- [ ] Tidak — respons sensitif yang berisi data sesi **dapat** disimpan dalam cache oleh peramban

---

## WSTG-SESS-05 — Testing for Cross Site Request Forgery

Cross-Site Request Forgery (CSRF) adalah kerentanan di mana penyerang menipu browser korban untuk melakukan tindakan yang tidak diinginkan pada situs web lain di mana korban sedang terautentikasi. Exploit ini memanfaatkan perilaku browser yang secara otomatis menyertakan kredensial "ambient", seperti session cookies atau header Authorization, ke dalam permintaan keluar. Penyerang biasanya menargetkan operasi yang mengubah status (state-changing operations) seperti mengubah kata sandi, memperbarui alamat email, atau mengeksekusi transfer keuangan dengan menghosting skrip berbahaya atau formulir tersembunyi di situs pihak ketiga. Eksploitasi yang berhasil dapat menyebabkan pengambilalihan akun secara penuh atau modifikasi data tanpa izin tanpa sepengetahuan atau persetujuan pengguna.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-SESS-05 |
| **CWE** | CWE-352 |
| **Status Pengujian** | Belum Dilakukan |
| **Severity** | Menengah / Tinggi* |

> *Severity menjadi Tinggi jika tindakan yang rentan memungkinkan pengambilalihan akun, eskalasi hak istimewa (privilege escalation), atau transaksi keuangan tanpa izin.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/05-Testing_for_Cross_Site_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/csrf-cross-site-request-forgery.html  
* https://portswigger.net/web-security/csrf  

**Tools:** `Burp Suite Professional (CSRF PoC Generator)`, `OWASP ZAP`, `CSRFTester`, `python3 (SimpleHTTPServer for PoC hosting)`

### Apakah token anti-CSRF diimplementasikan untuk permintaan yang mengubah status (state-changing requests)?
- [ ] Ya — token yang unik dan kuat secara kriptografis diperlukan untuk semua tindakan yang mengubah status  
- [ ] Ya — token tersedia tetapi **tidak** unik per sesi atau dapat diprediksi  
- [ ] Tidak — token anti-CSRF **tidak** diimplementasikan  

### Apakah validasi sisi server (server-side validation) dari token anti-CSRF sudah kuat?
- [ ] Ya — server memvalidasi token secara ketat dan bypass **tidak dimungkinkan**  
- [ ] Ya — validasi dilakukan tetapi **dapat** dilewati dengan menghapus parameter token  
- [ ] Ya — validasi dilakukan tetapi **dapat** dilewati dengan memberikan token kosong atau dummy  
- [ ] Ya — validasi dilakukan tetapi token **tidak** terikat dengan sesi pengguna  

### Apakah aplikasi mengandalkan metode yang mudah dilewati (bypassable) untuk perlindungan CSRF?
- [ ] Tidak — aplikasi tidak mengandalkan header yang lemah atau pemeriksaan origin saja  
- [ ] Ya — perlindungan hanya mengandalkan header `Referer` atau `Origin` yang **dapat** dipalsukan (spoofed) atau dihapus  
- [ ] Ya — perlindungan mengandalkan pemeriksaan header `X-Requested-With` yang **dapat** dilewati melalui kesalahan konfigurasi API atau CORS  

### Apakah session cookies dikonfigurasi dengan atribut `SameSite`?
- [ ] Ya — `SameSite` disetel ke `Strict` atau `Lax` pada semua cookie yang terkait dengan sesi  
- [ ] Tidak — atribut `SameSite` **hilang**, sehingga menggunakan perilaku bawaan masing-masing browser  
- [ ] Tidak — `SameSite` secara eksplisit disetel ke `None` tanpa flag `Secure` atau mitigasi tambahan  

### Apakah perlindungan CSRF dapat dilewati dengan mengubah metode permintaan HTTP?
- [ ] Tidak — perlindungan diterapkan tanpa memandang metode HTTP yang digunakan  
- [ ] Ya — token hanya divalidasi pada permintaan `POST`, tetapi tindakan tersebut **dapat** dilakukan melalui `GET`  
- [ ] Ya — mengubah metode (misalnya, menjadi `PUT` atau `DELETE`) dapat melewati pemeriksaan token

---

## WSTG-SESS-06 — Pengujian Fungsionalitas Logout

Fungsionalitas logout adalah kontrol keamanan kritis yang dirancang untuk mengakhiri sesi pengguna dan membatalkan pengenal sesi (session identifiers) yang terkait baik di sisi klien maupun server. Kegagalan dalam mengimplementasikan logout dengan benar memungkinkan serangan Session Fixation atau hijacking, karena penyerang yang mendapatkan akses ke mesin setelah pengguna melakukan "logout" berpotensi menggunakan kembali token sesi yang masih aktif. Pentester mengevaluasi hal ini dengan memeriksa apakah cookie sesi dihapus dari browser, apakah status sesi di sisi server dihancurkan secara eksplisit, dan apakah navigasi tombol kembali (back-button) memungkinkan akses ke data sensitif yang tersimpan dalam cache. Implementasi logout yang aman memastikan bahwa setelah tindakan tersebut dipicu, tidak ada permintaan berikutnya yang menggunakan token sesi lama yang diizinkan oleh aplikasi.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-SESS-06 |
| **CWE** | CWE-613 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Sedang |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/06-Testing_for_Logout_Functionality  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Alat:** `Burp Suite (Repeater/Proxy)`, `Browser Developer Tools`, `OWASP ZAP`

### Apakah mekanisme logout yang fungsional tersedia dan dapat diakses?
- [ ] Ya — tombol logout tersedia dan memicu permintaan pengakhiran (termination request)  
- [ ] Tidak — tombol logout **tidak ada** atau **tidak** memicu kejadian pengakhiran  

### Apakah pengenal sesi (session identifier) dibatalkan validitasnya di sisi server?
- [ ] Ya — server menolak semua permintaan berikutnya yang menggunakan token sesi lama  
- [ ] Tidak — server **terus menerima** token sesi lama setelah logout  

### Apakah aplikasi menghapus cookie sesi di browser saat logout?
- [ ] Ya — cookie ditimpa dengan tanggal kedaluwarsa atau nilai kosong  
- [ ] Ya — cookie tetap ada tetapi **tidak lagi valid** di server  
- [ ] Tidak — cookie tetap ada di browser dan **mempertahankan** nilai aslinya  

### Dapatkah konten terautentikasi yang sensitif diakses melalui tombol 'Kembali' (Back) browser setelah logout?
- [ ] Tidak — header `Cache-Control: no-store` atau serupa mencegah penampilan halaman sensitif setelah logout  
- [ ] Ya — halaman sensitif tersimpan dalam cache dan **dapat** dilihat melalui navigasi setelah sesi diakhiri

---

## WSTG-SESS-07 — Menguji Batas Waktu Sesi (Session Timeout)

Pengujian batas waktu sesi (session timeout) mengevaluasi apakah sebuah aplikasi secara efektif mengakhiri sesi pengguna setelah periode ketidakaktifan yang ditentukan sebelumnya atau durasi total. Kontrol ini sangat penting untuk memitigasi risiko pembajakan sesi (session hijacking), terutama pada workstation bersama atau dalam skenario di mana penyerang menangkap pengidentifikasi sesi (session identifier) melalui intersepsi jaringan atau XSS. Pentester menganalisis baik idle timeout, yang dipicu setelah periode ketidakaktifan, maupun absolute timeout, yang membatasi total masa pakai sesi terlepas dari aktivitasnya. Dari sudut pandang penyerang, sesi yang tidak terbatas atau terlalu lama memberikan jendela peluang yang diperpanjang untuk mempertahankan akses yang tidak sah dan melewati kebutuhan untuk autentikasi ulang (re-authentication).


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-SESS-07 |
| **CWE** | CWE-613 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Sedang |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/07-Testing_Session_Timeout  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Browser DevTools`

### Apakah aplikasi menerapkan idle session timeout?
- [ ] Ya — sesi kedaluwarsa setelah periode ketidakaktifan yang singkat (misalnya, 15-30 menit)  
- [ ] Ya — sesi kedaluwarsa tetapi durasi timeout **terlalu lama** (misalnya, > 24 jam)  
- [ ] Tidak — sesi **tetap aktif** tanpa batas waktu selama ketidakaktifan  

### Apakah batas waktu sesi (session timeout) diterapkan di sisi server (server-side)?
- [ ] Ya — server menolak permintaan dengan token yang kedaluwarsa terlepas dari status di sisi klien (client-side)  
- [ ] Tidak — timeout **hanya** diterapkan melalui JavaScript sisi klien atau meta-refresh  

### Apakah aplikasi menerapkan absolute session timeout?
- [ ] Ya — sesi dihentikan setelah durasi maksimum yang tetap terlepas dari aktivitas  
- [ ] Tidak — sesi **dapat** diperpanjang tanpa batas waktu selama ada aktivitas terus-menerus  

### Apakah pengidentifikasi sesi (session identifier) dibatalkan validitasnya di server saat timeout?
- [ ] Ya — token sesi **tidak dapat** digunakan kembali setelah ambang batas timeout tercapai  
- [ ] Tidak — token sesi **masih valid** di server bahkan setelah klien dialihkan ke halaman login  

### Bisakah sesi tetap aktif tanpa batas waktu menggunakan permintaan heartbeat otomatis?
- [ ] No — absolute timeout atau kontrol sekunder **mencegah** perpanjangan sesi yang tidak terbatas  
- [ ] Ya — permintaan latar belakang berkala (misalnya, AJAX) **dapat** mempertahankan sesi tanpa batas waktu

---

## WSTG-SESS-08 — Session Puzzling

Session Puzzling, yang juga dikenal sebagai Session Variable Overloading, terjadi ketika aplikasi menggunakan variabel sesi yang sama untuk berbagai tujuan di modul yang berbeda atau gagal menghapus status sesi dengan benar selama transisi alur kerja (workflow). Penyerang mengeksploitasi perilaku ini dengan mengisi variabel sesi dalam satu konteks—seperti pratinjau profil yang belum terautentikasi atau pendaftaran multi-langkah—lalu menavigasi ke area yang dilindungi di mana aplikasi secara keliru memercayai variabel yang sudah ada tersebut. Hal ini dapat menyebabkan dampak kritis termasuk Authentication Bypass, Privilege Escalation, atau akses tidak sah ke data sensitif dengan menipu aplikasi agar meyakini bahwa sesi berada dalam status hak akses yang lebih tinggi daripada yang sebenarnya. Kerentanan ini paling sering ditemukan pada aplikasi stateful yang kompleks di mana logika Manajemen Sesi (Session Management) diterapkan secara tidak konsisten di berbagai komponen fungsional.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-SESS-08 |
| **CWE** | CWE-621 |
| **Status Pengujian** | Belum Dilakukan |
| **Severitas** | Tinggi / Kritis |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/08-Testing_for_Session_Puzzling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite (Repeater/Comparator)`, `OWASP ZAP`, `Postman`, `Cookie Editor`

### Apakah aplikasi menggunakan nama variabel sesi yang sama untuk tujuan berbeda di modul yang terpisah?
- [ ] Tidak — nama variabel unik atau cakupannya terisolasi  
- [ ] Ya — terdapat nama variabel yang sama tetapi status dihapus di antara modul  
- [ ] Ya — terdapat nama variabel yang sama dan status **tidak** dihapus  

### Dapatkah tindakan tanpa autentikasi memengaruhi variabel sesi yang digunakan dalam konteks terautentikasi?
- [ ] Tidak — variabel sesi diinisialisasi hanya **setelah** autentikasi berhasil  
- [ ] Ya — variabel sesi dapat diatur sebelum login tetapi **tidak** memengaruhi status pasca-login  
- [ ] Ya — variabel sesi yang diatur selama pra-autentikasi **dipercayai** pasca-autentikasi *(Kritis)*  

### Apakah aplikasi melakukan inisialisasi ulang pengenal sesi (Session ID) dan variabel terkait saat terjadi perubahan tingkat hak akses?
- [ ] Ya — sesi direset sepenuhnya dan ID baru **diterbitkan**  
- [ ] Parsial — ID baru diterbitkan tetapi beberapa variabel sesi **tetap ada**  
- [ ] Tidak — ID sesi dan semua variabel **tetap** tidak berubah  

### Dapatkah hak administratif diperoleh dengan memanipulasi variabel sesi melalui akun tanpa hak khusus (unprivileged account)?
- [ ] Tidak — variabel hak akses **tidak dapat** dimodifikasi oleh pengguna  
- [ ] Ya — variabel dapat dimodifikasi tetapi aplikasi **memvalidasinya** terhadap database backend  
- [ ] Ya — variabel dapat dimodifikasi dan aplikasi **memercayai** status sesi secara implisit *(Kritis)*  

### Apakah status sesi segera dihapus setelah menyelesaikan alur kerja tertentu (misalnya, reset kata sandi, checkout)?
- [ ] Ya — variabel sesi untuk alur kerja dihancurkan setelah selesai  
- [ ] Tidak — variabel alur kerja **tetap ada** dan dapat digunakan kembali di area aplikasi lainnya

---

## WSTG-SESS-09 — Menguji Pengambilalihan Sesi (Session Hijacking)

Pengambilalihan sesi (Session Hijacking) terjadi ketika penyerang menangkap, memprediksi, atau menetapkan pengidentifikasi sesi (SID) yang valid untuk mendapatkan akses tidak sah ke sesi aktif pengguna. Kerentanan ini biasanya muncul karena keamanan lapisan transport yang tidak memadai, kurangnya flag keamanan cookie, atau algoritma pembuatan SID yang dapat diprediksi yang memungkinkan penyerang untuk melewati autentikasi sepenuhnya. Dari perspektif penyerang, eksploitasi yang berhasil memberikan kemampuan penyamaran penuh terhadap korban tanpa memerlukan kredensial mereka, yang sering kali dicapai melalui sniffing jaringan, Cross-Site Scripting (XSS), atau serangan fiksasi sesi (Session Fixation).


| Bidang | Nilai |
|---|---|
| **ID WSTG** | WSTG-SESS-09 |
| **CWE** | CWE-287 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Tinggi / Kritis |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/09-Testing_for_Session_Hijacking  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Alat:** `Burp Suite`, `OWASP ZAP`, `EditThisCookie`, `Wireshark`, `Bettercap`

### Apakah pengidentifikasi sesi terlindungi selama transit?
- [ ] Ya — flag `Secure` **diaktifkan** dan HSTS diterapkan secara ketat  
- [ ] Ya — flag `Secure` **diaktifkan** tetapi HSTS **dinonaktifkan**  
- [ ] Tidak — flag `Secure` **tidak diterapkan**, memungkinkan intersepsi SID melalui saluran yang tidak terenkripsi *(Tinggi)*  

### Apakah pengidentifikasi sesi terlindungi dari akses skrip sisi klien?
- [ ] Ya — flag `HttpOnly` **diterapkan** pada semua cookie yang terkait dengan sesi  
- [ ] Tidak — flag `HttpOnly` **tidak diterapkan**, memungkinkan eksfiltrasi SID melalui XSS *(Kritis)*  

### Apakah aplikasi menerapkan pengikatan sesi (session binding) ke atribut klien?
- [ ] Ya — sesi terikat ke atribut klien (IP/User-Agent) dan penggunaan kembali **tidak dimungkinkan**  
- [ ] Ya — pengikatan sesi ada tetapi bypass **dimungkinkan** melalui header spoofing  
- [ ] Tidak — tidak ada pengikatan sesi; SID **tetap valid** saat digunakan dari sumber mana pun  

### Apakah pengidentifikasi sesi cukup acak untuk mencegah prediksi?
- [ ] Ya — SID menggunakan generator angka pseudo-acak yang aman secara kriptografis (CSPRNG)  
- [ ] Tidak — panjang SID tidak memadai atau mengikuti urutan/pola yang **dapat diprediksi**  

### Apakah sesi bersamaan (concurrent sessions) dikelola untuk membatasi jendela serangan?
- [ ] Tidak — aplikasi secara ketat memberlakukan sesi aktif tunggal per pengguna  
- [ ] Ya — beberapa sesi bersamaan **diaktifkan** tetapi dipantau  
- [ ] Ya — sesi bersamaan yang tidak terbatas **dimungkinkan** di berbagai perangkat/lokasi

---

## WSTG-SESS-10 — Pengujian JSON Web Tokens

JSON Web Tokens (JWT) umumnya digunakan untuk manajemen sesi stateless dan propagasi identitas, namun keamanannya bergantung sepenuhnya pada implementasi algoritme penandatanganan (signing algorithms) yang tepat dan verifikasi klaim (claim) yang ketat. Penyerang sering kali mencoba melewati autentikasi dengan mengeksploitasi celah algoritme "none", melakukan Brute Force pada kunci rahasia HS256 yang lemah, atau melakukan serangan perpindahan algoritme (algorithm-switching) di mana kunci publik asimetris digunakan sebagai rahasia HMAC simetris. Kerentanan ini biasanya muncul pada cookie sesi atau header Authorization, yang berpotensi memungkinkan penyerang untuk melakukan eskalasi hak akses (privilege escalation) atau menyamar sebagai pengguna mana pun dengan memodifikasi klaim seperti `role` atau `user_id` tanpa merusak integritas kriptografi token tersebut.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-SESS-10 |
| **CWE** | CWE-347 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Tinggi / Kritis |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/10-Testing_JSON_Web_Tokens  
* https://hacktricks.wiki/en/pentesting-web/hacking-jwt-json-web-tokens.html  
* https://portswigger.net/web-security/jwt  

**Alat:** `jwt_tool`, `Burp Suite (JWT Editor Extension)`, `hashcat`, `john`, `jose`

### Apakah algoritme "none" diterima oleh server?
- [ ] Tidak — server **menolak** token yang menggunakan `alg: none` pada header  
- [ ] Ya — server **menerima** token dengan `alg: none` dan tanda tangan kosong *(Kritis)*  

### Bagaimana tanda tangan JWT diverifikasi dan dilindungi terhadap brute-force?
- [ ] Ya — kunci asimetris yang kuat (RS256/ES256) atau kunci rahasia HMAC dengan entropi tinggi digunakan  
- [ ] Ya — HS256 digunakan tetapi kunci rahasia **dapat** di-brute-force karena entropi yang rendah atau penggunaan nilai default  
- [ ] Tidak — verifikasi tanda tangan **tidak diterapkan** atau **dapat** dilewati melalui perpindahan algoritme/algorithm switching (dari RS256 ke HS256)  

### Apakah klaim standar (exp, nbf, iat) ditegakkan secara ketat oleh backend?
- [ ] Ya — `exp` (expiration) tersedia dan **tidak dapat** dilewati  
- [ ] Ya — `exp` tersedia tetapi server **tidak** menegakkannya selama proses validasi  
- [ ] Tidak — klaim kedaluwarsa **hilang** atau diabaikan, memungkinkan Session Replay tanpa batas waktu  

### Apakah implementasi mencegah injeksi kunci berbasis header (kid, jku, x5u)?
- [ ] Ya — header disanitasi dan hanya kunci/sumber internal tepercaya yang **diizinkan**  
- [ ] Tidak — header `kid` (Key ID) rentan terhadap Directory Traversal atau SQL Injection untuk merujuk ke berkas yang diketahui  
- [ ] Tidak — header `jku` atau `x5u` **dapat** diarahkan ke URL yang dikendalikan penyerang untuk memuat kunci berbahaya (malicious keys)

---

## WSTG-SESS-11 — Testing for Concurrent Sessions

Pengujian sesi konkuren (Concurrent Session) mengevaluasi apakah sebuah aplikasi mengizinkan satu akun pengguna untuk mempertahankan beberapa sesi aktif secara bersamaan di berbagai browser, perangkat, atau alamat IP. Kegagalan dalam membatasi sesi konkuren meningkatkan jendela peluang bagi penyerang untuk menggunakan token sesi yang dibajak atau kredensial yang disusupi tanpa memperingatkan pengguna yang sah atau menghentikan sesi yang ada. Perilaku ini biasanya dinilai dengan melakukan autentikasi dari berbagai sumber dan memantau stabilitas sesi, yang sering kali mengungkapkan kelemahan dalam logika manajemen sesi atau kurangnya aturan bisnis yang berfokus pada keamanan. Dari perspektif penyerang, kurangnya kontrol konkurensi memungkinkan persistensi tersembunyi setelah kompromi awal dan memfasilitasi serangan otomatis paralel.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-SESS-11 |
| **CWE** | CWE-613 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan (Severity)** | Low / Medium |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/11-Testing_for_Concurrent_Sessions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite (Repeater/Containers)`, `Firefox Multi-Account Containers`, `Postman`, `cURL`

### Apakah sesi konkuren dibatasi oleh kebijakan aplikasi?
- [ ] Ya — hanya satu sesi **diizinkan** per pengguna; login baru membatalkan sesi lama *(Paling aman)*  
- [ ] Ya — jumlah sesi konkuren yang tetap **diterapkan** dan tidak dapat dilampaui  
- [ ] Tidak — jumlah sesi konkuren yang tidak terbatas **mungkin dilakukan**  

### Bagaimana aplikasi merespons saat batas sesi tercapai?
- [ ] Sesi aktif tertua **dihentikan secara otomatis** (mitigasi session fixation/hijacking)  
- [ ] Pengguna **diminta** untuk menghentikan sesi yang ada sebelum sesi baru diizinkan  
- [ ] Upaya login baru **diblokir** hingga logout manual dilakukan dari perangkat lain  
- [ ] Tidak ada tindakan yang diambil — beberapa sesi tetap **aktif** secara bersamaan  

### Apakah aplikasi menyediakan antarmuka manajemen sesi untuk pengguna?
- [ ] Ya — pengguna **dapat** melihat sesi aktif (IP, perangkat, waktu) dan menghentikannya dari jarak jauh (Remote Terminate)  
- [ ] Ya — pengguna **dapat** melihat sesi aktif tetapi **tidak dapat** menghentikannya dari jarak jauh  
- [ ] Tidak — pengguna **tidak dapat** melihat atau mengelola sesi aktif lainnya yang terkait dengan akun mereka  

### Apakah pengguna diberi tahu saat aktivitas login konkuren terdeteksi?
- [ ] Ya — peringatan (email, SMS, atau dalam aplikasi) **dipicu** untuk login dari perangkat atau lokasi baru  
- [ ] Tidak — tidak ada notifikasi yang **diberikan** saat sesi konkuren dibuat

---

## WSTG-INPV-01 — Reflected Cross Site Scripting (XSS)

Reflected Cross Site Scripting (XSS) terjadi ketika sebuah aplikasi menyertakan data yang tidak terpercaya dalam respons HTTP tanpa validasi atau encoding yang memadai, sehingga menyebabkan payload dieksekusi dalam konteks browser korban. Penyerang mengirimkan payload berbahaya melalui social engineering, biasanya melalui URL yang dimanipulasi atau formulir, untuk mengambil alih sesi pengguna, mengeksfiltrasi cookie sensitif, atau melakukan tindakan tidak sah atas nama pengguna. Kerentanan ini umumnya ditemukan pada parameter pencarian, pesan kesalahan, dan endpoint mana pun yang merefleksikan input secara langsung kembali ke antarmuka pengguna. Eksploitasi bergantung pada interaksi korban dengan tautan berbahaya, menjadikannya vektor serangan non-persisten namun sangat efektif untuk menargetkan pengguna administratif atau pengguna terautentikasi tertentu.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INPV-01 |
| **CWE** | CWE-79 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Tinggi |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/01-Testing_for_Reflected_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/cross-site-scripting  

**Alat:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `XSStrike`, `KXS`, `dalfox`

### Apakah input yang diberikan pengguna direfleksikan dalam body respons?
- [ ] Tidak — input **tidak pernah** direfleksikan kembali ke pengguna  
- [ ] Ya — input direfleksikan tetapi di-encode dengan benar dan bypass **tidak dimungkinkan**  
- [ ] Ya — input direfleksikan **tanpa** encoding atau sanitasi apa pun  

### Apakah output encoding berbasis konteks diimplementasikan?
- [ ] Ya — encoding yang tepat (HTML, Atribut, JavaScript) **diterapkan** berdasarkan konteks refleksi yang spesifik  
- [ ] Ya — encoding **diterapkan** tetapi tidak memadai untuk konteks tertentu (misalnya, HTML encoding di dalam tag `<script>`)  
- [ ] Tidak — encoding **tidak diterapkan**  

### Dapatkah validasi input atau filter Web Application Firewall (WAF) di-bypass?
- [ ] Tidak — filter secara efektif memblokir semua payload XSS yang umum dan ter-obfuscated  
- [ ] Ya — filter tersedia tetapi bypass **dimungkinkan** menggunakan character encoding, tag non-standar, atau polyglot  
- [ ] Tidak — tidak ada filter atau mekanisme validasi yang diterapkan  

### Apa konteks eksekusi dari input yang direfleksikan?
- [ ] Aman — input direfleksikan di lokasi non-executable (misalnya, di dalam tag `<div>` atau `<span>` standar)  
- [ ] Berisiko — input direfleksikan di dalam atribut HTML atau di dalam `src`/`href` dari tag  
- [ ] Kritis — input direfleksikan langsung di dalam blok `<script>`, event handler, atau template literal  

### Apakah header keamanan browser modern tersedia untuk memitigasi dampak XSS?
- [ ] Ya — `Content-Security-Policy` (CSP) **diaktifkan** dengan script-src yang restriktif  
- [ ] Ya — `Content-Security-Policy` (CSP) **diaktifkan** tetapi mengandung `unsafe-inline` atau daftar putih (whitelist) yang lemah  
- [ ] Tidak — tidak ada header CSP atau header legacy `X-XSS-Protection` yang tersedia

---

## WSTG-INPV-02 — Stored Cross Site Scripting (XSS)

Stored Cross Site Scripting (XSS), juga dikenal sebagai Persistent XSS, terjadi ketika sebuah aplikasi menerima data dari pengguna dan menyimpannya dalam basis data (database), sistem berkas (file system), atau media penyimpanan lainnya secara persisten tanpa validasi atau pengkodean (encoding) yang memadai. Ketika korban kemudian menavigasi ke halaman yang mengambil dan menampilkan data yang belum dibersihkan (unsanitized) ini, skrip berbahaya akan dieksekusi di dalam konteks browser mereka di bawah asal (origin) aplikasi tersebut. Kerentanan ini sangat berbahaya karena tidak memerlukan korban untuk mengeklik tautan yang dirancang khusus; setiap pengguna yang melihat halaman yang terpengaruh menjadi target, yang berpotensi menyebabkan pembajakan sesi (session hijacking), tindakan tidak sah, atau pengambilan kredensial (credential harvesting). Penyerang biasanya menargetkan bagian komentar, profil pengguna, papan pesan, dan log administratif di mana input dirender secara konsisten untuk pengguna lain atau administrator.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INPV-02 |
| **CWE** | CWE-79 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/02-Testing_for_Stored_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/cross-site-scripting  

**Alat:** `Burp Suite`, `XSStrike`, `BeEF`, `KXSStrike`, `Sleepy Puppy`

### Apakah terdapat titik masuk (entry points) yang menyimpan input pengguna untuk ditampilkan kemudian kepada pengguna lain?
- [ ] Tidak — aplikasi tidak menyimpan input yang dapat dikontrol pengguna untuk perenderan di kemudian hari  
- [ ] Ya — input disimpan (misalnya, profil, komentar) tetapi **tidak** dirender dalam konteks HTML  
- [ ] Ya — input disimpan dan dirender kepada pengguna lain atau administrator  

### Apakah pengkodean output (output encoding) atau sanitasi diterapkan saat data yang disimpan dirender?
- [ ] Ya — pengkodean output yang sadar konteks (context-aware output encoding) **diterapkan** dan bypass **tidak dimungkinkan** *(Paling aman)*  
- [ ] Ya — sanitasi (misalnya, `DOMPurify`) **diterapkan** tetapi bypass **dimungkinkan** melalui kesalahan konfigurasi  
- [ ] No — data dirender secara langsung ke dalam DOM **tanpa** pengkodean atau sanitasi *(Tinggi)*  

### Bisakah validasi input atau sanitasi di-bypass menggunakan payload alternatif?
- [ ] Tidak — filter menangani berbagai pengkodean, tag tidak standar, dan event handler secara aman  
- [ ] Ya — bypass **dimungkinkan** menggunakan pengkodean entitas HTML atau tag tertentu (misalnya, `<svg>`, `<img>`)  
- [ ] Ya — bypass **dimungkinkan** melalui payload polyglot atau mutation-based XSS (mXSS)  

### Apakah Content Security Policy (CSP) menyediakan lapisan pertahanan sekunder?
- [ ] Ya — CSP yang ketat **diaktifkan** dan mencegah eksekusi skrip inline dan sumber yang tidak sah  
- [ ] Ya — CSP **diaktifkan** tetapi salah dikonfigurasi (misalnya, `unsafe-inline` atau `script-src` yang luas)  
- [ ] Tidak — tidak ada CSP yang **diaktifkan** untuk aplikasi tersebut  

### Apakah mungkin untuk mencapai "Stored XSS to RCE" atau rantai serangan berdampak tinggi lainnya (Blind XSS)?
- [ ] Tidak — dampak terbatas pada konteks browser sisi klien (client-side)  
- [ ] Ya — stored XSS **dapat** digunakan untuk menargetkan administrator (Blind XSS) di panel internal  
- [ ] Ya — stored XSS **dapat** dirangkai dengan kerentanan lain (misalnya, CSRF, eksfiltrasi data sensitif)

---

## WSTG-INPV-03 — Pengujian terhadap HTTP Verb Tampering

HTTP Verb Tampering mengeksploitasi kelemahan pada bagaimana server web dan framework aplikasi mengotorisasi akses ke sumber daya tertentu berdasarkan metode HTTP yang digunakan. Penyerang mencoba melewati batasan keamanan (security constraints) dengan mengganti metode standar seperti `GET` atau `POST` dengan alternatif seperti `HEAD`, `PUT`, `OPTIONS`, atau bahkan string arbitrer non-standar yang mungkin diproses secara tidak konsisten oleh backend. Kerentanan ini biasanya terjadi ketika konfigurasi keamanan—seperti filter Java EE web.xml atau aturan otorisasi .NET—mencantumkan metode yang diizinkan secara eksplisit tetapi gagal mempertimbangkan metode lainnya, atau ketika pendekatan "deny-by-method" digunakan alih-alih "deny-all." Eksploitasi yang berhasil dapat menghasilkan akses tidak sah ke fungsi administratif, modifikasi data, atau pengungkapan informasi mengenai konfigurasi internal server.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INPV-03 |
| **CWE** | CWE-288 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Sedang / Tinggi* |

> *Keparahan menjadi Tinggi jika fungsi administratif atau modifikasi data (PUT/DELETE) dapat dilakukan tanpa autentikasi.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/03-Testing_for_HTTP_Verb_Tampering  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite (Repeater/Intruder)`, `curl`, `nmap`, `ffuf`

### Apakah aplikasi merespons metode HTTP non-standar atau arbitrer?
- [ ] Tidak — aplikasi menolak semua metode kecuali yang secara eksplisit diperlukan  
- [ ] Ya — aplikasi merespons metode standar (OPTIONS, TRACE) tetapi **tidak dapat** melewati keamanan  
- [ ] Ya — aplikasi menerima string arbitrer (misalnya, FOO, TEST) dan memprosesnya sebagai permintaan `GET` atau `POST`  

### Apakah autentikasi/otorisasi dapat dilewati dengan mengubah metode HTTP?
- [ ] Tidak — kontrol keamanan diterapkan secara global tanpa memandang HTTP verb  
- [ ] Ya — kontrol sudah ada tetapi bypass **mungkin dilakukan** menggunakan metode `HEAD`  
- [ ] Ya — kontrol keamanan **tidak diterapkan** pada metode alternatif, memungkinkan akses tidak sah ke endpoint yang dibatasi  

### Apakah metode berbahaya seperti `PUT` atau `DELETE` diaktifkan pada server?
- [ ] Tidak — metode berbahaya **dinonaktifkan** atau mengembalikan 405 Method Not Allowed  
- [ ] Ya — metode diaktifkan tetapi memerlukan autentikasi tingkat tinggi yang valid  
- [ ] Ya — metode **diaktifkan** dan memungkinkan unggahan file atau penghapusan sumber daya yang tidak sah *(Kritis)*  

### Apakah metode `OPTIONS` mengungkapkan informasi sensitif tentang verb yang diizinkan?
- [ ] Tidak — `OPTIONS` **dinonaktifkan** atau mengembalikan respons generik  
- [ ] Ya — `OPTIONS` mengembalikan header `Allow` tetapi tidak mengekspos metode internal yang dibatasi  
- [ ] Ya — `OPTIONS` mengungkapkan metode internal saja atau administratif yang membantu dalam eksploitasi lebih lanjut  

### Apakah metode `TRACE` diaktifkan, berpotensi memungkinkan Cross-Site Tracing (XST)?
- [ ] Tidak — metode `TRACE` dan `TRACK` **dinonaktifkan**  
- [ ] Ya — `TRACE` **diaktifkan** dan memantulkan kembali header HTTP, termasuk cookie/token sensitif

---

## WSTG-INPV-04 — Testing for HTTP Parameter Pollution

HTTP Parameter Pollution (HPP) terjadi ketika sebuah aplikasi menerima beberapa parameter HTTP dengan nama yang sama dan memprosesnya dengan cara yang tidak konsisten atau tidak aman. Dengan menyediakan parameter duplikat, penyerang dapat memanipulasi logika internal aplikasi, yang berpotensi melewati Web Application Firewalls (WAF), filter validasi input, atau mekanisme kontrol akses. Kerentanan ini sangat bergantung pada server web atau kerangka kerja aplikasi yang mendasarinya, yang mungkin memilih parameter pertama, terakhir, atau versi gabungan (concatenated) dari parameter yang diulang. Eksploitasi biasanya menargetkan endpoint yang meneruskan parameter ke API back-end, kueri basis data, atau mekanisme pengalihan URL, yang menyebabkan dampak mulai dari Cross-Site Scripting (XSS) hingga eksfiltrasi data yang tidak sah.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INPV-04 |
| **CWE** | CWE-235 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Sedang |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/04-Testing_for_HTTP_Parameter_Pollution  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite (Repeater/Intruder)`, `Arjun`, `HPP Finder`, `Wfuzz`

### Bagaimana aplikasi menangani beberapa parameter HTTP dengan nama yang sama?
- [ ] Tidak — parameter duplikat ditolak atau diabaikan oleh server  
- [ ] Ya — server secara konsisten hanya memilih kemunculan **pertama** atau **terakhir**  
- [ ] Ya — server **menggabungkan** (concatenate) nilai-nilai tersebut, yang berpotensi memungkinkan terjadinya injeksi  

### Dapatkah HPP dimanfaatkan untuk melewati kontrol keamanan seperti WAF atau filter input?
- [ ] Tidak — kontrol keamanan memeriksa **semua** kemunculan parameter dengan benar  
- [ ] Ya — WAF atau filter hanya memeriksa kemunculan **pertama**, sehingga memungkinkan kemunculan **kedua** yang berbahaya untuk lolos  
- [ ] Ya — penggabungan memungkinkan penyerang untuk **membagi** Payload ke beberapa parameter untuk menghindari deteksi  

### Apakah aplikasi merefleksikan parameter yang tercemar dalam respons tanpa sanitasi yang tepat?
- [ ] Tidak — parameter disanitasi atau dikodekan sebelum direfleksikan dalam UI  
- [ ] Ya — pencemaran menghasilkan konten yang direfleksikan, tetapi sanitasi **telah diterapkan**  
- [ ] Ya — pencemaran menghasilkan konten yang direfleksikan dan sanitasi **tidak diterapkan**, yang menyebabkan XSS atau kesalahan logika  

### Apakah parameter yang diteruskan ke panggilan API internal atau sistem back-end rentan terhadap pencemaran?
- [ ] Tidak — permintaan back-end menggunakan metode konstruksi yang aman dan non-dinamis  
- [ ] Ya — HPP memungkinkan penyerang untuk **menyuntikkan** (inject) atau **menimpa** (overwrite) parameter dalam struktur permintaan back-end  

### Apakah aplikasi menunjukkan perilaku yang berbeda ketika parameter dicemari dalam permintaan GET vs POST?
- [ ] Tidak — perilaku konsisten di semua metode HTTP  
- [ ] Ya — hanya parameter GET yang rentan terhadap pencemaran  
- [ ] Ya — hanya parameter POST (body) yang rentan terhadap pencemaran

---

## WSTG-INPV-05 — SQL Injection

SQL Injection terjadi ketika input yang diberikan pengguna dimasukkan ke dalam query SQL tanpa parameterisasi atau sanitisasi yang tepat, sehingga memungkinkan penyerang untuk memanipulasi logika query. Eksploitasi yang berhasil dapat menyebabkan eksfiltrasi data yang tidak sah, bypass autentikasi, modifikasi data, dan dalam beberapa kasus, Remote Code Execution (RCE) melalui fitur basis data seperti `xp_cmdshell` atau `LOAD_FILE()`. Kerentanan ini umumnya memengaruhi formulir login, fungsi pencarian, parameter REST API, HTTP headers, dan endpoint mana pun yang menyusun query SQL dinamis dari input yang dikendalikan pengguna. Dari perspektif penyerang, mengidentifikasi titik injeksi adalah gerbang utama menuju kompromi basis data secara penuh dan potensi lateral movement di dalam infrastruktur.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INPV-05 |
| **CWE** | CWE-89 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Kritis (Critical) |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/05-Testing_for_SQL_Injection  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html  
* https://portswigger.net/web-security/sql-injection  
* https://portswigger.net/web-security/nosql-injection  

**Tools:** `sqlmap`, `Burp Suite (Intruder/Repeater)`, `Ghauri`, `jSQL Injection`, `BBQSQL`

### Apakah parameter yang dapat dikontrol pengguna diproses menggunakan metode akses basis data yang aman?
- [ ] Ya — aplikasi menggunakan ORM atau parameterized queries secara eksklusif dan bypass **tidak mungkin**  
- [ ] Ya — parameterized queries digunakan tetapi bypass **mungkin** melalui edge cases (misal: klausa `ORDER BY`)  
- [ ] Tidak — konkatenasi string mentah digunakan untuk membangun query SQL **tanpa** parameterisasi  

### Bisakah mekanisme autentikasi dilewati melalui SQL injection?
- [ ] Tidak — parameter login **tidak** rentan terhadap injeksi  
- [ ] Ya — bypass autentikasi **mungkin** menggunakan payload berbasis tautologi (misal: `' OR 1=1 --`)  

### Apakah eksfiltrasi data memungkinkan melalui teknik in-band atau out-of-band?
- [ ] Tidak — tidak ada eksfiltrasi data yang teridentifikasi  
- [ ] Ya — eksfiltrasi UNION-based atau error-based **mungkin** dilakukan  
- [ ] Ya — eksfiltrasi blind (Boolean atau Time-based) **mungkin** dilakukan  
- [ ] Ya — eksfiltrasi out-of-band (DNS/HTTP) **mungkin** dilakukan  

### Apakah fungsi aplikasi menunjukkan kerentanan second-order SQL injection?
- [ ] Tidak — data yang tersimpan ditangani dengan aman saat diambil untuk query selanjutnya  
- [ ] Ya — input pengguna disimpan dan kemudian digunakan dalam query SQL yang tidak aman **tanpa** validasi  

### Apakah hak istimewa akun layanan basis data dibatasi hingga minimum yang diperlukan?
- [ ] Ya — akun layanan memiliki **least privilege** (misal: terbatas pada tabel/tindakan tertentu saja)  
- [ ] Tidak — akun layanan memiliki hak istimewa tinggi (misal: `DROP TABLE`, hak akses `FILE`, atau `xp_cmdshell` **aktif**)

---

## WSTG-INPV-06 — Testing for LDAP Injection

LDAP Injection terjadi ketika aplikasi memasukkan data yang diberikan pengguna ke dalam filter LDAP (Lightweight Directory Access Protocol) tanpa sanitisasi atau escaping yang tepat, sehingga memungkinkan penyerang untuk memanipulasi logika kueri. Dengan menyuntikkan karakter khusus seperti tanda bintang (asterisk), tanda kurung, dan operator logika, penyerang dapat mengubah filter pencarian untuk melewati mekanisme autentikasi atau mengeksfiltrasi informasi direktori yang sensitif termasuk username, keanggotaan grup, dan atribut organisasi. Kerentanan ini biasanya muncul pada fitur-fitur seperti pencarian direktori perusahaan, portal karyawan, atau sistem Single Sign-On (SSO) yang terhubung dengan Active Directory atau OpenLDAP. Dari sudut pandang penyerang, eksploitasi yang berhasil sering kali melibatkan teknik blind untuk melakukan enumerasi struktur direktori atau nilai atribut secara bit-demi-bit ketika output kueri langsung ditekan oleh aplikasi.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INPV-06 |
| **CWE** | CWE-90 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/06-Testing_for_LDAP_Injection  
* https://hacktricks.wiki/en/pentesting-web/ldap-injection.html  

**Tools:** `Burp Suite (Intruder/Repeater)`, `ldapsearch`, `JNDIExploit`, `Wfuzz`, `nmap`

### Apakah aplikasi menggunakan LDAP untuk autentikasi atau pencarian direktori?
- [ ] Tidak — LDAP **tidak digunakan** dalam arsitektur aplikasi  
- [ ] Ya — LDAP digunakan dan semua input disanitisasi secara ketat atau diparameterisasi  
- [ ] Ya — LDAP digunakan dan input pengguna digabungkan ke dalam filter **tanpa** escaping yang tepat  

### Apakah meta-karakter LDAP telah di-escape atau difilter dengan benar pada input pengguna?
- [ ] Ya — karakter seperti `(`, `)`, `&`, `|`, `*`, dan `\` **tidak diizinkan** atau di-escape dengan benar  
- [ ] Tidak — meta-karakter **dapat** disuntikkan untuk mengubah struktur filter LDAP  

### Dapatkah mekanisme autentikasi dilewati melalui injeksi?
- [ ] Tidak — logika login **tidak rentan** terhadap manipulasi kueri  
- [ ] Ya — autentikasi **dapat** dilewati menggunakan injeksi logika OR (`|`) atau wildcard (`*`) pada kolom username atau password  

### Apakah eksfiltrasi data secara blind dimungkinkan dari layanan direktori?
- [ ] Tidak — hasil pencarian terbatas dan tidak ada perbedaan waktu (timing) atau boolean yang teramati  
- [ ] Ya — atribut direktori **dapat** dienumerasi secara bit-demi-bit melalui teknik boolean-based blind injection  
- [ ] Ya — rekaman direktori lengkap **ditampilkan** dalam respons aplikasi karena manipulasi kueri  

### Apakah komponen aplikasi sekunder (misalnya, pengirim email, SSO) rentan terhadap injeksi atribut berbasis LDAP?
- [ ] Tidak — atribut LDAP ditangani dengan aman di semua komponen  
- [ ] Ya — penyerang **dapat** mengubah atribut mereka sendiri (misalnya, email, keanggotaan grup) melalui injeksi untuk meningkatkan hak akses (escalate privileges) atau mengalihkan komunikasi

---

## WSTG-INPV-07 — XML Injection

XML Injection terjadi ketika sebuah aplikasi memasukkan data yang diberikan oleh pengguna ke dalam dokumen atau aliran (*stream*) XML tanpa validasi, sanitisasi, atau *escaping* karakter meta XML yang tepat. Dengan menyuntikkan tag XML atau elemen struktural tertentu, penyerang dapat memanipulasi logika dokumen, melewati mekanisme autentikasi, atau mengganggu integritas data yang diproses oleh *backend*. Kerentanan ini umumnya ditemukan pada layanan web berbasis SOAP, REST API yang menerima XML, asersi SAML, dan aplikasi yang menghasilkan file konfigurasi atau laporan XML secara dinamis. Dari perspektif penyerang, tujuannya adalah untuk keluar dari konteks data yang dimaksudkan guna mengubah struktur dokumen, yang berpotensi menyebabkan eskalasi hak istimewa (*privilege escalation*) yang tidak sah atau eksekusi logika bisnis yang tidak diinginkan.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INPV-07 |
| **CWE** | CWE-91 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi / Sedang |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/07-Testing_for_XML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/xxe  

**Alat:** `Burp Suite (Intruder/Repeater)`, `OWASP ZAP`, `XMLSpy`, `CyberChef`

### Apakah aplikasi menerima dan memproses input berformat XML dari pengguna?
- [ ] Tidak — aplikasi **tidak** memproses input XML  
- [ ] Ya — input XML diproses melalui API (SOAP/REST)  
- [ ] Ya — XML diproses melalui unggahan file (SVG, DOCX, dll.)  

### Apakah karakter meta XML (misalnya, `<`, `>`, `&`, `'`, `"`) di-*escape* atau disanitisasi dengan benar?
- [ ] Ya — semua karakter meta di-*escape* atau disanitisasi dengan **benar** *(Paling aman)*  
- [ ] Ya — beberapa pemfilteran diterapkan tetapi *bypass* **memungkinkan** menggunakan pengodean (*encoding*)  
- [ ] Tidak — input mentah dimasukkan langsung ke dalam struktur XML **tanpa** sanitisasi  

### Apakah validasi skema sisi server (XSD/DTD) diberlakukan untuk semua input XML?
- [ ] Ya — validasi skema yang ketat **diaktifkan** dan mencegah perubahan struktural  
- [ ] Ya — validasi **diaktifkan** tetapi **tidak** mencegah injeksi tag di dalam bidang data  
- [ ] Tidak — validasi skema **dinonaktifkan** atau tidak diimplementasikan  

### Bisakah penyerang menyuntikkan tag XML baru untuk mengubah struktur atau logika dokumen?
- [ ] Tidak — struktur tetap utuh terlepas dari input  
- [ ] Ya — tag dapat disuntikkan tetapi **tidak dapat** mengubah logika bisnis  
- [ ] Ya — injeksi tag **memungkinkan** dan mengizinkan *bypass* logika atau modifikasi data *(Kritis)*  

### Bisakah parser XML dimanipulasi menggunakan bagian CDATA atau komentar XML?
- [ ] Tidak — *parser* menangani atau menolak penanda CDATA/komentar dengan benar  
- [ ] Ya — injeksi `<![CDATA[...]]>` atau `<!-- ... -->` **memungkinkan** untuk menyembunyikan atau melewati filter

---

## WSTG-INPV-08 — Testing for SSI Injection

Server-Side Includes (SSI) Injection terjadi ketika sebuah aplikasi gagal melakukan sanitasi pada input pengguna sebelum memasukkannya ke dalam berkas HTML yang diproses oleh mesin SSI server. Penyerang mengeksploitasi hal ini dengan menginjeksikan direktif SSI, seperti `<!--#exec cmd="id" -->`, untuk mengeksekusi perintah shell yang sewenang-wenang, membaca berkas lokal, atau mengeksfiltrasi variabel lingkungan. Kerentanan ini biasanya ditemukan pada aplikasi yang melayani ekstensi berkas lama (legacy) seperti `.shtml`, `.shtm`, atau `.stm`, atau di mana web server secara eksplisit dikonfigurasi untuk memproses SSI dalam berkas HTML standar. Eksploitasi yang berhasil memberikan penyerang izin yang sama dengan proses web server, yang sering kali mengarah pada kompromi sistem secara penuh atau paparan data sensitif.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INPV-08 |
| **CWE** | CWE-97 |
| **Status Pengujian** | Tidak Dilakukan |
| **Keparahan** | Tinggi |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/08-Testing_for_SSI_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Intruder/Repeater)`, `ffuf`, `Wfuzz`, `curl`

### Apakah web server mendukung dan memproses direktif SSI?
- [ ] Tidak — server **tidak mendukung** SSI atau ekstensi berkas seperti `.shtml` **dinonaktifkan**  
- [ ] Ya — SSI **diaktifkan** tetapi input pengguna **tidak** direfleksikan dalam halaman yang diproses  
- [ ] Ya — SSI **diaktifkan** dan input pengguna **direfleksikan** dalam halaman yang diproses  

### Dapatkah perintah sistem yang sewenang-wenang dieksekusi melalui direktif `#exec`?
- [ ] Tidak — direktif `#exec` **dinonaktifkan** atau dibatasi oleh konfigurasi server  
- [ ] Ya — eksekusi perintah **dimungkinkan** melalui payload `#exec cmd` atau `#exec cgi`  

### Apakah eksfiltrasi berkas sensitif atau variabel lingkungan dimungkinkan?
- [ ] Tidak — direktif seperti `#include` atau `#config` **dinonaktifkan**  
- [ ] Ya — membaca berkas lokal (misalnya, `/etc/passwd`) **dimungkinkan** melalui `#include virtual`  
- [ ] Ya — variabel lingkungan server **dapat** dieksfiltrasi melalui `#printenv` atau `#echo`  

### Apakah kontrol validasi input atau WAF efektif terhadap payload SSI?
- [ ] Ya — validasi input yang ketat **diterapkan** dan bypass **tidak dimungkinkan**  
- [ ] Ya — validasi/WAF **diterapkan** tetapi bypass **dimungkinkan** menggunakan pengkodean karakter atau sintaks SSI yang berbeda  
- [ ] Tidak — tidak ada validasi atau perlindungan WAF untuk karakter SSI (`<`, `!`, `#`, `-`, `"`)

---

## WSTG-INPV-09 — Pengujian untuk XPath Injection

XPath Injection terjadi ketika sebuah aplikasi menggunakan informasi yang diberikan oleh pengguna untuk menyusun kueri XPath untuk data XML, yang memungkinkan penyerang untuk mengganggu logika kueri tersebut. Dengan menyuntikkan sintaks tertentu seperti tanda kutip tunggal, kurung siku, dan operator logika, penyerang dapat menavigasi struktur dokumen XML, melewati autentikasi (Bypass Authentication), atau mengeksfiltrasi node data sensitif. Kerentanan ini umumnya ditemukan pada layanan web (Web Services), antarmuka manajemen konfigurasi, dan sistem warisan (Legacy Systems) yang mengandalkan penyimpanan data berbasis XML daripada basis data SQL tradisional. Dari sudut pandang penyerang, hal ini sering dieksploitasi menggunakan teknik berbasis boolean (boolean-based) atau berbasis kesalahan (error-based) untuk memetakan pohon XML secara sistematis dan mengambil nilai tersembunyi dari backend.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INPV-09 |
| **CWE** | CWE-643 |
| **Test Status** | Belum Dilakukan |
| **Severity** | Tinggi / Kritis |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/09-Testing_for_XPath_Injection  
* https://hacktricks.wiki/en/pentesting-web/xpath-injection.html  

**Alat:** `Burp Suite (Intruder)`, `XCat`, `XPath-Injection-Tool`, `FFUF`

### Apakah input pengguna yang digunakan untuk menyusun kueri XPath telah disanitasi atau diparameterisasi dengan benar?
- [ ] Ya — input **tidak** digunakan dalam kueri XPath atau telah diparameterisasi secara ketat  
- [ ] Ya — validasi input/pengkodean (encoding) yang kuat **telah diterapkan** dan bypass **tidak dimungkinkan**  
- [ ] Ya — beberapa validasi **telah diterapkan** tetapi bypass **dimungkinkan** menggunakan karakter yang dikodekan  
- [ ] Tidak — input pengguna digabungkan secara langsung (concatenated) ke dalam kueri XPath *(Kritis)*  

### Apakah aplikasi mengungkapkan detail struktural XML/XPath melalui pesan kesalahan?
- [ ] Tidak — halaman kesalahan generik ditampilkan dan **tidak ada** detail XML yang bocor  
- [ ] Ya — pesan kesalahan mengungkapkan adanya pemrosesan XML tetapi **tidak ada** detail jalur (path)  
- [ ] Ya — pesan kesalahan yang mendetail (verbose) mengungkapkan sintaks XPath, nama node, atau jalur file  

### Apakah mungkin untuk melewati logika autentikasi atau otorisasi menggunakan logika XPath?
- [ ] Tidak — logika autentikasi **tidak** bergantung pada XML/XPath atau diimplementasikan secara aman  
- [ ] Ya — pemeriksaan login atau izin dapat dilewati menggunakan payload berbasis logika seperti `' or 1=1 or '`  

### Bisakah struktur dokumen atau data XML dienumerasi melalui blind injection?
- [ ] Tidak — aplikasi **tidak** rentan terhadap inferensi berbasis boolean atau berbasis waktu  
- [ ] Ya — nama node dan data **dapat** dieksfiltrasi menggunakan inferensi berbasis boolean  
- [ ] Ya — penelusuran pohon XML lengkap (full XML tree traversal) dan ekstraksi data **dimungkinkan**

---

## WSTG-INPV-10 — IMAP SMTP Injection

Kerentanan IMAP dan SMTP Injection muncul ketika aplikasi web tidak menyaring data yang diberikan pengguna secara semestinya sebelum memasukkannya ke dalam perintah yang dikirim ke server email. Dengan menyuntikkan karakter Carriage Return dan Line Feed (CRLF), penyerang dapat menghentikan perintah yang dimaksud dan menyisipkan instruksi email sembarang, seperti menambahkan penerima tambahan (CC/BCC) atau memodifikasi isi pesan. Celah ini paling sering ditemukan pada gateway web-to-mail, formulir kontak, dan klien email yang berkomunikasi langsung dengan server email melalui protokol seperti IMAP4 atau SMTP. Eksploitasi yang berhasil memungkinkan pengiriman spam tanpa izin (unauthorized relaying), eksfiltrasi konten email yang sensitif, dan dalam beberapa kasus, kompromi total terhadap integritas server email.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INPV-10 |
| **CWE** | CWE-93 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/10-Testing_for_IMAP_SMTP_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite (Repeater/Intruder)`, `netcat`, `telnet`, `nmap`

### Apakah aplikasi memproses masukan yang diberikan pengguna untuk membangun perintah IMAP atau SMTP?
- [ ] Tidak — aplikasi **tidak** berinteraksi dengan server email melalui masukan pengguna  
- [ ] Ya — aplikasi berinteraksi dengan server email tetapi menggunakan API atau library yang aman  
- [ ] Ya — aplikasi membangun perintah email mentah menggunakan parameter yang dikendalikan pengguna  

### Apakah masukan divalidasi untuk mencegah penyuntikan urutan Carriage Return (`\r`) dan Line Feed (`\n`)?
- [ ] Ya — karakter CRLF disaring secara **ketat**, di-encode, atau ditolak  
- [ ] Ya — masukan divalidasi terhadap allowlist karakter yang ketat  
- [ ] Tidak — karakter CRLF **tidak** disaring, memungkinkan penghentian perintah dan injeksi  

### Bisakah penyerang menyuntikkan header email tambahan seperti `Bcc:` atau `Cc:`?
- [ ] Tidak — modifikasi header **tidak memungkinkan** karena batasan masukan yang ketat  
- [ ] Ya — penyuntikan header tambahan **memungkinkan** melalui urutan CRLF  
- [ ] Ya — penerusan email (mail relaying) ke domain eksternal **aktif** dan dapat dieksploitasi *(Kritis)*  

### Apakah aplikasi memungkinkan manipulasi perintah IMAP untuk mengakses kotak surat (mailbox) yang tidak sah?
- [ ] Tidak — aplikasi **tidak** menggunakan IMAP atau membatasi akses kotak surat secara ketat hanya untuk pengguna yang terautentikasi  
- [ ] Ya — IMAP command injection **memungkinkan** tetapi terbatas pada enumerasi metadata  
- [ ] Ya — IMAP command injection memungkinkan pembacaan atau penghapusan email pengguna lain  

### Apakah server email backend rentan terhadap command pipelining?
- [ ] Tidak — server email menolak perintah batch atau beberapa perintah dalam satu sesi  
- [ ] Ya — server email memproses beberapa perintah yang disuntikkan melalui satu bidang masukan (input field)

---

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

## WSTG-INPV-12 — Command Injection

Command Injection (Injeksi Perintah) terjadi ketika sebuah aplikasi meneruskan data tidak aman yang disediakan oleh pengguna, seperti input formulir, header HTTP, atau cookie, ke shell sistem. Kerentanan ini memungkinkan penyerang untuk mengeksekusi perintah sistem operasi arbitrer pada server, biasanya dengan hak akses (privileges) dari proses aplikasi web tersebut. Dari sudut pandang penyerang, hal ini dieksploitasi dengan menggunakan metakarakter shell seperti titik koma (semicolon), pipe, atau backtick untuk merantai perintah berbahaya ke dalam alur eksekusi yang dimaksudkan. Dampaknya seringkali bersifat katastropik, yang mengarah pada pengambilalihan sistem secara penuh (full system compromise), eksfiltrasi data tanpa izin, atau pergerakan lateral (lateral movement) di dalam jaringan internal.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INPV-12 |
| **CWE** | CWE-78 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Kritis |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/12-Testing_for_Command_Injection  
* https://hacktricks.wiki/en/pentesting-web/command-injection.html  
* https://portswigger.net/web-security/os-command-injection  

**Alat:** `Commix`, `Burp Suite (Intruder/Repeater)`, `dnsbin`, `Interactsh`, `Collaborator`

### Apakah aplikasi memanggil perintah tingkat sistem atau executable shell?
- [ ] Tidak — aplikasi **tidak** berinteraksi dengan shell OS  
- [ ] Ya — aplikasi menggunakan API internal atau library tingkat tinggi untuk tugas-tugas sistem  
- [ ] Ya — aplikasi secara langsung memanggil perintah shell melalui system calls seperti `system()`, `exec()`, atau `popen()`  

### Apakah input pengguna divalidasi atau disanitasi sebelum diteruskan ke system calls?
- [ ] Ya — allow-listing yang ketat dan validasi input **telah diterapkan**  
- [ ] Ya — sanitasi atau escaping digunakan tetapi bypass **mungkin dilakukan**  
- [ ] Tidak — input pengguna diteruskan langsung ke shell **tanpa** validasi  

### Apakah metakarakter shell dapat digunakan untuk memanipulasi alur eksekusi perintah?
- [ ] Tidak — metakarakter telah difilter atau di-escape dengan benar  
- [ ] Ya — eksekusi in-band di mana output perintah tercermin dalam respons **mungkin terjadi**  
- [ ] Ya — eksekusi blind di mana tidak ada output yang tercermin **mungkin terjadi**  

### Apakah eksfiltrasi out-of-band (OOB) memungkinkan dari host?
- [ ] Tidak — server tidak memiliki akses keluar (egress) atau sinyal OOB (DNS/HTTP) gagal  
- [ ] Ya — eksfiltrasi data melalui sinyal OOB berbasis DNS atau HTTP **mungkin dilakukan**  

### Apakah proses aplikasi berjalan dengan hak akses sistem yang tinggi (elevated privileges)?
- [ ] Tidak — aplikasi berjalan dengan akun layanan khusus berhak akses rendah  
- [ ] Ya — aplikasi berjalan sebagai `root`, `Administrator`, atau pengguna dengan hak akses tinggi *(Kritis)*

---

## WSTG-INPV-13 — Format String Injection

Format string injection (Injeksi string format) terjadi ketika aplikasi web meneruskan masukan yang dikendalikan pengguna secara langsung ke dalam argumen string format dari fungsi variadik (variadic function), seperti keluarga `printf` pada bahasa C atau pembungkus logging tingkat rendah (low-level logging wrappers). Meskipun kurang umum pada kerangka kerja web kode terkelola (managed-code) modern, kerentanan ini tetap kritis pada aplikasi CGI lama (legacy), ekstensi native, atau sistem logging kasus tepi (edge-case) yang berinteraksi dengan pustaka sistem tingkat rendah. Penyerang mengeksploitasi hal ini dengan memberikan spesifikasi format (format specifiers) seperti `%x` atau `%p` untuk membocorkan alamat memori sensitif dan data dari stack, atau spesifikasi `%n` untuk melakukan penulisan memori arbitrer (arbitrary memory writes). Eksploitasi yang berhasil biasanya berujung pada kompromi sistem secara penuh melalui Remote Code Execution (RCE) atau, setidaknya, kondisi Denial-of-Service (DoS) yang berdampak melalui kegagalan proses (crash).


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INPV-13 |
| **CWE** | CWE-134 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi / Kritis |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/13-Testing_for_Format_String_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `GDB`, `strings`, `radare2`, `Checksec`, `AFL++`

### Apakah aplikasi memproses masukan pengguna melalui kode native atau antarmuka CGI lama?
- [ ] Tidak — aplikasi dibangun sepenuhnya pada kerangka kerja kode terkelola (misalnya, JVM, CLR)  
- [ ] Ya — aplikasi menggunakan JNI, ekstensi C/C++ native, atau CGI biner lama di mana kerentanan Format String **dapat** ada  

### Apakah masukan yang diberikan pengguna diteruskan sebagai argumen utama ke fungsi yang peka terhadap format (format-aware functions)?
- [ ] Tidak — masukan diteruskan sebagai argumen data, dan string format bersifat **statis** serta **hardcoded**  
- [ ] Ya — masukan pengguna digabungkan langsung ke dalam argumen string format, namun sanitisasi **telah diterapkan**  
- [ ] Ya — masukan pengguna digunakan secara langsung sebagai string format dan tidak ada sanitisasi yang **diterapkan**  

### Apakah mungkin untuk membocorkan konten memori menggunakan spesifikasi format?
- [ ] Tidak — masukan divalidasi dan spesifikasi seperti `%p`, `%x`, atau `%s` **tidak diproses**  
- [ ] Ya — pemberian spesifikasi `%p` atau `%x` menghasilkan refleksi alamat memori stack atau heap  
- [ ] Ya — data sensitif (misalnya, pointer, stack cookies) **dapat** dieksfiltrasi melalui spesifikasi format  

### Apakah aplikasi dapat dibuat crash atau dimanipulasi menggunakan spesifikasi `%n`?
- [ ] Tidak — spesifikasi yang mengizinkan penulisan memori **dinonaktifkan** atau diblokir oleh compiler/lingkungan  
- [ ] Ya — aplikasi mengalami crash (DoS) ketika `%s%s%s%s` atau string serupa diberikan  
- [ ] Ya — penulisan memori arbitrer via `%n` **mungkin dilakukan**, yang berpotensi menyebabkan Remote Code Execution (RCE)  

### Apakah perlindungan biner modern (ASLR, DEP, Stack Canaries) dapat dilewati (bypass) melalui injeksi ini?
- [ ] Tidak — lingkungan telah diperkeras dan kebocoran memori **tidak dapat** digunakan untuk melewati perlindungan  
- [ ] Ya — Format String Injection **dapat** digunakan untuk membocorkan alamat dasar, sehingga bypass ASLR/DEP **mungkin dilakukan**

---

## WSTG-INPV-14 — Testing for Incubated Vulnerabilities

Kerentanan inkubasi, yang juga dikenal sebagai kerentanan persisten atau *second-order*, terjadi ketika Payload berbahaya disimpan oleh aplikasi dalam keadaan "dingin" dan kemudian dieksekusi atau diproses dalam konteks yang berbeda. Pola serangan ini biasanya menargetkan sistem back-end, konsol administratif, atau alat pelaporan otomatis yang mengambil data dari basis data bersama atau filesystem tanpa validasi ulang atau API *output encoding* yang memadai. Pentester harus menelusuri siklus hidup input pengguna dari titik masuk ("inkubator") ke setiap lokasi di mana data tersebut akhirnya dirender atau digunakan oleh komponen lain atau aplikasi sekunder. Eksploitasi yang berhasil sering kali menghasilkan dampak tinggi seperti pembajakan sesi administratif melalui Stored XSS atau eksfiltrasi data melalui second-order SQL Injection dalam modul pelaporan internal.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INPV-14 |
| **CWE** | CWE-20 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Tinggi |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/14-Testing_for_Incubated_Vulnerability  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html#second-order-injection  
* https://portswigger.net/web-security/web-cache-deception  
* https://portswigger.net/web-security/web-cache-poisoning  

**Alat:** `Burp Suite (Collaborator)`, `sqlmap`, `OWASP ZAP`, `KXSs`, `Dalfox`

### Apakah terdapat endpoint di mana input yang dikontrol pengguna disimpan untuk diambil kemudian oleh pengguna atau proses lain?
- [ ] Tidak — aplikasi **tidak** menyimpan input pengguna untuk penggunaan di masa mendatang  
- [ ] Ya — input **disimpan** dalam kolom basis data, file log, atau pengaturan konfigurasi  

### Apakah validasi input atau sanitasi diterapkan sebelum data ditulis ke lapisan penyimpanan persisten?
- [ ] Ya — validasi *allow-list* yang ketat **diterapkan** pada titik masuk  
- [ ] Ya — sanitasi umum **diterapkan** tetapi bypass **dimungkinkan** melalui encoding atau karakter non-standar  
- [ ] Tidak — input disimpan dalam bentuk mentah (*raw*), tanpa validasi  

### Apakah data yang disimpan di-encode atau disanitasi dengan benar saat dirender di antarmuka administratif atau sekunder?
- [ ] Ya — *context-aware output encoding* **diterapkan** di setiap lokasi data dirender  
- [ ] Ya — encoding **diterapkan** di beberapa lokasi tetapi **tidak ada** di lokasi lain (misalnya, dasbor admin internal)  
- [ ] Tidak — data dirender tanpa encoding atau sanitasi apa pun, memungkinkan eksekusi Payload  

### Apakah Payload yang disimpan dalam basis data dapat memengaruhi proses batch back-end atau sistem pemantauan out-of-band?
- [ ] Tidak — proses back-end menggunakan metode parsing yang aman atau *parameterized queries*  
- [ ] Ya — Payload yang disimpan **dapat** memicu interaksi dalam logika back-end (misalnya, CSV Injection, Command Injection dalam log)  
- [ ] Ya — Payload yang disimpan **dapat** memicu permintaan *out-of-band* (OOB) ke server yang dikontrol penyerang

---

## WSTG-INPV-15 — HTTP Splitting Smuggling

Kerentanan HTTP Splitting dan Smuggling muncul dari ketidaksesuaian tentang bagaimana proksi frontend dan server backend menginterpretasikan serta memproses batas-batas permintaan HTTP, khususnya terkait header `Content-Length` dan `Transfer-Encoding`. Dengan menyusun permintaan yang ambigu, penyerang dapat "menyelundupkan" (smuggle) permintaan tersembunyi ke backend atau menyuntikkan sekuens CRLF untuk memecah respons, yang menyebabkan cache poisoning, pembajakan permintaan (request hijacking), atau bypass kontrol keamanan. Celah ini biasanya bermanifestasi dalam lingkungan kompleks yang menggunakan reverse proxy, load balancer, atau CDN yang memiliki logika pemrosesan (parsing logic) yang tidak konsisten. Dari sudut pandang penyerang, hal ini memungkinkan pengalihan lalu lintas pengguna, pencurian token sesi sensitif, dan eksekusi tindakan yang tidak sah dalam konteks sesi pengguna lain.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INPV-15 |
| **CWE** | CWE-444 |
| **Status Pengujian** | Tidak Dilakukan |
| **Tingkat Kerawanan** | Tinggi / Kritis |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/15-Testing_for_HTTP_Response_Splitting  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  

**Alat:** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `smuggler.py`, `curl`

### Apakah lingkungan rentan terhadap request smuggling melalui ketidaksesuaian CL.TE atau TE.CL?
- [ ] Tidak — server frontend dan backend menangani batas-batas permintaan secara konsisten  
- [ ] Ya — terdapat ketidaksesuaian tetapi eksploitasi **tidak dimungkinkan** karena mitigasi infrastruktur  
- [ ] Ya — smuggling CL.TE atau TE.CL **dimungkinkan**, memungkinkan permintaan tersembunyi mencapai backend *(Tinggi)*  
- [ ] Ya — TE.TE (double encoding/obfuscation) **dimungkinkan** untuk melakukan bypass filter frontend *(Kritis)*  

### Apakah HTTP Response Splitting dapat dicapai melalui injeksi CRLF pada header?
- [ ] Tidak — aplikasi melakukan sanitasi sekuens CRLF dengan benar pada semua input header  
- [ ] Ya — sekuens CRLF tercermin dalam header tetapi response splitting **tidak dimungkinkan**  
- [ ] Ya — injeksi CRLF **dimungkinkan**, memungkinkan injeksi header atau cache poisoning  

### Apakah kontrol keamanan (WAF/ACL) dapat di-bypass menggunakan permintaan yang diselundupkan (smuggled requests)?
- [ ] Tidak — kontrol keamanan berlaku untuk permintaan luar maupun yang diselundupkan  
- [ ] Ya — permintaan yang diselundupkan **dapat** melewati aturan WAF frontend atau ACL berbasis IP  

### Apakah mungkin untuk membajak sesi pengguna lain atau mengalihkan lalu lintas mereka?
- [ ] Tidak — aliran request/response terisolasi dan tidak dapat bersilangan  
- [ ] Ya — request smuggling **dimungkinkan** dan memungkinkan pengambilan permintaan pengguna lain *(Kritis)*  
- [ ] Ya — response splitting **dimungkinkan** dan memungkinkan browser-side cache poisoning atau XSS

---

## WSTG-INPV-16 — HTTP Request Smuggling

HTTP Request Smuggling (HRS) terjadi ketika rantai server HTTP (seperti load balancer dan back-end web server) menginterpretasikan batasan permintaan (request boundaries) secara berbeda, biasanya karena header `Content-Length` dan `Transfer-Encoding` yang bertentangan. Penyerang mengeksploitasi desinkronisasi ini untuk "menyelundupkan" (smuggle) entri ke dalam buffer permintaan server back-end, yang secara efektif memberikan awalan (prefix) pada permintaan pengguna sah berikutnya dengan segmen yang dikendalikan penyerang. Teknik ini memungkinkan serangan berat termasuk session hijacking, mem-bypass kontrol keamanan, dan cache poisoning. Eksploitasi biasanya dilakukan dengan analisis berbasis waktu (timing-based analysis) atau dengan mengamati respons yang tidak terduga dari back-end saat permintaan berikutnya dikirim ke server.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INPV-16 |
| **CWE** | CWE-444 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Kritis / Tinggi |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/16-Testing_for_HTTP_Request_Smuggling  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  
* https://portswigger.net/web-security/request-smuggling  

**Tools:** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `curl`, `SmuggleHunter`

### Apakah server front-end dan back-end menangani header `Transfer-Encoding: chunked` secara konsisten?
- [ ] Ya — kedua server menangani chunked encoding secara identik dan desinkronisasi **tidak dimungkinkan**  
- [ ] Tidak — server menginterpretasikan header secara berbeda tetapi sanitasi/normalisasi **diterapkan**  
- [ ] Tidak — desinkronisasi CL.TE (Content-Length/Transfer-Encoding) **dimungkinkan** *(Kritis)*  
- [ ] Tidak — desinkronisasi TE.CL (Transfer-Encoding/Content-Length) **dimungkinkan** *(Kritis)*  

### Dapatkah penyerang meracuni (poison) cache sisi server atau klien menggunakan permintaan yang diselundupkan?
- [ ] Tidak — caching **dinonaktifkan** atau tidak rentan terhadap peracunan  
- [ ] Ya — permintaan yang diselundupkan **dapat** mengalihkan pengguna ke domain berbahaya atau menyuntikkan skrip melalui cache poisoning  

### Apakah mungkin untuk menangkap permintaan pengguna lain atau token sesi melalui penggabungan permintaan (request concatenation)?
- [ ] Tidak — penyelundupan **tidak** memungkinkan paparan data antar-pengguna  
- [ ] Ya — penyelundupan memungkinkan penambahan permintaan pengguna berikutnya ke dalam bodi `POST` yang dikendalikan penyerang, sehingga eksfiltrasi **dimungkinkan**  

### Apakah infrastruktur menggunakan HTTP/2, dan apakah rentan terhadap desinkronisasi H2.CL atau H2.TE?
- [ ] Tidak — aplikasi hanya menggunakan HTTP/1.1 atau HTTP/2 diimplementasikan dengan aman tanpa downgrade  
- [ ] Ya — terjadi downgrade cleartext dari HTTP/2 ke HTTP/1.1 dan desinkronisasi **dimungkinkan**  

### Apakah header yang cacat (malformed) (misalnya, `Transfer-Encoding:  chunked` dengan spasi) ditangani dengan aman?
- [ ] Ya — header yang cacat ditolak atau dinormalisasi di semua tingkatan  
- [ ] Tidak — desinkronisasi TE.TE **dimungkinkan** karena penanganan header yang cacat atau tersembunyi (obscured) yang tidak konsisten

---

## WSTG-INPV-17 — Testing for Host Header Injection

Host Header Injection terjadi ketika aplikasi mempercayai header `Host` yang diberikan oleh pengguna dan menyertakannya ke dalam logika sisi server, seperti pembuatan tautan atau pengalihan (redirect), tanpa validasi yang memadai. Penyerang memanipulasi header ini untuk memfasilitasi Web Cache Poisoning, memfasilitasi Password Reset Poisoning dengan mengalihkan token sensitif ke domain eksternal, atau melewati kontrol keamanan yang bergantung pada perutean berbasis header. Eksploitasi yang berhasil memungkinkan penyerang untuk mengeksfiltrasi data sesi, melakukan pengambilalihan akun (Account Takeover), atau mengalihkan pengguna yang tidak menaruh curiga ke infrastruktur berbahaya. Kerentanan ini paling umum ditemukan di lingkungan dengan penyeimbang beban (load-balanced) atau aplikasi yang menghasilkan URL absolut secara dinamis berdasarkan konteks permintaan yang masuk.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INPV-17 |
| **CWE** | CWE-20 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Sedang / Tinggi* |

> *Tingkat keparahan menjadi Tinggi jika header Host digunakan untuk menghasilkan tautan sensitif dalam email, seperti pengaturan ulang kata sandi (password reset).

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/17-Testing_for_Host_Header_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/host-header  

**Alat:** `Burp Suite (Repeater/Collaborator)`, `curl`, `Param Miner`, `nmap (http-vhosts)`

### Apakah aplikasi mencerminkan nilai header `Host` dalam tautan, pengalihan (redirect), atau skrip?
- [ ] Tidak — aplikasi menggunakan base URL yang di-hardcode atau konfigurasi yang divalidasi secara ketat  
- [ ] Ya — header `Host` dicerminkan tetapi divalidasi secara ketat terhadap daftar putih (whitelist)  
- [ ] Ya — header `Host` dicerminkan dan bypass **mungkin dilakukan** melalui nilai yang salah format (misalnya, `expected.com:attacker.com`)  
- [ ] Ya — header `Host` dicerminkan **tanpa** validasi apa pun  

### Apakah header alternatif terkait host diproses oleh aplikasi atau infrastruktur?
- [ ] Tidak — header seperti `X-Forwarded-Host`, `X-Host`, atau `Forwarded` diabaikan  
- [ ] Ya — header alternatif diproses tetapi **tidak** menimpa (override) logika internal  
- [ ] Ya — header alternatif **dapat** digunakan untuk menimpa nilai header `Host` utama  

### Dapatkah header `Host` dimanipulasi untuk meracuni email yang dihasilkan sistem (misalnya, pengaturan ulang kata sandi)?
- [ ] Tidak — tautan email menggunakan domain statis tepercaya dari konfigurasi server  
- [ ] Ya — header `Host` memengaruhi tautan email tetapi validasi **tidak dapat** dilewati  
- [ ] Ya — header `Host` **dapat** dimanipulasi untuk mengalihkan token pengaturan ulang kata sandi ke domain yang dikendalikan penyerang *(Kritis)*  

### Apakah aplikasi rentan terhadap Web Cache Poisoning melalui header `Host`?
- [ ] Tidak — tidak ada mekanisme caching atau header `Host` adalah bagian dari cache key  
- [ ] Ya — terdapat cache tetapi ia memvalidasi secara ketat atau mengabaikan header `Host` untuk input yang tidak memiliki key (unkeyed inputs)  
- [ ] Ya — terdapat cache dan **dapat** diracuni oleh header `Host` yang diinjeksikan untuk menyajikan konten berbahaya kepada pengguna lain  

### Apakah server menerima header `Host` sembarang untuk perutean host virtual?
- [ ] Tidak — server mengembalikan kesalahan atau halaman default untuk header `Host` yang tidak dikenali  
- [ ] Ya — server menerima header `Host` apa pun, yang **berguna** untuk rekognisi atau enumerasi layanan internal

---

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

## WSTG-INPV-19 — Testing for Server-Side Request Forgery (SSRF)

Server-Side Request Forgery (SSRF) terjadi ketika penyerang dapat memengaruhi aplikasi web untuk membuat permintaan yang tidak sah dari server ke sumber daya internal atau eksternal. Dengan memanipulasi parameter yang mengharapkan URL, hostname, atau alamat IP, penyerang dapat melewati kontrol tingkat jaringan seperti firewall dan ACL untuk melakukan probe pada segmen jaringan internal, mengakses layanan metadata cloud (IMDS), atau berinteraksi dengan API internal dan basis data yang rentan. Kerentanan ini biasanya muncul pada fitur-fitur yang melibatkan pengambilan gambar (image fetching), pembuatan PDF (PDF generation), webhook, atau unggahan berkas melalui URL. Dari sudut pandang penyerang, SSRF adalah primitif yang kuat yang digunakan untuk melakukan pivot ke lingkungan yang terisolasi, mengeksfiltrasi data konfigurasi yang sensitif, atau berpotensi mencapai eksekusi kode jarak jauh (Remote Code Execution) melalui interaksi dengan layanan internal seperti Redis atau Memcached.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-19 |
| **CWE** | CWE-918 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Tinggi / Kritis |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/19-Testing_for_Server-Side_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/ssrf-server-side-request-forgery/index.html  
* https://portswigger.net/web-security/ssrf  

**Alat:** `Burp Suite (Collaborator/Interactsh)`, `Interact.sh`, `Gopherus`, `ffuf`, `cURL`, `DNSRebind Tool`

### Apakah aplikasi memproses URL atau hostname yang diberikan oleh pengguna?
- [ ] Tidak — tidak ada fitur yang menerima URL atau hostname eksternal  
- [ ] Ya — fitur tersedia tetapi input **divalidasi secara ketat** terhadap daftar izin (allow-list)  
- [ ] Ya — fitur tersedia dan menerima URL yang diberikan pengguna  

### Apakah kontrol validasi atau daftar blokir diterapkan pada tujuan yang diminta?
- [ ] Ya — sebuah **allow-list** ketat untuk domain/IP tepercaya diterapkan  
- [ ] Ya — sebuah **deny-list** digunakan (misalnya, memblokir `127.0.0.1`, `169.254.169.254`)  
- [ ] No — **tidak ada validasi** atau pemfilteran yang diterapkan pada input  

### Apakah mungkin untuk melewati filter melalui encoding, redirect, atau DNS rebinding?
- [ ] Tidak — filter kuat dan menangani redirect/resolusi DNS dengan aman  
- [ ] Ya — bypass **dimungkinkan** menggunakan URL encoding atau format IP alternatif (hex, octal)  
- [ ] Ya — bypass **dimungkinkan** melalui HTTP redirect 3xx ke target internal  
- [ ] Ya — bypass **dimungkinkan** melalui serangan DNS rebinding untuk melakukan resolusi ke IP internal  

### Dapatkah aplikasi menjangkau layanan metadata internal atau antarmuka lokal?
- [ ] Tidak — jaringan lokal dan endpoint metadata cloud **tidak dapat dijangkau**  
- [ ] Ya — akses ke localhost (`127.0.0.1`) atau layanan loopback **dimungkinkan**  
- [ ] Ya — akses ke layanan metadata cloud (misalnya, AWS/GCP/Azure IMDS) **dimungkinkan** *(Kritis)*  

### Bagaimana karakteristik respons SSRF tersebut?
- [ ] Blind — tidak ada respons atau metadata yang dikembalikan ke pengguna  
- [ ] Partial — metadata respons (header, ukuran, waktu) dikembalikan ke pengguna  
- [ ] Full — seluruh isi respons (response body) dari sumber daya internal **direfleksikan** ke pengguna

---

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

## WSTG-ERRH-01 — Testing for Improper Error Handling

Penanganan kesalahan yang tidak tepat (Improper Error Handling) terjadi ketika aplikasi web mengungkapkan detail teknis yang sensitif—seperti stack traces, informasi skema basis data (database schema), atau jalur file internal—melalui respons kesalahannya. Penyerang memicu kesalahan ini dengan mengirimkan input yang cacat (malformed input), mengakses sumber daya yang tidak ada, atau menginduksi pengecualian sisi server (server-side exceptions) untuk memetakan arsitektur dasar aplikasi dan mengidentifikasi vektor potensial untuk eksploitasi lebih lanjut. Kebocoran informasi ini sering kali menjadi pendahulu bagi serangan yang lebih parah, termasuk SQL Injection atau path traversal, dengan memberikan spesifikasi lingkungan yang tepat kepada penyerang. Implementasi yang aman harus menyediakan pesan ramah pengguna yang generik (generic user-friendly messages) sambil menangkap diagnostik terperinci hanya dalam log sisi server yang aman.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-ERRH-01 |
| **CWE** | CWE-209 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Rendah / Sedang |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/01-Testing_For_Improper_Error_Handling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite (Repeater/Intruder)`, `ffuf`, `wfuzz`, `Arjun`, `curl`

### Apakah aplikasi menggunakan penanganan kesalahan global yang generik untuk pengecualian yang tidak tertangani (unhandled exceptions)?
- [ ] Ya — halaman kesalahan generik **diaktifkan** dan **tidak** mengungkapkan detail teknis  
- [ ] Ya — halaman kesalahan khusus digunakan tetapi kebocoran **mungkin terjadi** melalui header tertentu  
- [ ] Tidak — halaman kesalahan server default (misalnya, Tomcat, IIS, Nginx) **terlihat**  

### Dapatkah informasi teknis dienumerasi melalui input yang cacat (malformed input) atau kasus batas (boundary cases)?
- [ ] Tidak — kesalahan ditangani dengan baik dengan ID referensi unik untuk logging  
- [ ] Ya — stack traces atau kueri basis data (database queries) **diungkapkan** dalam respons  
- [ ] Ya — jalur file internal, variabel lingkungan (environment variables), atau string versi server **diungkapkan**  

### Apakah respons API membocorkan objek kesalahan verbose atau informasi debug?
- [ ] Tidak — API mengembalikan kode kesalahan standar dan pesan JSON yang telah dibersihkan (sanitized)  
- [ ] Ya — API mengembalikan objek debug verbose atau detail pengecualian lengkap dalam body respons  
- [ ] Ya — stack traces disertakan dalam bidang `details` atau `exception` dari respons JSON  

### Apakah aplikasi berperilaku berbeda (Berbasis waktu atau Berbasis konten) saat terjadi kesalahan?
- [ ] Tidak — tanda tangan (signatures) dan waktu respons konsisten terlepas dari jenis kesalahannya  
- [ ] Ya — respons diferensial **dapat** digunakan untuk mengenumerasi status valid vs. tidak valid (misalnya, User Enumeration)

---

## WSTG-ERRH-02 — Testing for Stack Traces

Stack trace dihasilkan ketika aplikasi gagal menangani sebuah `exception` dengan baik, yang mengakibatkan terungkapnya status eksekusi internal dan hierarki panggilan (`call hierarchy`) kepada pengguna akhir. Bagi seorang penetration tester, trace ini sangat berharga karena dapat mengungkapkan `application framework`, versi pustaka (`library`), jalur sistem berkas (`file system paths`) internal, dan alur logika, yang secara signifikan mengurangi upaya yang diperlukan untuk `reconnaissance`. Eksploitasi melibatkan pemicuan kesalahan (`error`) secara sengaja melalui `malformed input`, tipe data yang tidak terduga, atau pelanggaran `boundary condition` untuk memaksa aplikasi ke dalam status tidak stabil dan mengeksfiltrasi informasi tentang `technology stack` yang mendasarinya. Kebocoran ini sering kali memberikan konteks yang diperlukan untuk mengidentifikasi komponen pihak ketiga yang rentan atau untuk merancang `exploit` spesifik bagi kelemahan logika yang ditemukan dalam jalur kode yang terungkap.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-ERRH-02 |
| **CWE** | CWE-209 |
| **Status Pengujian** | Belum Dilakukan |
| **Severity** | Low / Medium* |

> *Severity meningkat menjadi Medium jika stack trace mengungkapkan detail arsitektur yang sensitif, kueri database (database queries), atau parameter konfigurasi internal.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/02-Testing_for_Stack_Traces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Peralatan:** `Burp Suite`, `ffuf`, `Arjun`, `Wfuzz`, `Wappalyzer`

### Apakah stack trace lengkap dikembalikan ke klien saat terjadi kesalahan aplikasi?
- [ ] Tidak — aplikasi mengembalikan halaman kesalahan umum atau pesan kesalahan kustom  
- [ ] Ya — stack trace **diaktifkan** tetapi hanya untuk modul tertentu yang tidak sensitif  
- [ ] Ya — stack trace lengkap **diaktifkan** dan terlihat dalam badan respons HTTP  

### Apakah pesan kesalahan mengungkapkan informasi lingkungan yang sensitif?
- [ ] Tidak — pesan kesalahan tidak mengandung detail teknis atau lingkungan  
- [ ] Ya — pesan mengungkapkan **jalur berkas internal** atau **struktur direktori** sisi server  
- [ ] Ya — pesan mengungkapkan **skema database**, **kueri SQL**, atau **versi pustaka pihak ketiga**  

### Dapatkah stack trace dipicu dengan memanipulasi parameter input?
- [ ] Tidak — malformed input ditangani dengan baik atau ditolak oleh validasi  
- [ ] Ya — trace dapat dipicu oleh **tipe data yang tidak terduga** (misalnya, mengirimkan array di mana string diharapkan)  
- [ ] Ya — trace dipicu oleh **null bytes**, **karakter khusus**, atau **nilai batas** (boundary values)  

### Apakah global error handler diterapkan secara konsisten di seluruh aplikasi?
- [ ] Ya — sebuah global exception handler **diterapkan secara konsisten** di semua endpoint yang diuji  
- [ ] Ya — penangan (handler) tersedia tetapi **tidak diterapkan** pada endpoint legacy, API, atau endpoint yang baru ditambahkan  
- [ ] Tidak — tidak ada mekanisme penanganan kesalahan global yang **terdeteksi**, mengakibatkan kesalahan server default

---

## WSTG-CRYP-01 — Pengujian Keamanan Transport Layer yang Lemah

Pengujian untuk keamanan transport layer yang lemah melibatkan analisis implementasi dan konfigurasi SSL/TLS untuk memastikan kerahasiaan dan integritas data selama transit. Penyerang menargetkan kesalahan konfigurasi seperti protokol yang usang (SSLv2, TLS 1.0), cipher suite yang lemah (NULL, RC4, atau anonymous), dan sertifikat yang tidak valid atau ditandatangani dengan lemah untuk melakukan serangan Man-in-the-Middle (MitM). Pengujian ini biasanya menargetkan endpoint web server atau load balancer aplikasi di mana komunikasi terenkripsi dibangun. Eksploitasi yang berhasil memungkinkan penyerang untuk mencegat data sensitif, membajak sesi pengguna (session hijacking), atau menurunkan koneksi ke status tidak aman melalui penurunan versi protokol (protocol downgrade) atau dekripsi lalu lintas.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CRYP-01 |
| **CWE** | CWE-326 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Tinggi / Sedang* |

> *Tingkat keparahan adalah Tinggi jika data sensitif (PII, kredensial, token sesi) ditransmisikan; Sedang untuk lalu lintas informasi umum.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/01-Testing_for_Weak_Transport_Layer_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `testssl.sh`, `sslyze`, `nmap`, `OpenSSL`, `Qualys SSL Labs`

### Apakah protokol TLS/SSL yang usang atau tidak aman didukung?
- [ ] Tidak — hanya TLS 1.2 dan TLS 1.3 yang **diaktifkan**  
- [ ] Ya — TLS 1.0 atau TLS 1.1 **diaktifkan**  
- [ ] Ya — SSLv2 atau SSLv3 **diaktifkan** *(Kritis)*  

### Apakah cipher suite yang lemah atau tidak aman diterima oleh server?
- [ ] Tidak — hanya cipher yang kuat dan modern (misal, AES-GCM, ChaCha20) yang **didukung**  
- [ ] Ya — cipher dengan enkripsi lemah (misal, 3DES, RC4) atau panjang bit rendah **didukung**  
- [ ] Ya — cipher NULL, anonymous (ADH), atau export **didukung** *(Kritis)*  

### Apakah sertifikat server valid dan tepercaya?
- [ ] Ya — sertifikat valid, sesuai dengan domain, dan ditandatangani oleh **CA tepercaya**  
- [ ] Tidak — sertifikat telah **kedaluwarsa** atau menggunakan **algoritma tanda tangan yang lemah** (misal, SHA-1)  
- [ ] Tidak — sertifikat **ditandatangani sendiri (self-signed)** atau terjadi **ketidaksesuaian (mismatch)** nama domain  

### Apakah server mendukung Secure Renegotiation dan mencegah serangan Downgrade?
- [ ] Ya — Secure Renegotiation **didukung** dan TLS Fallback SCSV **diaktifkan**  
- [ ] Tidak — Secure Renegotiation **tidak didukung**, berpotensi memungkinkan injeksi MitM  
- [ ] Tidak — Server rentan terhadap serangan **POODLE** atau **ROBOT**  

### Apakah HTTP Strict Transport Security (HSTS) diimplementasikan dengan benar?

| Header | Ada | Tidak Ada |
|--------|:-------:|:-------:|
| `Strict-Transport-Security` |  |  |

- [ ] Ya — header HSTS ada dengan `max-age` yang panjang dan `includeSubDomains` **diaktifkan**  
- [ ] Ya — header HSTS ada tetapi tidak ada `includeSubDomains` atau memiliki **max-age yang pendek**  
- [ ] Tidak — header HSTS **tidak ada**

---

## WSTG-CRYP-02 — Pengujian untuk Padding Oracle

Kerentanan Padding Oracle terjadi ketika proses dekripsi aplikasi membocorkan informasi mengenai apakah padding dari ciphertext yang didekripsi valid atau tidak valid. Dengan mengamati respons side-channel ini—seperti kode status HTTP yang berbeda, pesan kesalahan spesifik, atau variasi waktu respons (response timing variations)—seorang penyerang dapat mendekripsi data tanpa mengetahui kunci enkripsi dan berpotensi mengenkripsi ulang plaintext arbitrer. Kerentanan ini biasanya muncul pada session cookie yang terenkripsi, token autentikasi, atau parameter URL yang menggunakan block cipher dalam mode Cipher Block Chaining (CBC) dengan padding PKCS#7. Eksploitasi memungkinkan hilangnya kerahasiaan secara penuh dan dapat menyebabkan kompromi integritas melalui pembuatan blok terenkripsi palsu (forged encrypted blocks).


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CRYP-02 |
| **CWE** | CWE-209 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi / Kritis* |

> *Keparahan adalah Kritis jika kerentanan terdapat pada manajemen sesi, token identitas, atau enkripsi PII.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/02-Testing_for_Padding_Oracle  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `PadBuster`, `Burp Suite (Intruder/Padding Oracle Solver)`, `pkcs7-padbuster`, `CyberChef`

### Apakah block cipher digunakan dalam mode CBC untuk data sensitif?
- [ ] Tidak — aplikasi menggunakan authenticated encryption (AES-GCM/ChaCha20-Poly1305)  
- [ ] Ya — aplikasi menggunakan block cipher tetapi padding **tidak** diperlukan (misal: Stream cipher atau mode CTR)  
- [ ] Ya — aplikasi menggunakan mode CBC dan padding **diterapkan**  

### Apakah server mengungkapkan validitas padding melalui side channels?
- [ ] Tidak — server mengembalikan pesan kesalahan yang seragam dan waktu respons yang konsisten  
- [ ] Ya — server mengembalikan kode status HTTP yang berbeda (misal: 200 vs 500) untuk kesalahan padding  
- [ ] Ya — server mengembalikan pesan kesalahan spesifik (misal: "Invalid Padding", "Decryption failed")  
- [ ] Ya — server menunjukkan perbedaan waktu yang terukur selama kegagalan dekripsi  

### Apakah dekripsi otomatis dari ciphertext memungkinkan?
- [ ] Tidak — side channels **tidak** cukup stabil untuk eksploitasi  
- [ ] Ya — ciphertext **dapat** didekripsi secara parsial (beberapa blok terakhir)  
- [ ] Ya — pemulihan plaintext secara penuh **memungkinkan** menggunakan alat seperti `PadBuster`  

### Apakah penyerang dapat melakukan bit-flipping atau memalsukan ciphertext yang valid?
- [ ] Tidak — pemeriksaan integritas (HMAC) diverifikasi **sebelum** dekripsi  
- [ ] Ya — tidak ada pemeriksaan integritas, tetapi konstruksi payload **tidak** layak  
- [ ] Ya — konstruksi ciphertext arbitrer **memungkinkan**, yang mengizinkan eskalasi hak istimewa (privilege escalation) atau injeksi data

---

## WSTG-CRYP-03 — Testing for Sensitive Information Sent Via Unencrypted Channels

Transmisi data sensitif melalui saluran yang tidak terenkripsi, seperti HTTP biasa, memaparkan informasi terhadap intersepsi dan modifikasi melalui serangan Man-in-the-Middle (MITM). Kerentanan ini biasanya terjadi ketika aplikasi web gagal menerapkan Transport Layer Security (TLS) di seluruh endpoint, terutama pada komponen legacy, formulir login, atau selama transisi awal dari HTTP ke HTTPS. Dari perspektif penyerang, hal ini memungkinkan sniffing pasif terhadap kredensial autentikasi, session tokens, dan personally identifiable information (PII) pada segmen jaringan yang terkompromi atau tidak tepercaya seperti Wi-Fi publik. Memastikan bahwa semua data sensitif dienkapsulasi dalam tunnel TLS yang kuat sangat penting untuk menjaga kerahasiaan dan integritas interaksi pengguna serta mencegah session hijacking.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CRYP-03 |
| **CWE** | CWE-319 |
| **Status Pengujian** | Belum Dilakukan |
| **Severity** | Tinggi / Sedang |

> *Severity menjadi Tinggi jika kredensial autentikasi, session identifiers, atau PII ditransmisikan dalam bentuk cleartext.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/03-Testing_for_Sensitive_Information_Sent_via_Unencrypted_Channels  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Wireshark`, `tcpdump`, `Burp Suite (Proxy/Alerts)`, `nmap`, `sslyze`, `testssl.sh`

### Apakah aplikasi mengizinkan komunikasi melalui HTTP yang tidak terenkripsi untuk endpoint sensitif?
- [ ] Tidak — seluruh lalu lintas aplikasi secara ketat dipaksa melalui HTTPS *(Paling aman)*  
- [ ] Ya — halaman non-sensitif dapat diakses melalui HTTP tetapi endpoint sensitif **tidak**  
- [ ] Ya — endpoint sensitif (login, checkout, profil) **dapat** diakses melalui HTTP yang tidak terenkripsi *(Risiko Tinggi)*  

### Apakah terdapat pengalihan global dari HTTP ke HTTPS?
- [ ] Ya — aplikasi melakukan pengalihan 301/302 ke HTTPS untuk semua permintaan HTTP  
- [ ] Ya — pengalihan hanya dilakukan untuk endpoint sensitif tertentu  
- [ ] Tidak — tidak ada pengalihan dari HTTP ke HTTPS yang **diterapkan**  

### Apakah HTTP Strict Transport Security (HSTS) diimplementasikan?
- [ ] Ya — HSTS **diaktifkan** dengan `max-age` yang lama dan menyertakan direktif `includeSubDomains` serta `preload`  
- [ ] Ya — HSTS **diaktifkan** tetapi tidak memiliki direktif `includeSubDomains` atau `preload`  
- [ ] Tidak — HSTS **tidak diaktifkan** pada server aplikasi  

### Apakah cookie sensitif dilindungi dari transmisi cleartext?

| Nama Cookie | Flag Secure Ada | Flag Secure Tidak Ada |
|-------------|:---------------:|:--------------------:|
| `SessionID` |                 |                      |
| `AuthToken` |                 |                      |
| `CSRF-Token`|                 |                      |

### Apakah aplikasi mentransmisikan data sensitif ke API pihak ketiga melalui saluran yang tidak terenkripsi?
- [ ] Tidak — semua panggilan API eksternal dilakukan melalui tunnel terenkripsi (HTTPS)  
- [ ] Ya — beberapa telemetri non-sensitif dikirim melalui HTTP  
- [ ] Ya — API keys, PII, atau kredensial **dikirim** ke pihak ketiga melalui HTTP yang tidak terenkripsi

---

## WSTG-CRYP-04 — Testing for Weak Cryptographic Primitives

Primitif kriptografi yang lemah melibatkan penggunaan algoritma yang sudah usang atau tidak aman, seperti MD5, SHA-1, DES, atau RC4, yang rentan terhadap serangan kolisi (collision attacks), analisis frekuensi, atau dekripsi brute-force. Kelemahan ini biasanya terjadi pada hashing kata sandi, enkripsi data saat diam (data encryption at rest), pembuatan token sesi, atau verifikasi tanda tangan digital di dalam backend aplikasi. Dari sudut pandang penyerang, mengidentifikasi primitif ini memungkinkan kompromi terhadap integritas dan kerahasiaan data, seperti memecahkan hash yang dicegat untuk mendapatkan kata sandi teks biasa (cleartext) atau memalsukan token autentikasi. Aplikasi modern seharusnya menggunakan primitif yang sesuai standar industri, tahan kolisi (collision-resistant), dan memiliki entropi tinggi (high-entropy) seperti Argon2, Bcrypt, atau AES-GCM untuk memastikan perlindungan yang kuat terhadap kriptanalisis.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CRYP-04 |
| **CWE** | CWE-327 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/04-Testing_for_Weak_Cryptographic_Primitives  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `hashcat`, `John the Ripper`, `Burp Suite`, `CyberChef`, `OpenSSL`, `nmap (ssl-enum-ciphers)`

### Apakah algoritma hashing yang sudah usang seperti MD5 atau SHA-1 digunakan untuk penyimpanan data sensitif?
- [ ] Tidak — aplikasi menggunakan algoritma modern yang tahan kolisi seperti Argon2 atau Bcrypt  
- [ ] Ya — algoritma usang digunakan tetapi **hanya** untuk pemeriksaan integritas yang tidak sensitif terhadap keamanan  
- [ ] Ya — algoritma usang **digunakan** untuk kata sandi atau token sensitif *(Tinggi)*  

### Apakah cipher enkripsi simetris yang lemah seperti DES, 3DES, atau RC4 saat ini diterapkan?
- [ ] Tidak — AES (128-bit atau lebih tinggi) dalam mode aman seperti GCM atau CBC digunakan  
- [ ] Ya — cipher lama **diaktifkan** untuk kompatibilitas mundur tetapi **bukan** sebagai default  
- [ ] Ya — cipher lama adalah metode utama untuk melindungi data saat diam (at rest) atau saat transit  

### Apakah aplikasi mengimplementasikan logika kriptografi "roll-your-own" atau non-standar?
- [ ] Tidak — aplikasi secara ketat mematuhi pustaka kriptografi standar yang telah ditinjau sejawat (peer-reviewed)  
- [ ] Ya — terdapat wrapper khusus tetapi primitif yang mendasarinya **adalah** standar industri  
- [ ] Ya — algoritma kriptografi milik sendiri (proprietary) atau logika khusus **diterapkan** pada data sensitif *(Kritis)*  

### Apakah cryptographic salt dan initialization vector (IV) dikelola sesuai dengan praktik terbaik?
- [ ] Ya — salt unik per pengguna dan IV tidak dapat diprediksi untuk setiap operasi  
- [ ] Ya — salt digunakan tetapi **tidak** unik di seluruh rekaman data  
- [ ] Tidak — salt bersifat statis, hardcoded, atau tidak ada, dan IV **digunakan kembali** di beberapa sesi  

### Apakah panjang bit (bit-length) dari kunci asimetris (RSA, Diffie-Hellman) mencukupi untuk standar modern?
- [ ] Ya — kunci RSA/DH adalah 2048 bit atau lebih tinggi, atau Elliptic Curve (ECC) digunakan  
- [ ] Tidak — kunci kurang dari 2048 bit tetapi bypass **tidak** sepele (trivial)  
- [ ] Tidak — kunci 1024 bit atau lebih kecil, membuatnya **rentan** terhadap faktorisasi atau serangan komputasi

---

## WSTG-BUSL-01 — Pengujian Validasi Data Logika Bisnis (Test Business Logic Data Validation)

Pengujian validasi data logika bisnis berfokus pada kemampuan aplikasi untuk mengidentifikasi dan menolak data yang benar secara sintaksis tetapi tidak valid secara logika dalam konteks domain bisnis. Penyerang mengeksploitasi celah ini dengan mengirimkan nilai yang tidak terduga—seperti kuantitas negatif dalam keranjang belanja, tanggal di masa lalu untuk acara mendatang, atau integer yang sangat besar—untuk melewati batasan dan memanipulasi status aplikasi. Kerentanan ini sering ditemukan dalam alur kerja multi-langkah (multi-step workflows), proses checkout, dan modul manajemen akun di mana logika sisi server (server-side logic) gagal memverifikasi integritas kontekstual dari data yang diberikan pengguna. Eksploitasi yang berhasil dapat menyebabkan kerugian finansial, kompromi integritas, dan akses tidak sah ke fitur atau data terbatas.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-BUSL-01 |
| **CWE** | CWE-20 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan (Severity)** | Tinggi / Sedang |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/01-Test_Business_Logic_Data_Validation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**Alat:** `Burp Suite (Repeater/Intruder)`, `Postman`, `Python (Requests)`, `OWASP ZAP`

### Apakah aplikasi menerapkan batasan rentang logika pada input numerik?
- [ ] Ya — aplikasi menolak nilai negatif, nol, atau nilai yang terlalu besar dengan benar jika tidak sesuai *(Paling aman)*  
- [ ] Ya — aplikasi menolak beberapa rentang yang tidak valid tetapi **rentan** terhadap integer overflow atau bypass batasan (boundary bypass)  
- [ ] No — aplikasi menerima nilai numerik apa pun terlepas dari konteks bisnis *(Kritis)*  

### Apakah pemeriksaan validasi sisi server konsisten di semua lapisan aplikasi?
- [ ] Ya — validasi diterapkan secara konsisten di lapisan API/layanan dan basis data  
- [ ] Ya — validasi **diterapkan** pada UI/Frontend tetapi bypass melalui permintaan API langsung **dimungkinkan**  
- [ ] No — validasi **tidak diterapkan** atau tidak konsisten, memungkinkan terjadinya korupsi data logis  

### Dapatkah logika bisnis disubversi dengan memanipulasi data dalam alur kerja multi-langkah (multi-step workflows)?
- [ ] No — status (state) dikelola dengan aman di server dan data dari langkah sebelumnya **tidak dapat** dirusak  
- [ ] Ya — validasi terjadi pada langkah awal tetapi pengiriman akhir **tidak** memverifikasi ulang integritas data  
- [ ] Ya — bypass alur kerja **dimungkinkan** dengan melewati langkah-langkah atau mengirimkan data yang tidak berurutan  

### Apakah aplikasi menangani data "kasus tepi" (edge case) yang melewati batasan bisnis dengan benar?
- [ ] Ya — batasan bersifat kuat dan menangani input yang tidak biasa tetapi valid secara sintaksis (misalnya, tahun kabisat, pergeseran zona waktu)  
- [ ] Ya — beberapa kasus tepi ditangani tetapi kombinasi tertentu **dapat** menyebabkan perilaku yang tidak terduga  
- [ ] No — kasus tepi secara langsung menyebabkan kegagalan logika, seperti pembelian gratis atau perubahan status yang tidak sah

---

## WSTG-BUSL-02 — Menguji Kemampuan Memalsukan Permintaan (Test Ability to Forge Requests)

Memalsukan permintaan (forging requests) dalam konteks logika bisnis (business logic) melibatkan manipulasi parameter yang ditentukan aplikasi untuk melakukan tindakan di luar alur kerja (workflow) atau tingkat otorisasi (authorization) yang dimaksudkan. Penyerang menargetkan proses multi-tahap, seperti sistem checkout atau pendaftaran akun, di mana status (state) dipertahankan melalui parameter sisi klien (client-side) yang gagal divalidasi ulang oleh server terhadap sumber backend yang tepercaya. Dengan mengubah bidang tersembunyi (hidden fields), nilai JSON, atau parameter REST seperti harga (prices), jumlah (quantities), atau bendera status internal (internal status flags), penyerang dapat melewati persyaratan pembayaran, meningkatkan hak istimewa (escalate privileges), atau memicu perubahan status yang tidak diinginkan. Kerentanan ini biasanya muncul pada formulir web stateful atau API di mana server secara implisit memercayai representasi klien tentang status bisnis selama transaksi.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-BUSL-02 |
| **CWE** | CWE-602 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi / Kritis* |

> *Tingkat keparahan menjadi Kritis jika permintaan yang dipalsukan memungkinkan kerugian finansial, tindakan administratif yang tidak sah, atau pengambilalihan akun secara penuh (full account takeover).

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/02-Test_Ability_to_Forge_Requests  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite (Proxy, Repeater, Intruder)`, `Postman`, `OWASP ZAP`, `CyberChef`

### Apakah aplikasi memvalidasi parameter transaksional terhadap "source of truth" di sisi server?
- [ ] Ya — semua parameter sensitif (harga, peran, ID) divalidasi terhadap database dan bypass **tidak dimungkinkan**  
- [ ] Ya — validasi diterapkan tetapi dapat dilewati melalui parameter pollution atau pengkodean alternatif  
- [ ] No — aplikasi memercayai nilai sisi klien untuk menentukan biaya atau hasil transaksi *(Kritis)*  

### Apakah nilai-nilai kritis bisnis (misalnya, jumlah, nominal) dapat diubah menjadi nilai yang tidak sah?
- [ ] Tidak — nilai negatif, nilai nol, atau nilai yang sangat tinggi ditolak oleh server  
- [ ] Ya — server menerima nilai yang dimodifikasi tetapi hanya dalam rentang terbatas  
- [ ] Ya — nilai negatif atau nol **dapat** dikirimkan, yang menyebabkan bypass finansial atau logika  

### Apakah bidang tersembunyi atau parameter API digunakan untuk mempertahankan status di seluruh proses multi-tahap?
- [ ] Tidak — status dipertahankan secara aman melalui sesi sisi server (server-side sessions) atau token yang ditandatangani  
- [ ] Ya — status diteruskan melalui klien tetapi pemeriksaan integritas (HMAC/Signatures) **diaktifkan** dan kuat  
- [ ] Ya — status diteruskan melalui klien dan pemeriksaan integritas **dinonaktifkan** atau dapat dihapus  

### Apakah mungkin untuk memalsukan permintaan yang melewatkan langkah-langkah perantara wajib dalam suatu alur kerja (workflow)?
- [ ] Tidak — server memaksakan urutan operasi yang ketat untuk setiap transaksi  
- [ ] Ya — langkah-langkah **dapat** dilewati tetapi tindakan akhir tetap memerlukan data yang valid dari langkah-langkah sebelumnya  
- [ ] Ya — permintaan "submit" atau "complete" akhir **dapat** dipalsukan secara langsung untuk melewati langkah otorisasi atau pembayaran

---

## WSTG-BUSL-03 — Menguji Pemeriksaan Integritas

Pengujian pemeriksaan integritas berfokus pada verifikasi bahwa aplikasi memastikan data tetap tidak berubah dan dapat dipercaya saat berpindah melalui berbagai proses bisnis atau antara klien dan server. Penyerang menargetkan logika ini dengan memanipulasi parameter kritis—seperti harga produk, jumlah, hak istimewa pengguna, atau status transaksi—di dalam HTTP request, hidden fields, atau cookies. Jika aplikasi kekurangan verifikasi server-side atau kontrol integritas yang kuat seperti Hashing atau HMACs, penyerang dapat memanipulasi nilai-nilai ini untuk melakukan penipuan, melakukan eskalasi hak istimewa (privilege escalation), atau melewati batasan bisnis yang dimaksudkan. Pengujian ini sangat penting untuk aplikasi apa pun yang menangani transaksi keuangan, transisi status yang sensitif, atau alur kerja multi-tahap di mana output dari satu tahap berfungsi sebagai input tepercaya untuk tahap berikutnya.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-BUSL-03 |
| **CWE** | CWE-353 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Sedang / Tinggi* |

> *Tingkat Keparahan menjadi Tinggi jika kegagalan integritas memungkinkan pencurian finansial, eskalasi hak istimewa yang tidak sah, atau modifikasi catatan persisten.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/03-Test_Integrity_Checks  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite (Proxy/Repeater)`, `CyberChef`, `Postman`, `Python (Requests)`

### Apakah parameter bisnis yang sensitif terpapar pada manipulasi client-side?
- [ ] Tidak — nilai-nilai sensitif dikelola secara eksklusif di sisi server-side  
- [ ] Ya — nilai-nilai seperti harga, jumlah, atau flag status **terpapar** tetapi terenkripsi  
- [ ] Ya — nilai-nilai **terpapar** dalam teks biasa (plain text) di dalam hidden fields, parameter URL, atau cookies  

### Apakah mekanisme integritas (misalnya, HMAC, Checksum) diterapkan pada parameter yang dikendalikan klien?
- [ ] Ya — pemeriksaan integritas yang kuat **diterapkan** dan diverifikasi pada setiap request  
- [ ] Ya — pemeriksaan integritas **diterapkan** tetapi menggunakan algoritma yang lemah/dapat diprediksi atau secret yang di-hardcode  
- [ ] Tidak — tidak ada pemeriksaan integritas yang **diterapkan** pada parameter sensitif  

### Apakah pemeriksaan integritas dapat dilewati atau dihitung ulang oleh penyerang?
- [ ] Tidak — verifikasi tanda tangan (signature) ditegakkan secara ketat dan bypass **tidak memungkinkan**  
- [ ] Ya — verifikasi signature dapat dilewati dengan menghilangkan parameter signature  
- [ ] Ya — signature **dapat** dihitung ulang karena kunci rahasia (secret key) atau logika terpapar/lemah  

### Apakah aplikasi melakukan validasi ulang backend terhadap sumber data tepercaya?
- [ ] Ya — server memvalidasi ulang semua nilai terhadap database dan bypass **tidak memungkinkan**  
- [ ] Ya — server melakukan validasi parsial tetapi bypass **dimungkinkan** melalui kasus tepi (edge cases) tertentu  
- [ ] Tidak — server mempercayai data yang diberikan klien tanpa verifikasi backend *(Kritis)*  

### Apakah mungkin untuk memanipulasi status (state) dari proses multi-tahap?
- [ ] Tidak — status sesi (session state) dikelola di sisi server dan tidak dapat dimanipulasi  
- [ ] Ya — informasi status dikirim melalui klien dan manipulasi **dimungkinkan** untuk melewati langkah-langkah atau mengulangi tindakan

---

## WSTG-BUSL-04 — Test for Process Timing

Serangan timing proses (Process timing attacks) melibatkan pengukuran waktu yang dibutuhkan aplikasi untuk memproses permintaan tertentu guna menyimpulkan informasi sensitif tentang status internal atau keberadaan data. Dengan menganalisis perbedaan waktu respons pada tingkat milidetik—yang sering kali disebabkan oleh logika kondisional early-exit, pencarian basis data, atau perbandingan kriptografi non-constant-time—penyerang dapat melakukan analisis side-channel untuk mengenumerasi nama pengguna yang valid, memverifikasi fragmen kata sandi, atau mengonfirmasi keberadaan catatan tertentu. Kerentanan ini biasanya terjadi pada endpoint autentikasi, formulir reset kata sandi, dan filter API yang berat sumber daya di mana logika backend bercabang berdasarkan validitas input. Dari perspektif penyerang, perbedaan waktu ini berfungsi sebagai "boolean oracle" yang mengungkapkan kebenaran tentang data backend bahkan ketika aplikasi mengembalikan pesan kesalahan umum yang identik kepada pengguna.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-BUSL-04 |
| **CWE** | CWE-208 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Medium |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/04-Test_for_Process_Timing  
* https://hacktricks.wiki/en/pentesting-web/timing-attacks.html  
* https://portswigger.net/web-security/race-conditions  

**Alat:** `Burp Suite (Timing Advisor / Logger++)`, `ffuf`, `curl`, `Python (time module)`, `THC-Hydra`

### Apakah aplikasi menunjukkan waktu respons yang bervariasi untuk permintaan sumber daya yang valid vs. tidak valid?
- [ ] Tidak — waktu respons identik terlepas dari validitas input  
- [ ] Ya — terdapat perbedaan waktu yang sangat kecil namun **tidak** signifikan secara statistik  
- [ ] Ya — perbedaan waktu yang konsisten dapat **diamati** dan menyediakan oracle yang andal  

### Apakah fungsi perbandingan waktu-konstan (constant-time comparison functions) atau penundaan buatan diimplementasikan untuk menormalkan waktu respons?
- [ ] Ya — algoritma waktu-konstan **diterapkan** pada semua operasi perbandingan sensitif *(Paling aman)*  
- [ ] Ya — penundaan buatan atau jitter **diaktifkan**, tetapi logika yang mendasarinya tetap non-constant-time  
- [ ] Tidak — tidak ada mitigasi timing yang **diterapkan**, memungkinkan pengukuran langsung pada cabang logika  

### Dapatkah penyerang menggunakan analisis statistik untuk mengisolasi sinyal timing dari noise jaringan?
- [ ] Tidak — jitter jaringan dan noise aplikasi membuat analisis timing **tidak mungkin dilakukan**  
- [ ] Ya — dengan ukuran sampel yang cukup, sinyal timing **dapat** diisolasi dari noise  
- [ ] Ya — selisih timing cukup besar sehingga normalisasi statistik **tidak** diperlukan  

### Apakah enumerasi akun (account enumeration) dimungkinkan melalui fungsionalitas login atau reset kata sandi?
- [ ] Tidak — keberadaan akun tidak dapat ditentukan melalui timing  
- [ ] Ya — perbedaan timing memungkinkan penyerang jarak jauh untuk **mengenumerasi** nama pengguna atau email yang valid  

### Apakah operasi yang intensif sumber daya (misalnya, pemrosesan file, pencarian kompleks) membocorkan keberadaan data melalui timing?
- [ ] Tidak — konsumsi sumber daya seragam terlepas dari apakah sebuah catatan ditemukan atau tidak  
- [ ] Ya — keberadaan catatan sensitif **dapat** disimpulkan dengan mengukur durasi pemrosesan backend

---

## WSTG-BUSL-05 — Menguji Batas Berapa Kali Fungsi Dapat Digunakan

Pengujian ini mengevaluasi apakah aplikasi menerapkan batasan logika bisnis (business logic) pada frekuensi dan jumlah total berapa kali tindakan atau fungsi tertentu dapat dijalankan oleh satu pengguna, sesi, atau alamat IP. Kegagalan dalam menerapkan batasan ini memungkinkan penyerang untuk menyalahgunakan fungsi bernilai tinggi—seperti pengiriman SMS OTP, memicu email pengaturan ulang kata sandi (password reset), menerapkan kode diskon, atau melakukan ekspor data yang berat—yang menyebabkan kerugian finansial, kehabisan sumber daya (resource exhaustion), atau kerusakan reputasi. Kerentanan ini biasanya ditemukan pada endpoint transaksional atau fitur komunikasi dan dieksploitasi melalui skrip otomatis yang mengulangi tindakan jauh melampaui ambang batas bisnis yang dimaksudkan. Dari perspektif penyerang, ini adalah target utama untuk SMS pumping, mail bombing, atau alur kerja Brute Force yang tidak memiliki Rate Limiting atau pembatasan berbasis jumlah yang eksplisit.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-BUSL-05 |
| **CWE** | CWE-799 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Sedang / Tinggi* |

> *Keparahan menjadi Tinggi jika fungsi tersebut melibatkan transaksi keuangan, biaya SMS, atau berdampak pada ketersediaan sistem secara keseluruhan.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/05-Test_Number_of_Times_a_Function_Can_Be_Used_Limits  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite (Intruder/Repeater)`, `Turbo Intruder`, `Python (requests)`, `OWASP ZAP`

### Apakah aplikasi menentukan dan menerapkan batasan pada fungsi bisnis yang sensitif?
- [ ] Ya — batasan diterapkan secara ketat dan bypass **tidak mungkin dilakukan**  
- [ ] Ya — batasan diterapkan tetapi terlalu tinggi untuk mencegah penyalahgunaan  
- [ ] Tidak — tidak ada batasan yang diterapkan pada eksekusi fungsi  

### Apakah pembatasan laju (rate limits) dan penghitung penggunaan diterapkan di sisi server?
- [ ] Ya — penerapan dilakukan secara ketat di sisi server (server-side) dan **tidak dapat** dimanipulasi  
- [ ] Tidak — penerapan bergantung pada logika sisi klien (client-side), misalnya JavaScript atau field tersembunyi  
- [ ] Tidak — tidak ada penerapan batasan di level mana pun  

### Dapatkah batasan penggunaan dilewati melalui manipulasi header atau sesi?
- [ ] Tidak — bypass melalui `X-Forwarded-For`, `Origin`, atau rotasi sesi **tidak mungkin dilakukan**  
- [ ] Ya — batasan **dapat** dilewati dengan melakukan spoofing pada header IP atau menghapus cookie  
- [ ] Ya — batasan **dapat** dilewati dengan membuat banyak akun atau sesi  

### Apakah melampaui batas memicu respons pertahanan yang tepat?
- [ ] Ya — aplikasi mengembalikan `429 Too Many Requests` dan mencatat log kejadian tersebut *(Paling aman)*  
- [ ] Ya — aplikasi mengembalikan kesalahan tetapi **tidak** mencatat log potensi penyalahgunaan  
- [ ] Tidak — aplikasi terus memproses permintaan tetapi mengabaikan output-nya  
- [ ] Tidak — aplikasi tidak memberikan respons atau mengalami crash di bawah beban tinggi  

### Apakah dampak dari penyalahgunaan fungsi tersebut signifikan bagi bisnis?
- [ ] Tidak — penyalahgunaan fungsi memiliki biaya atau dampak sumber daya yang sangat kecil  
- [ ] Ya — penyalahgunaan **dapat** menyebabkan konsumsi sumber daya minor atau gangguan pada pengguna  
- [ ] Ya — penyalahgunaan **dapat** menyebabkan biaya finansial yang signifikan (misalnya biaya SMS) atau penolakan layanan (Denial of Service) *(Kritis)*

---

## WSTG-BUSL-06 — Testing for the Circumvention of Work Flows

Pengabaian alur kerja (*Workflow circumvention*) melibatkan manipulasi logika aplikasi untuk melewati urutan atau prasyarat yang dimaksudkan dalam proses multitahap. Penyerang mengidentifikasi endpoint sensitif—seperti konfirmasi pembayaran, persetujuan administratif, atau layar penyelesaian MFA—dan mencoba mengaksesnya secara langsung, dengan melewatkan langkah-langkah perantara yang wajib. Kerentanan ini terjadi ketika logika sisi server gagal mempertahankan *state machine* yang kuat, melainkan mengandalkan asumsi bahwa pengguna mengikuti jalur yang digerakkan oleh antarmuka pengguna (UI). Eksploitasi yang berhasil dapat menyebabkan kegagalan logika bisnis yang kritis, termasuk transaksi yang tidak sah, eskalasi hak istimewa (*privilege escalation*), dan pengabaian mekanisme verifikasi identitas.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-BUSL-06 |
| **CWE** | CWE-841 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Sedang / Tinggi* |

> *Keparahan menjadi Tinggi jika pengabaian memungkinkan transaksi keuangan yang tidak sah, eskalasi hak istimewa, atau akses administratif.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/06-Testing_for_the_Circumvention_of_Work_Flows  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**Alat:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Postman`, `Caido`

### Apakah aplikasi berisi proses bisnis multitahap?
- [ ] Tidak — aplikasi hanya terdiri dari tindakan permintaan tunggal  
- [ ] Ya — proses multitahap (misalnya, keranjang belanja, formulir wizard, registrasi) **tersedia**  

### Apakah urutan langkah-langkah ditegakkan secara ketat oleh server?
- [ ] Ya — pelacakan status (*state tracking*) sisi server memastikan langkah-langkah **tidak dapat** dilewati  
- [ ] Ya — kontrol sisi klien menyarankan sebuah urutan, tetapi server **tidak** memvalidasi progresnya  
- [ ] Tidak — aplikasi **tidak** menegakkan urutan operasi tertentu apa pun  

### Dapatkah penyerang mencapai tahap akhir dari sebuah proses dengan langsung meminta URL atau endpoint terminal?
- [ ] Tidak — akses langsung ke langkah terminal **tidak dimungkinkan** tanpa penyelesaian sebelumnya  
- [ ] Ya — langkah terminal (misalnya, `/api/v1/checkout/complete`) **dapat** dijangkau melalui permintaan langsung  

### Apakah aplikasi memverifikasi bahwa data prasyarat dari langkah sebelumnya ada dan valid?
- [ ] Ya — server memvalidasi semua data langkah sebelumnya sebelum melanjutkan  
- [ ] Tidak — server **rentan** terhadap "pelewatkan" langkah-langkah di mana data diharapkan tetapi tidak diverifikasi  

### Dapatkah kontrol keamanan seperti MFA atau verifikasi identitas diabaikan dengan memanipulasi alur kerja?
- [ ] Tidak — kontrol kritis **tidak dapat** diabaikan melalui manipulasi alur  
- [ ] Ya — pengabaian MFA atau pemeriksaan identitas **dimungkinkan** dengan melompat ke langkah pasca-verifikasi

---

## WSTG-BUSL-07 — Test Defenses Against Application Misuse

Pertahanan terhadap penyalahgunaan aplikasi mengevaluasi kemampuan aplikasi untuk mengidentifikasi, mencatat (log), dan menanggapi perilaku pengguna yang anomali yang menyimpang dari logika bisnis yang dimaksudkan. Berbeda dengan keamanan berbasis tanda tangan (signature-based security) tradisional, pertahanan ini berfokus pada deteksi pola seperti permintaan bertubi-tubi (rapid-fire requests), Credential Stuffing, atau enumerasi sumber daya secara berurutan yang menandakan penyalahgunaan otomatis. Dari sudut pandang penyerang, pengujian ini menentukan ketahanan aplikasi terhadap otomatisasi dan serangan "low and slow" yang dirancang untuk mengeksfiltrasi data atau menguras sumber daya tanpa memicu peringatan keamanan standar. Kegagalan dalam menerapkan pertahanan ini sering kali mengakibatkan pengerukan data (data scraping) secara masif, pengambilalihan akun (Account Takeover), atau Denial of Service (DoS) melalui fitur aplikasi yang sah namun intensif sumber daya.


| Kolom | Nilai |
|---|---|
| **WSTG ID** | WSTG-BUSL-07 |
| **CWE** | CWE-799 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Medium / High |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/07-Test_Defenses_Against_Application_Misuse  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite (Intruder/Turbo Intruder)`, `OWASP ZAP`, `Python (Requests/Selenium)`, `Caido`, `K6`

### Apakah mekanisme rate-limiting atau throttling diterapkan pada endpoint sensitif?
- [ ] Ya — Rate Limiting yang ketat **diterapkan** dan ditegakkan di semua endpoint sensitif *(Paling aman)*  
- [ ] Ya — Rate Limiting **diterapkan** tetapi dapat dilewati melalui rotasi IP atau manipulasi header  
- [ ] Ya — Rate Limiting **diterapkan** hanya pada endpoint autentikasi, membiarkan yang lain terpapar  
- [ ] No — Tidak ada Rate Limiting atau throttling yang **diterapkan** pada fitur aplikasi apa pun  

### Apakah aplikasi mendeteksi dan memblokir pola interaksi otomatis?
- [ ] Ya — analisis perilaku atau CAPTCHA **diaktifkan** dan secara efektif memblokir bot  
- [ ] Ya — anti-otomatisasi **diaktifkan** tetapi bypass **mungkin dilakukan** menggunakan headless browser atau session cycling  
- [ ] No — aplikasi **tidak dapat** membedakan antara pengguna manusia dan skrip otomatis  

### Apakah perilaku anomali dicatat dan diikuti dengan respons defensif?
- [ ] Ya — penyalahgunaan memicu pemblokiran otomatis dan memperingatkan tim keamanan  
- [ ] Ya — penyalahgunaan dicatat untuk audit tetapi **tidak ada** tindakan defensif otomatis yang diambil  
- [ ] No — aplikasi **tidak mencatat** atau menanggapi pola aktivitas yang tidak biasa  

### Apakah pertahanan dapat dilewati dengan memanipulasi pengidentifikasi sisi klien atau header?
- [ ] No — pertahanan bergantung pada state di sisi server dan telemetri yang terverifikasi  
- [ ] Ya — kontrol **dapat** dilewati dengan menghapus cookie atau merotasi string `User-Agent`  
- [ ] Ya — kontrol **dapat** dilewati dengan menyisipkan header seperti `X-Forwarded-For` atau `X-Real-IP`

---

## WSTG-BUSL-08 — Uji Unggah Jenis File yang Tidak Terduga

Mengunggah jenis file yang tidak terduga memungkinkan penyerang untuk melewati batasan logika bisnis dengan mengirimkan file yang tidak dimaksudkan untuk diproses oleh aplikasi, yang berpotensi menyebabkan eksekusi kode jarak jauh (Remote Code Execution), Cross-Site Scripting (XSS), atau Denial of Service (DoS). Penyerang menguji kerentanan ini dengan memodifikasi ekstensi file, MIME types, dan magic bytes untuk menipu logika validasi sisi server (server-side) agar menerima konten berbahaya. Hal ini biasanya terjadi pada unggahan foto profil, sistem manajemen dokumen, atau lampiran tiket dukungan di mana validasi yang tidak memadai tersedia pada bagian back-end. Eksploitasi yang berhasil dapat membahayakan server host jika file yang diunggah dieksekusi, atau menyebabkan serangan sisi klien (client-side) jika file tersebut disajikan kembali kepada pengguna lain dengan Content-Type yang salah.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-BUSL-08 |
| **CWE** | CWE-434 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi / Kritis |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/08-Test_Upload_of_Unexpected_File_Types  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**Alat:** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`

### Apakah aplikasi membatasi unggahan file ke set ekstensi tertentu?
- [ ] Ya — whitelist ekstensi yang ketat diterapkan dan bypass **tidak dimungkinkan**  
- [ ] Ya — whitelist ekstensi tersedia tetapi bypass **dimungkinkan** melalui ekstensi ganda atau null bytes  
- [ ] Tidak — ekstensi file apa pun **dapat** diunggah  

### Apakah konten file divalidasi lebih dari sekadar ekstensi file?
- [ ] Ya — logika sisi server memverifikasi Magic Bytes dan melakukan inspeksi konten secara mendalam  
- [ ] Ya — logika sisi server memverifikasi MIME type hanya melalui header `Content-Type`  
- [ ] Tidak — tidak ada validasi konten yang **diterapkan** setelah pemeriksaan ekstensi  

### Dapatkah penyerang mengeksekusi kode dengan mengunggah web shell atau skrip?
- [ ] Tidak — file yang diunggah disimpan dalam direktori non-executable atau bucket penyimpanan (S3/Azure Blob)  
- [ ] Ya — file disimpan dalam direktori yang dapat diakses web tetapi eksekusi **dinonaktifkan** melalui konfigurasi server  
- [ ] Ya — skrip berbahaya yang diunggah **dapat** dieksekusi langsung melalui jalur URL yang dapat diprediksi *(Kritis)*  

### Apakah serangan sisi klien seperti XSS dimungkinkan melalui jenis file yang diunggah?
- [ ] Tidak — file disajikan dengan `Content-Disposition: attachment` dan `X-Content-Type-Options: nosniff`  
- [ ] Ya — file SVG atau HTML **dapat** diunggah dan dirender di browser, sehingga menyebabkan XSS  
- [ ] Ya — metadata gambar (EXIF) **diproses** dan direfleksikan dalam UI tanpa sanitasi (sanitization)

---

## WSTG-BUSL-09 — Test Upload of Malicious Files

Fungsionalitas unggah berkas memungkinkan pengguna untuk mengirimkan data ke server, yang memberikan vektor langsung untuk memasukkan konten berbahaya ke dalam lingkungan aplikasi. Penyerang mengeksploitasi logika validasi yang lemah untuk mengunggah Web Shell, Malware, atau berkas yang dirancang khusus—seperti SVG atau HTML—untuk mencapai Remote Code Execution (RCE) atau Cross-Site Scripting (XSS). Kerentanan ini biasanya ditemukan pada fitur-fitur seperti pengaturan profil, sistem manajemen dokumen, dan penanganan lampiran di mana server gagal memverifikasi tipe berkas, konten, dan izin eksekusi dengan benar. Eksploitasi yang berhasil sering kali menyebabkan kompromi sistem secara penuh, eksfiltrasi data, atau penggunaan server sebagai titik tumpu (pivot point) untuk serangan jaringan internal.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-BUSL-09 |
| **CWE** | CWE-434 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Kerentanan** | Tinggi / Kritis |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/09-Test_Upload_of_Malicious_Files  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**Alat:** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`, `Weevely`

### Apakah aplikasi menyediakan fungsionalitas untuk mengunggah berkas?
- [ ] Tidak — tidak ada fungsionalitas unggah berkas
- [ ] Ya — fungsionalitas tersedia untuk pengguna terautentikasi
- [ ] Ya — fungsionalitas tersedia untuk pengguna **tanpa autentikasi** *(Kritis)*

### Apakah validasi ekstensi berkas ditegakkan di sisi server?
- [ ] Ya — allowlist ekstensi yang ketat **diterapkan** dan bypass **tidak dimungkinkan**
- [ ] Ya — allowlist digunakan tetapi bypass **dimungkinkan** melalui kepekaan huruf (case sensitivity) atau ekstensi ganda
- [ ] Ya — blocklist digunakan, yang **dapat** dilompati dengan ekstensi alternatif (contoh: `.phtml`, `.asa`)
- [ ] Tidak — tidak ada validasi ekstensi yang **diterapkan** di sisi server

### Apakah konten berkas divalidasi selain dari ekstensi atau tipe MIME?
- [ ] Ya — aplikasi memverifikasi magic bytes dan melakukan inspeksi konten yang mendalam
- [ ] Ya — aplikasi hanya memeriksa header `Content-Type`, yang sangat mudah **dipalsukan** (spoofed)
- [ ] Tidak — aplikasi sepenuhnya bergantung pada ekstensi berkas untuk validasi

### Apakah berkas yang diunggah disimpan dalam direktori dengan izin eksekusi?
- [ ] Tidak — berkas disimpan pada layanan penyimpanan khusus (seperti S3) atau direktori non-executable
- [ ] Ya — berkas disimpan di server web tetapi eksekusi **dinonaktifkan** melalui konfigurasi
- [ ] Ya — berkas disimpan dalam direktori yang dapat diakses melalui web dan eksekusi **diaktifkan** *(Kritis)*

### Apakah filter keamanan dapat dilompati melalui manipulasi nama berkas?
- [ ] Tidak — nama berkas disanitasi atau dihasilkan ulang secara acak oleh server
- [ ] Ya — filter **dapat** dilompati menggunakan null bytes (contoh: `shell.php%00.jpg`)
- [ ] Ya — karakter Path Traversal (contoh: `../../`) **dapat** digunakan untuk mengunggah berkas ke direktori sembarang

---

## WSTG-BUSL-10 — Menguji Fungsionalitas Pembayaran

Pengujian fungsionalitas pembayaran melibatkan identifikasi cacat logika bisnis (business logic flaws) yang memungkinkan penyerang melewati gateway pembayaran (payment gateways), memanipulasi total pesanan, atau memalsukan sinyal transaksi yang berhasil. Kerentanan ini biasanya terletak pada komunikasi antara aplikasi, browser klien, dan pemroses pembayaran pihak ketiga seperti Stripe, PayPal, atau sistem buku besar internal. Penyerang mungkin mencoba memodifikasi parameter harga saat dalam pengiriman (in transit), mengirimkan jumlah negatif untuk mengurangi total biaya, atau melakukan replay pada callback transaksi yang berhasil untuk memenuhi pesanan tanpa pembayaran yang sebenarnya. Eksploitasi yang berhasil menyebabkan kerugian finansial langsung, ketidaksesuaian inventaris, dan potensi kompromi pada integritas e-commerce aplikasi.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-BUSL-10 |
| **CWE** | CWE-840 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi / Kritis |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/10-Test-Payment-Functionality  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite`, `Turbo Intruder`, `Postman`, `Hackvertor`

### Apakah jumlah transaksi divalidasi di sisi server sebelum diproses?
- [ ] Ya — jumlah divalidasi secara ketat terhadap database produk di sisi server *(Paling aman)*  
- [ ] Ya — jumlah divalidasi tetapi bypass logika **mungkin terjadi** melalui parameter pollution atau penggantian mata uang  
- [ ] Tidak — jumlah transaksi **dapat** dimodifikasi dalam permintaan sisi klien dan diterima oleh backend  

### Apakah jumlah item dapat dimanipulasi sehingga menghasilkan total nol atau negatif?
- [ ] Tidak — jumlah negatif atau nol ditolak oleh logika validasi aplikasi  
- [ ] Ya — jumlah negatif diterima di keranjang tetapi total diperbaiki saat checkout  
- [ ] Ya — jumlah negatif **dapat** digunakan untuk mengurangi total pesanan secara keseluruhan atau mendapatkan kredit *(Kritis)*  

### Apakah aplikasi menangani callback atau webhook gateway pembayaran dengan aman?
- [ ] Ya — callback memerlukan tanda tangan kriptografi valid yang **tidak dapat** dipalsukan  
- [ ] Ya — tanda tangan diperiksa tetapi rahasia (secret) lemah atau verifikasi **dapat** dilewati  
- [ ] Tidak — aplikasi mengandalkan callback yang tidak terautentikasi atau tidak terverifikasi untuk mengonfirmasi status pembayaran  

### Apakah aplikasi rentan terhadap race conditions selama fase checkout atau pembayaran?
- [ ] Tidak — integritas transaksional dan mekanisme penguncian database **diaktifkan**  
- [ ] Ya — race conditions **mungkin terjadi** tetapi tidak berdampak pada harga akhir atau pemenuhan pesanan  
- [ ] Ya — permintaan konkuren **dapat** menyebabkan beberapa pemenuhan pesanan untuk satu pembayaran atau "double-spending" pada kartu hadiah (gift cards)  

### Apakah kode diskon atau poin loyalitas dapat dimanipulasi atau digunakan kembali?
- [ ] Tidak — kode divalidasi untuk satu kali penggunaan dan batasan logika **diterapkan**  
- [ ] Ya — kode dapat digunakan kembali atau diterapkan pada item yang tidak memenuhi syarat, tetapi dampaknya terbatas  
- [ ] Ya — penyerang **dapat** menerapkan beberapa kode yang bertentangan atau melewati batas kedaluwarsa dan penggunaan

---

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

## WSTG-CLNT-04 — Pengujian Client-Side URL Redirect

Client-side URL redirection terjadi ketika aplikasi web menerima input yang dikontrol pengguna—biasanya melalui parameter URL atau fragmen hash—dan menggunakannya di dalam sink pengalihan sisi klien seperti `window.location` atau `document.location.href`. Kerentanan ini terutama dimanfaatkan oleh penyerang untuk melakukan kampanye phishing yang canggih, karena korban awalnya mempercayai domain yang sah sebelum dialihkan secara diam-diam ke situs berbahaya. Selain phishing, client-side redirect sering kali dapat dirantai untuk mengeksfiltrasi data sensitif seperti access token OAuth atau session identifier jika pengalihan terjadi saat nilai-nilai tersebut ada di dalam URL atau header `Referer`. Dalam Single Page Applications (SPA) modern, kerentanan ini sering muncul dalam logika routing, parameter "return to" setelah autentikasi, atau fungsionalitas pengalihan bahasa.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CLNT-04 |
| **CWE** | CWE-601 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Sedang* |

> *Tingkat keparahan menjadi Tinggi jika pengalihan dapat digunakan untuk mengeksfiltrasi token OAuth, session ID, atau melewati kontrol keamanan dalam alur autentikasi.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/04-Testing_for_Client-side_URL_Redirect  
* https://hacktricks.wiki/en/pentesting-web/open-redirect.html  

**Alat:** `Burp Suite (DOM Invader)`, `Browser DevTools`, `LinkFinder`, `Arjun`, `B-Config`

### Apakah aplikasi menggunakan skrip sisi klien untuk menangani pengalihan berdasarkan input pengguna?
- [ ] Tidak — pengalihan ditangani secara eksklusif di sisi server melalui kode status HTTP `3xx`  
- [ ] Ya — aplikasi menggunakan sink JavaScript seperti `window.location` untuk menavigasi berdasarkan parameter URL  
- [ ] Ya — aplikasi menggunakan router framework sisi klien (misalnya, React Router, Vue Router) yang memproses jalur (path) yang diberikan pengguna  

### Apakah validasi input atau kontrol whitelisting diimplementasikan untuk URL tujuan?
- [ ] Ya — **whitelist** ketat untuk domain/jalur yang diizinkan diterapkan dan bypass **tidak dimungkinkan**  
- [ ] Ya — validasi ada tetapi mengandalkan regex yang lemah atau blacklist, dan bypass **dimungkinkan**  
- [ ] Tidak — aplikasi menerima string arbitrer apa pun sebagai target pengalihan  

### Dapatkah logika pengalihan dilewati menggunakan teknik obfuskasi umum?
- [ ] Tidak — kontrol menangani karakter terenkode, protocol-relative URLs (`//`), dan backslash dengan benar  
- [ ] Ya — bypass **dimungkinkan** menggunakan URL encoding atau double-encoding  
- [ ] Ya — bypass **dimungkinkan** menggunakan protocol-relative URLs atau karakter `@` (misalnya, `https://expected.com@attacker.com`)  
- [ ] Ya — bypass **dimungkinkan** menggunakan browser quirks tertentu atau null byte injection  

### Apakah proses pengalihan mengekspos data sensitif ke situs tujuan?
- [ ] Tidak — tidak ada data sensitif yang ada di URL atau dikirimkan melalui header selama pengalihan  
- [ ] Ya — data sensitif (misalnya, session tokens, API keys) **bocor** melalui header `Referer` ke situs eksternal  
- [ ] Ya — data sensitif yang ada dalam fragmen URL (`#`) **dapat diakses** oleh skrip situs tujuan  

### Apakah mungkin untuk mengeksekusi JavaScript melalui sink pengalihan (DOM-based XSS)?
- [ ] Tidak — sink hanya mengizinkan navigasi ke protokol `http` atau `https`  
- [ ] Ya — sink pengalihan mengizinkan URI `javascript:` atau `data:`, sehingga memungkinkan terjadinya **DOM-based XSS**

---

## WSTG-CLNT-05 — Pengujian untuk CSS Injection

CSS Injection terjadi ketika sebuah aplikasi memungkinkan input yang disediakan pengguna untuk memengaruhi Cascading Style Sheets (CSS) dari sebuah halaman web tanpa sanitasi atau escaping yang tepat. Meskipun sering dianggap sebagai masalah kosmetik, penyerang dapat memanfaatkan CSS injection untuk mengeksfiltrasi data sensitif, seperti CSRF tokens atau session IDs, dengan menggunakan attribute selectors dan properti background-image untuk memicu permintaan eksternal. Kerentanan ini biasanya terjadi pada endpoint di mana preferensi pengguna (misalnya, tema kustom, warna font) direfleksikan di dalam blok `<style>` atau atribut `style` inline. Dari perspektif penyerang, eksploitasi yang berhasil dapat menyebabkan UI redressing, phishing melalui modifikasi tata letak yang tidak sah, atau eksfiltrasi data secara sembunyi-sembunyi di lingkungan di mana Content Security Policies yang ketat mungkin memblokir serangan berbasis JavaScript.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CLNT-05 |
| **CWE** | CWE-74 |
| **Status Pengujian** | Belum Dilakukan |
| **Severity** | Sedang / Tinggi* |

> *Severity menjadi Tinggi jika titik injeksi memungkinkan eksfiltrasi atribut sensitif seperti CSRF tokens atau jika dapat digunakan untuk UI redressing dalam alur kerja (workflow) yang sensitif.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/05-Testing_for_CSS_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite`, `CSS-Injection-Payload-Generator`, `CyberChef`, `Postman`

### Apakah aplikasi merefleksikan input yang dikontrol pengguna dalam konteks CSS?
- [ ] Tidak — input **tidak** direfleksikan dalam tag style, atribut, atau file CSS  
- [ ] Ya — input direfleksikan tetapi sangat terbatas pada nilai alfanumerik yang aman  
- [ ] Ya — input direfleksikan di dalam atribut `style` atau blok `<style>`  

### Apakah metakarakter dan fungsi spesifik CSS disanitasi dengan benar?
- [ ] Ya — karakter seperti `{`, `}`, `:`, dan fungsi seperti `url()` telah **di-escape dengan benar**  
- [ ] Ya — sanitasi sudah diterapkan tetapi bypass **dimungkinkan** melalui encoding atau komentar CSS  
- [ ] No — tidak ada sanitasi yang diterapkan pada metakarakter CSS  

### Apakah eksfiltrasi data mungkin dilakukan menggunakan CSS selectors?
- [ ] Tidak — selectors **tidak dapat** memicu permintaan eksternal atau membocorkan data  
- [ ] Ya — eksfiltrasi data parsial **dimungkinkan** melalui attribute selectors dan `background-image`  
- [ ] Ya — eksfiltrasi penuh token sensitif **dimungkinkan** menggunakan teknik Brute Force otomatis  

### Apakah Content Security Policy (CSP) memitigasi risiko CSS injection?
- [ ] Ya — `style-src` dibatasi ke 'self' dan `img-src` atau `connect-src` **dibatasi**  
- [ ] Ya — CSP ada tetapi menggunakan 'unsafe-inline' atau mengizinkan wildcard domain eksternal  
- [ ] Tidak — tidak ada CSP yang diimplementasikan untuk mencegah pemuatan sumber daya eksternal melalui CSS  

### Dapatkah kerentanan ini digunakan untuk melakukan UI redressing atau phishing?
- [ ] Tidak — cakupan injeksi terlalu terbatas untuk memodifikasi tata letak secara signifikan  
- [ ] Ya — modifikasi UI **dimungkinkan**, memungkinkan overlay elemen berbahaya  
- [ ] Ya — kontrol penuh tata letak halaman **dimungkinkan**, memfasilitasi serangan phishing yang sangat meyakinkan

---

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

## WSTG-CLNT-07 — Cross-Origin Resource Sharing (CORS)

Cross-Origin Resource Sharing (CORS) adalah mekanisme berbasis browser yang memungkinkan server untuk secara eksplisit mengizinkan akses ke sumber dayanya (resources) dari origin yang berbeda, yang secara efektif melonggarkan Same-Origin Policy (SOP). Konfigurasi yang salah (misconfiguration) terjadi ketika aplikasi terlalu mempercayai origin arbitrer, seringkali dengan merefleksikan header `Origin` dalam header respons `Access-Control-Allow-Origin` atau dengan menggunakan whitelist regex yang diimplementasikan dengan buruk. Dari perspektif penyerang, kebijakan CORS yang permisif dapat dieksploitasi dengan menghosting skrip berbahaya pada domain yang dikendalikan yang membuat permintaan terautentikasi (authenticated requests) ke aplikasi yang rentan, sehingga memungkinkan penyerang untuk mengeksfiltrasi data sensitif, seperti token CSRF atau PII, langsung dari badan respons (response body). Kerentanan ini paling kritis pada endpoint API yang memproses sesi terautentikasi dan mengembalikan informasi non-publik.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CLNT-07 |
| **CWE** | CWE-942 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Sedang / Tinggi |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/07-Testing_Cross_Origin_Resource_Sharing  
* https://hacktricks.wiki/en/pentesting-web/cors-bypass.html  
* https://portswigger.net/web-security/cors  

**Tools:** `Burp Suite (CORS Raider)`, `curl`, `Postman`, `Corsy`

### Apakah aplikasi mengimplementasikan header CORS pada endpoint yang terautentikasi?
- [ ] Tidak — CORS **tidak diaktifkan**; browser kembali ke Same-Origin Policy yang ketat secara default  
- [ ] Ya — Header CORS ada dan dibatasi pada **whitelist** yang ketat  
- [ ] Ya — Header CORS ada tetapi dikonfigurasi secara **permisif**  

### Bagaimana server menangani header `Access-Control-Allow-Origin` (ACAO)?
- [ ] ACAO diatur ke domain statis yang spesifik dan **terpercaya**  
- [ ] ACAO diatur ke `*` (wildcard) dan kredensial **tidak dapat** dikirim  
- [ ] ACAO diatur ke `*` (wildcard) dan kredensial **dapat** dikirim *(Catatan: Sebagian besar browser memblokir hal ini)*  
- [ ] ACAO **merefleksikan secara dinamis** nilai header `Origin` dari permintaan  

### Apakah header `Access-Control-Allow-Credentials` (ACAC) diatur ke `true`?
- [ ] Tidak — ACAC **tidak ada** atau diatur ke `false`  
- [ ] Ya — ACAC diatur ke `true`, tetapi ACAO **dibatasi** pada origin yang terpercaya  
- [ ] Ya — ACAC diatur ke `true` dan ACAO **merefleksikan** origin arbitrer *(Kritis)*  

### Dapatkah whitelist origin di-bypass melalui manipulasi subdomain atau null origin?
- [ ] Tidak — whitelist ditegakkan secara ketat dan **tidak dapat** di-bypass  
- [ ] Ya — whitelist menerima **subdomain apa pun** dari target (misalnya, `attacker.target.com`)  
- [ ] Ya — whitelist menerima origin `null` melalui iframe yang di-sandbox  
- [ ] Ya — whitelist rentan terhadap bypass regex (misalnya, `target.com.attacker.com` atau `target.com-safe.com`)  

### Apakah konfigurasi CORS memungkinkan eksfiltrasi data sensitif?
- [ ] Tidak — endpoint yang terpapar **tidak** berisi data sensitif atau spesifik sesi  
- [ ] Ya — endpoint yang terautentikasi mengembalikan PII, kredensial, atau token CSRF yang **dapat** dibaca oleh origin yang tidak sah

---

## WSTG-CLNT-08 — Pengujian Cross Site Flashing

Cross-Site Flashing (XSF) adalah kerentanan sisi klien (client-side) yang terjadi ketika file Flash (SWF) tidak menangani input yang dikontrol pengguna dengan benar, sehingga memungkinkan penyerang untuk mengeksekusi kode ActionScript berbahaya atau menjembatani ke dalam lingkungan JavaScript browser. Dengan memanipulasi parameter seperti `FlashVars` atau URL query string yang diteruskan ke sink seperti `ExternalInterface.call`, `getURL`, atau `loadMovie`, penyerang dapat melakukan tindakan dalam konteks domain yang rentan, termasuk session hijacking dan eksfiltrasi data. Kerentanan ini terutama ditemukan pada aplikasi enterprise warisan (legacy) yang masih menghosting file SWF, di mana validasi input yang tidak memadai memungkinkan file Flash tersebut disalahgunakan sebagai vektor untuk Cross-Site Scripting (XSS) atau untuk melewati Same-Origin Policy (SOP) melalui konfigurasi lintas-domain (cross-domain) yang terlalu permisif.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CLNT-08 |
| **CWE** | CWE-79 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Sedang / Tinggi |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/08-Testing_for_Cross_Site_Flashing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `JPEXS Free Flash Decompiler`, `FFDec`, `Burp Suite`, `Google Dorks`, `strings`

### Apakah file Adobe Flash (.swf) lama di-host pada aplikasi?
- [ ] Tidak — tidak ada file Flash yang di-host atau dirujuk dalam cakupan aplikasi  
- [ ] Ya — file SWF ada tetapi hanya menyajikan konten statis  
- [ ] Ya — file SWF ada dan menerima parameter dinamis melalui `FlashVars` atau string URL  

### Apakah input yang diteruskan ke ActionScript sink telah disanitasi?
- [ ] Ya — semua input divalidasi dengan ketat dan ActionScript sink **tidak dapat** dimanipulasi  
- [ ] Ya — validasi diterapkan tetapi bypass **memungkinkan** melalui teknik encoding tertentu  
- [ ] Tidak — input diteruskan secara langsung ke sink sensitif seperti `ExternalInterface.call` atau `getURL` *(Kritis)*  

### Dapatkah file Flash digunakan untuk mengeksekusi JavaScript arbitrer (XSS)?
- [ ] Tidak — `ExternalInterface` dalam kondisi **nonaktif** atau parameter `allowScriptAccess` diatur ke `never`  
- [ ] Ya — `allowScriptAccess` diatur ke `sameDomain` tetapi SWF di-host pada domain target  
- [ ] Ya — `allowScriptAccess` diatur ke `always`, memungkinkan XSS dari domain mana pun  

### Apakah kebijakan `crossdomain.xml` mencegah permintaan lintas-asal (cross-origin) yang tidak sah?
- [ ] Ya — `crossdomain.xml` bersifat restriktif dan hanya mengizinkan asal (origin) spesifik yang terpercaya  
- [ ] Tidak — file `crossdomain.xml` **tidak ditemukan**  
- [ ] Ya — kebijakan terlalu permisif (misalnya, `<allow-access-from domain="*" />`)  

### Dapatkah file SWF dipaksa untuk memuat movie eksternal yang dikendalikan penyerang?
- [ ] Tidak — aplikasi **tidak dapat** dipaksa untuk memuat file SWF eksternal  
- [ ] Ya — fungsi `loadMovie` atau `loadMovieNum` menerima URL eksternal yang tidak divalidasi

---

## WSTG-CLNT-09 — Testing for Clickjacking

Clickjacking, juga dikenal sebagai UI redressing, terjadi ketika penyerang menggunakan lapisan transparan atau buram untuk mengelabui pengguna agar mengeklik tombol atau tautan pada halaman lain padahal mereka bermaksud mengeklik halaman tingkat atas. Kerentanan ini mengompromikan integritas tindakan pengguna, yang berpotensi menyebabkan modifikasi data yang tidak sah, perubahan akun, atau transfer keuangan yang dilakukan atas nama korban. Penyerang biasanya mengeksploitasi hal ini dengan menanamkan (embedding) situs web target di dalam `<iframe>` pada situs berbahaya yang dikendalikan dan melapisinya dengan konten yang menipu atau umpan yang menarik. Hal ini paling sering terjadi pada halaman yang melakukan tindakan perubahan status (state-changing actions) yang sensitif seperti tombol "Hapus Akun", formulir pengaturan ulang kata sandi, atau panel konfigurasi administratif.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CLNT-09 |
| **CWE** | CWE-1021 |
| **Status Pengujian** | Tidak Dilakukan |
| **Tingkat Keparahan** | Sedang / Tinggi* |

> *Tingkat keparahan menjadi Tinggi jika tindakan sensitif (misalnya, transfer dana, penghapusan akun, atau Eskalasi Hak Istimewa (Privilege Escalation)) dapat dipicu melalui clickjacking.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/09-Testing_for_Clickjacking  
* https://hacktricks.wiki/en/pentesting-web/clickjacking.html  
* https://portswigger.net/web-security/clickjacking  

**Alat:** `Burp Suite Clickbandit`, `Browser Developer Tools`, `OWASP ZAP`, `Online Clickjack Test`

### Apakah aplikasi menerapkan header sisi server untuk mencegah framing yang tidak sah?
- [ ] Ya — `X-Frame-Options` atau `Content-Security-Policy` **diterapkan** dan mencegah framing *(Paling aman)*  
- [ ] Ya — header ada tetapi **salah dikonfigurasi** (misalnya, `X-Frame-Options: ALLOW-FROM` yang kurang mendapat dukungan browser secara luas)  
- [ ] No — tidak ada header perlindungan framing yang **diterapkan**  

### Apakah direktif `frame-ancestors` pada `Content-Security-Policy` (CSP) diimplementasikan?
- [ ] Ya — `frame-ancestors 'none'` atau `'self'` **diterapkan**  
- [ ] Ya — `frame-ancestors` ada tetapi **mengizinkan** asal (origin) yang tidak tepercaya atau terlalu luas  
- [ ] No — direktif **hilang** atau CSP tidak diimplementasikan  

### Apakah header `X-Frame-Options` (XFO) dikonfigurasi dengan benar untuk dukungan browser lama (legacy)?
- [ ] Ya — `DENY` atau `SAMEORIGIN` **diterapkan**  
- [ ] Ya — XFO ada tetapi **diabaikan** oleh browser modern karena adanya direktif `frame-ancestors` pada CSP  
- [ ] No — header XFO **hilang** atau disetel ke nilai yang tidak aman  

### Bisakah perlindungan framing dilewati (bypass) menggunakan teknik yang diketahui?
- [ ] No — bypass **tidak dimungkinkan** melalui teknik standar (double-framing, dll.)  
- [ ] Ya — perlindungan **dapat** dilewati karena regex atau logika yang lemah pada daftar putih (whitelist) `frame-ancestors`  
- [ ] Ya — perlindungan **dapat** dilewati karena aplikasi hanya mengandalkan frame-busting berbasis JavaScript  

### Apakah tindakan sensitif dapat dieksploitasi melalui proof-of-concept (PoC) clickjacking?
- [ ] No — framing diblokir atau tidak ada tindakan sensitif yang dapat dijangkau  
- [ ] Ya — UI redressing **dimungkinkan** tetapi membutuhkan interaksi pengguna yang kompleks  
- [ ] Ya — UI redressing **dimungkinkan** dan memungkinkan tindakan perubahan status (state-changing) secara langsung *(Kritis)*

---

## WSTG-CLNT-10 — Pengujian WebSockets

Pengujian WebSocket berfokus pada saluran komunikasi full-duplex yang dibangun antara klien dan server, yang melewati pola request-response HTTP tradisional. Pentester mengevaluasi keamanan proses handshake, keberadaan perlindungan Cross-Site WebSocket Hijacking (CSWSH), dan ketatnya validasi input yang diterapkan pada payload pesan. Eksploitasi biasanya melibatkan pembajakan sesi aktif untuk mengeksfiltrasi data secara real-time atau menyuntikkan payload berbahaya yang memicu kerentanan sisi server seperti Command Injection atau SQLi melalui socket yang persisten. Karena banyak Web Application Firewall (WAF) tradisional tidak memeriksa lalu lintas WebSocket secara efektif, protokol ini sering kali memberikan vektor tersembunyi untuk melewati pertahanan perimeter.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CLNT-10 |
| **CWE** | CWE-1385 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi / Sedang |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/10-Testing_WebSockets  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/websockets  

**Alat:** `Burp Suite (WebSockets tab)`, `OWASP ZAP`, `wscat`, `Socket.io-client`, `Wireshark`

### Apakah koneksi WebSocket dibangun melalui saluran yang aman?
- [ ] Ya — `wss://` (WebSocket Secure) diterapkan untuk semua koneksi  
- [ ] Tidak — `ws://` digunakan, memungkinkan penyadapan person-in-the-middle (PITM)  

### Apakah aplikasi terlindungi dari Cross-Site WebSocket Hijacking (CSWSH)?
- [ ] Tidak — server memvalidasi header `Origin` secara ketat selama handshake  
- [ ] Ya — validasi header `Origin` lemah atau **dapat** dilewati melalui origin null atau palsu (spoofed)  
- [ ] Ya — tidak ada validasi header `Origin` yang dilakukan, memungkinkan penyerang lintas situs untuk memulai koneksi *(Kritis)*  

### Apakah validasi dan sanitasasi input diterapkan pada payload pesan WebSocket?
- [ ] Ya — semua pesan masuk disanitasi dan divalidasi terhadap skema  
- [ ] Ya — beberapa validasi ada namun bypass **mungkin dilakukan** menggunakan payload yang dienkode atau non-standar  
- [ ] Tidak — pesan diproses secara langsung, memungkinkan injeksi (XSS, SQLi, RCE) atau manipulasi logika  

### Apakah pemeriksaan autentikasi dan otorisasi dilakukan pada setiap pesan WebSocket?
- [ ] Ya — sesi diverifikasi untuk setiap pesan atau protokol bersifat stateless dengan token  
- [ ] Ya — autentikasi terjadi pada saat handshake, tetapi otorisasi untuk tindakan tertentu **tidak** diterapkan per pesan  
- [ ] Tidak — autentikasi hanya diperiksa saat handshake, memungkinkan pembajakan sesi untuk memberikan akses penuh  

### Apakah server menerapkan pembatasan laju (Rate Limiting) atau batasan sumber daya pada WebSocket?
- [ ] Ya — Rate Limiting dan ukuran pesan maksimum **diaktifkan** dan diterapkan  
- [ ] Tidak — tidak ada batasan, memungkinkan Denial of Service (DoS) melalui pembanjiran pesan (message flooding) atau ukuran payload yang besar

---

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

## WSTG-CLNT-12 — Menguji Penyimpanan Browser

Menguji penyimpanan browser melibatkan analisis terhadap bagaimana sebuah aplikasi memanfaatkan mekanisme sisi klien (client-side) seperti `LocalStorage`, `SessionStorage`, `IndexedDB`, dan `WebSQL` untuk menyimpan data secara persisten. Informasi sensitif yang disimpan secara tidak aman—termasuk PII, token otentikasi, atau identitas sesi—dapat dikompromikan jika penyerang berhasil melakukan Cross-Site Scripting (XSS) atau memperoleh akses fisik ke perangkat pengguna. Pentester harus menentukan apakah data disimpan dalam bentuk cleartext, apakah data tetap dapat diakses setelah logout, dan apakah siklus hidup penyimpanan dikelola dengan tepat untuk mencegah kebocoran data. Dari perspektif penyerang, mekanisme penyimpanan ini merupakan target yang menguntungkan untuk mengeksfiltrasi informasi pemeliharaan status (state-maintaining information) yang memungkinkan terjadinya pembajakan sesi (session hijacking) atau persistensi akun jangka panjang.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CLNT-12 |
| **CWE** | CWE-922 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan (Severity)** | Sedang / Tinggi* |

> *Tingkat keparahan menjadi Tinggi jika token sesi atau PII sensitif disimpan di `LocalStorage` dan dapat diakses melalui XSS.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/12-Testing_Browser_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Browser DevTools`, `Storage Explorer`, `Burp Suite`, `OWASP ZAP`

### Apakah aplikasi menyimpan informasi sensitif di penyimpanan browser?
- [ ] Tidak — aplikasi **tidak** menggunakan penyimpanan browser atau hanya menyimpan status UI yang tidak sensitif  
- [ ] Ya — data sensitif disimpan tetapi **dienkripsi** menggunakan kriptografi sisi klien yang kuat  
- [ ] Ya — data sensitif (token, PII, rahasia) disimpan dalam bentuk **cleartext** *(Sedang)*  

### Apakah data yang disimpan dapat diakses oleh skrip yang tidak sah (risiko XSS)?
- [ ] Tidak — data disimpan dalam cookie HttpOnly (bukan penyimpanan browser) atau penyimpanan **tidak digunakan**  
- [ ] Ya — `LocalStorage`/`SessionStorage` digunakan, membuat data **dapat diakses sepenuhnya** melalui payload XSS apa pun *(Tinggi)*  

### Apakah penyimpanan browser dibersihkan dengan tepat saat pengguna melakukan logout?
- [ ] Ya — semua penyimpanan yang terkait dengan aplikasi **dibersihkan secara eksplisit** selama proses logout  
- [ ] Tidak — penyimpanan tetap ada setelah logout, tetapi **tidak** berisi data sesi yang sensitif  
- [ ] Tidak — token otentikasi atau PII **tetap ada** dalam penyimpanan setelah sesi dihentikan *(Sedang)*  

### Apakah aplikasi menggunakan IndexedDB atau WebSQL untuk dataset yang besar/sensitif?
- [ ] Tidak — mekanisme penyimpanan ini **tidak digunakan**  
- [ ] Ya — data disimpan dan **disanitasi dengan benar** sebelum dirender di DOM  
- [ ] Ya — dataset sensitif disimpan dalam bentuk **cleartext** di dalam struktur `IndexedDB`/`WebSQL`  

### Apakah integritas data yang disimpan dapat dimanipulasi untuk memengaruhi logika aplikasi?
- [ ] Tidak — aplikasi memvalidasi atau menandatangani data sebelum memprosesnya dari penyimpanan  
- [ ] Ya — memodifikasi nilai penyimpanan (misalnya, peran pengguna, flag) **dimungkinkan** dan menghasilkan perubahan perilaku aplikasi

---

## WSTG-CLNT-13 — Testing for Cross Site Script Inclusion

Cross-Site Script Inclusion (XSSI) terjadi ketika aplikasi web mengekspor data sensitif dalam file JavaScript yang dihasilkan secara dinamis atau respons JSONP, yang kemudian dapat disertakan oleh situs yang dikendalikan penyerang melalui tag `<script>`. Karena penyertaan skrip melewati Same-Origin Policy (SOP), penyerang dapat mengeksekusi skrip dalam konteks mereka sendiri dan menimpa objek atau variabel global untuk mengeksfiltrasi informasi sensitif. Kerentanan ini biasanya muncul pada endpoint terautentikasi yang mengembalikan data spesifik pengguna seperti alamat email, API keys, atau metadata sesi dalam format skrip. Dari perspektif penyerang, eksploitasi melibatkan pembuatan halaman berbahaya yang mereferensikan skrip rentan dan menangkap perubahan variabel global atau pemanggilan fungsi yang dihasilkan.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CLNT-13 |
| **CWE** | CWE-200 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Sedang / Tinggi |

> *Tingkat keparahan menjadi Tinggi jika data yang bocor mencakup token CSRF, pengidentifikasi sesi, atau PII (Personally Identifiable Information) yang sangat sensitif.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/13-Testing_for_Cross_Site_Script_Inclusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `JSParser`, `LinkFinder`, `Browser Developer Tools`

### Apakah terdapat endpoint terautentikasi yang mengembalikan JavaScript dinamis atau JSONP yang berisi informasi sensitif?
- [ ] Tidak — semua skrip bersifat statis dan **tidak dapat** berisi data spesifik pengguna  
- [ ] Ya — terdapat skrip dinamis tetapi **tidak berisi** informasi sensitif  
- [ ] Ya — skrip dinamis berisi PII atau token dan **dapat** diakses lintas origin  

### Apakah header `X-Content-Type-Options: nosniff` diimplementasikan pada respons skrip yang sensitif?
- [ ] Ya — header **diterapkan** dan mencegah MIME-type sniffing  
- [ ] Tidak — header **hilang**, berpotensi memungkinkan konten non-skrip dieksekusi sebagai skrip  

### Apakah skrip sensitif dilindungi oleh mekanisme anti-XSSI seperti awalan non-executable atau header khusus?
- [ ] Ya — skrip memerlukan header khusus atau token yang **tidak dapat** dikirim melalui tag `<script>` standar  
- [ ] Ya — skrip menggunakan awalan non-executable (misal: `)]}'`) yang **tidak dapat** dilewati dengan mudah  
- [ ] Tidak — skrip dapat diakses langsung melalui permintaan GET menggunakan ambient credentials *(Kritis)*  

### Apakah eksfiltrasi data sensitif dimungkinkan dengan menimpa variabel global atau prototipe?
- [ ] Tidak — data sensitif memiliki cakupan lokal atau dilindungi melalui fitur peramban modern  
- [ ] Ya — data sensitif ditetapkan ke variabel global dan **dapat** diambil  
- [ ] Ya — data bocor melalui callback JSONP yang **dapat** dicegat

---

## WSTG-CLNT-14 — Testing for Reverse Tabnabbing

Reverse Tabnabbing adalah kerentanan sisi klien (Client-side vulnerability) di mana sebuah halaman yang dibuka melalui `target="_blank"` mempertahankan referensi fungsional ke halaman induk melalui objek `window.opener`. Hal ini memungkinkan situs tujuan yang dikontrol oleh penyerang untuk secara terprogram mengalihkan tab asli yang tepercaya ke URL berbahaya, biasanya berupa halaman phishing yang dirancang untuk mencuri kredensial (Harvesting credentials). Serangan ini mengeksploitasi kepercayaan inheren pengguna terhadap tab asli, karena mereka cenderung tidak menyadari bahwa halaman yang baru saja ditinggalkan telah dialihkan ke domain yang berbeda. Praktik keamanan modern memerlukan penggunaan atribut `rel="noopener"` atau `rel="noreferrer"` pada tag anchor, atau menyetel properti `opener` menjadi null dalam JavaScript, untuk mencegah komunikasi lintas jendela (Cross-window communication) ini.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CLNT-14 |
| **CWE** | CWE-1022 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Rendah (Low) / Sedang (Medium)* |

> *Keparahan bernilai Rendah (Low) pada sebagian besar browser modern karena saat ini secara default menyetel `target="_blank"` ke `rel="noopener"`. Keparahan menjadi Sedang (Medium) jika aplikasi menargetkan browser lama (Legacy browsers) atau menggunakan `window.open()` tanpa menyetel properti `opener` menjadi null.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/14-Testing_for_Reverse_Tabnabbing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Browser DevTools (Inspector)`, `Burp Suite (Repeater)`, `ZAP`

### Apakah terdapat tautan yang membuka di jendela atau tab baru?
- [ ] Tidak — tidak ada tautan yang menggunakan `target="_blank"` atau pengalihan berbasis JavaScript yang setara  
- [ ] Ya — `target="_blank"` digunakan secara eksklusif pada tautan internal yang tepercaya  
- [ ] Ya — `target="_blank"` digunakan pada tautan eksternal atau URL yang disediakan oleh pengguna  

### Apakah hubungan `window.opener` dibatasi pada tag anchor?
- [ ] Ya — `rel="noopener"` atau `rel="noreferrer"` **diterapkan secara konsisten** pada semua tautan dengan `target="_blank"` *(Paling aman)*  
- [ ] Ya — atribut diterapkan tetapi **hilang** pada tautan berisiko tinggi atau tautan yang dibuat oleh pengguna  
- [ ] Tidak — atribut **tidak diterapkan**, memungkinkan halaman anak untuk mengakses `window.opener`  

### Apakah aplikasi rentan terhadap Reverse Tabnabbing melalui JavaScript `window.open()`?
- [ ] Tidak — `window.open()` tidak digunakan atau secara eksplisit menyetel properti `opener` menjadi null  
- [ ] Ya — `window.open()` digunakan dan referensi `opener` **tetap dapat diakses** oleh jendela anak  

### Dapatkah penyerang mengontrol tujuan dari tautan yang membuka di tab baru?
- [ ] Tidak — semua tujuan tautan di-hardcode dan merujuk ke domain tepercaya  
- [ ] Ya — tujuan tautan disediakan oleh pengguna (misalnya, profil media sosial, tautan bio) dan **tidak** dibersihkan (Sanitized) untuk perlindungan tabnabbing

---

## WSTG-APIT-01 — API Reconnaissance

API reconnaissance adalah proses sistematis untuk mengidentifikasi dan memetakan permukaan serangan (attack surface) API guna mengungkap endpoint, metode yang didukung, dan struktur data yang mendasarinya. Penyerang melakukan fase ini untuk menemukan API yang tidak terdokumentasi (shadow API), versi yang sudah tidak digunakan lagi (deprecated) dengan kerentanan warisan (legacy vulnerabilities), serta berkas dokumentasi yang terekspos seperti definisi Swagger atau OpenAPI yang mengungkapkan logika bisnis internal. Dengan menganalisis pola URL yang dapat diprediksi, berkas JavaScript sisi klien (client-side), dan hasil directory brute-force, seorang lawan dapat membangun peta komprehensif dari API untuk mengidentifikasi target serangan Broken Object Level Authorization (BOLA) atau Mass Assignment. Rekognisi ini biasanya menargetkan jalur umum seperti `/api/v1/`, `/swagger.json`, atau `/graphql` untuk mendapatkan peta jalan dari fungsionalitas backend aplikasi.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-APIT-01 |
| **CWE** | CWE-200 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Informasional / Rendah |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/01-API_Reconnaissance  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/api-testing  

**Alat:** `Kiterunner`, `ffuf`, `Arjun`, `Burp Suite (Logger++)`, `Postman`, `Swagger-ez`

### Apakah berkas dokumentasi API (Swagger, OpenAPI, WSDL) dapat diakses secara publik?
- [ ] Tidak — dokumentasi API **tidak** dapat diakses atau dibatasi oleh autentikasi  
- [ ] Ya — dokumentasi dapat diakses namun memerlukan kredensial yang valid  
- [ ] Ya — dokumentasi API (misalnya, `/swagger-ui.html`) **dapat diakses secara publik** tanpa autentikasi  

### Dapatkah endpoint API yang tidak terdokumentasi atau "shadow API" ditemukan melalui fuzzing?
- [ ] Tidak — fuzzing direktori dan endpoint tidak menghasilkan jalur yang tidak terdokumentasi  
- [ ] Ya — endpoint yang tidak terdokumentasi ditemukan tetapi **tidak dapat** diakses tanpa otorisasi  
- [ ] Ya — endpoint yang tidak terdokumentasi ditemukan dan **dapat** diakses tanpa otorisasi yang valid  

### Apakah aplikasi mengungkapkan beberapa versi API (misalnya, /v1/, /v2/, /beta/)?
- [ ] Tidak — hanya versi API saat ini yang sudah diperkuat (hardened) yang dapat diakses  
- [ ] Ya — versi lama (legacy) tersedia tetapi menerapkan kontrol keamanan yang **sama** dengan versi saat ini  
- [ ] Ya — versi lama (misalnya, `/v1/`) dapat diakses dan **tidak memiliki** kontrol keamanan seperti pada versi yang lebih baru  

### Apakah sumber daya sisi klien (JavaScript/Aplikasi Mobile) membocorkan struktur endpoint API atau kunci?
- [ ] Tidak — kode sisi klien **tidak** mengandung jalur API yang dikodekan secara keras (hardcoded) atau kunci sensitif  
- [ ] Ya — kode sisi klien mengandung peta endpoint API tetapi **tidak ada** kunci sensitif  
- [ ] Ya — kunci API, rahasia (secrets), atau URL endpoint khusus internal **dikodekan secara keras (hardcoded)** dalam sumber daya sisi klien  

### Apakah parameter atau header tersembunyi dapat ditemukan pada endpoint yang diketahui?
- [ ] Tidak — fuzzing parameter (misalnya, melalui `Arjun`) **tidak** mengungkapkan input tersembunyi  
- [ ] Ya — parameter tersembunyi (misalnya, `debug=true`, `admin=1`) ditemukan tetapi **tidak** berfungsi  
- [ ] Ya — parameter tersembunyi ditemukan dan **dapat** digunakan untuk mengubah perilaku aplikasi

---

## WSTG-APIT-02 — API Broken Object Level Authorization

Broken Object Level Authorization (BOLA), juga dikenal sebagai Insecure Direct Object Reference (IDOR), terjadi ketika API gagal memvalidasi apakah seorang pengguna memiliki izin yang tepat untuk mengakses atau memanipulasi sumber daya (resource) tertentu yang diidentifikasi oleh sebuah ID. Penyerang mengeksploitasi hal ini dengan melakukan enumerasi atau menebak pengenal (identifiers) secara sistematis—seperti ID numerik atau UUID—dalam jalur permintaan (request paths), parameter kueri (query parameters), atau badan JSON (JSON bodies) untuk mengakses data milik pengguna lain. Kerentanan ini adalah masalah yang paling umum dan berdampak besar dalam keamanan API modern, yang berpotensi menyebabkan eksfiltrasi data massal, modifikasi yang tidak sah, atau pengambilalihan akun secara penuh. Dari perspektif penyerang, tujuannya adalah untuk mengidentifikasi titik akhir (endpoints) yang menerima pengenal objek dan menguji apakah logika otorisasi di sisi server (server-side authorization) hilang atau dapat dilewati dengan memanipulasi ID tersebut.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-APIT-02 |
| **CWE** | CWE-285 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Tinggi / Kritis |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/02-API_Broken_Object_Level_Authorization  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/api-testing  

**Alat:** `Burp Suite (Intruder/Repeater)`, `Autorize`, `Postman`, `ffuf`, `Arjun`

### Apakah pengenal objek (object identifiers) dapat diprediksi atau dienumerasi dalam permintaan API?
- [ ] Tidak — pengenal bersifat panjang, acak, dan aman secara kriptografi (misalnya, UUIDv4)  
- [ ] Ya — pengenal berupa integer berurutan (misalnya, `101`, `102`)  
- [ ] Ya — pengenal mengikuti pola yang dapat diprediksi atau diturunkan dari informasi publik  

### Apakah API melakukan validasi kepemilikan objek di sisi server untuk setiap permintaan?
- [ ] Ya — pemeriksaan otorisasi diterapkan untuk setiap permintaan dan bypass **tidak dimungkinkan**  
- [ ] Ya — pemeriksaan diterapkan tetapi bypass **dimungkinkan** melalui parameter pollution atau method tunneling  
- [ ] No — aplikasi hanya mengandalkan autentikasi pengguna tanpa memeriksa kepemilikan sumber daya tertentu  

### Apakah mungkin untuk mengakses atau memodifikasi sumber daya pengguna lain dengan mengubah pengenal?
- [ ] Tidak — akses tidak sah ke sumber daya pengguna lain **tidak dimungkinkan**  
- [ ] Ya — akses **baca** tidak sah (Horizontal BOLA) **dimungkinkan**  
- [ ] Ya — **modifikasi** atau **penghapusan** tidak sah (Horizontal BOLA) **dimungkinkan**  

### Apakah API mengizinkan akses ke objek tingkat administratif atau sistem dengan mengganti ID?
- [ ] Tidak — sumber daya administratif dilindungi oleh lapisan otorisasi sekunder  
- [ ] Ya — mengakses atau memodifikasi sumber daya tingkat sistem (Vertical BOLA) **dimungkinkan**  

### Dapatkah pemeriksaan otorisasi dilewati dengan memindahkan pengenal ke bagian permintaan yang berbeda?
- [ ] Tidak — logika otorisasi konsisten terlepas dari lokasi parameter  
- [ ] Ya — bypass **dimungkinkan** ketika ID dipindahkan dari jalur URL ke badan JSON atau header  
- [ ] Ya — bypass **dimungkinkan** ketika beberapa ID diberikan dan server memproses ID yang tidak sah

---

## WSTG-APIT-99 — Testing GraphQL

GraphQL adalah bahasa kueri (query language) untuk API yang memungkinkan klien meminta struktur data tertentu, tetapi miskonfigurasi sering kali menyebabkan risiko keamanan yang signifikan termasuk pengungkapan informasi (information disclosure) dan eksploitasi sumber daya (resource exhaustion). Kerentanan biasanya muncul dari introspeksi (introspection) yang diaktifkan, kurangnya batasan kedalaman/kompleksitas kueri, dan otorisasi tingkat objek yang rusak (Broken Object-Level Authorization - BOLA) di dalam fungsi resolver. Penyerang mengeksploitasi endpoint ini dengan menggunakan introspeksi untuk memetakan seluruh skema, memanfaatkan fragmen sirkular untuk memicu Denial of Service (DoS), atau melewati otorisasi dengan mengakses bidang yang tidak sah melalui kueri bersarang (nested queries). Pengujian berfokus pada endpoint `/graphql`, `/v1/graphql`, atau `/graphiql` untuk memastikan bahwa implementasi menerapkan kontrol akses (access control) dan manajemen sumber daya yang ketat.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-APIT-99 |
| **CWE** | CWE-200 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi / Kritis* |

> *Keparahan menjadi Kritis jika otorisasi tingkat objek yang rusak (BOLA) memungkinkan akses tidak sah ke data sensitif (PII) atau mutasi administratif.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/99-Testing_GraphQL  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/graphql  

**Alat:** `InQL`, `Graphw00f`, `GraphQL Voyager`, `Burp Suite`, `Altair GraphQL Client`, `Clairvoyance`

### Apakah sistem introspeksi (introspection) GraphQL diaktifkan?
- [ ] Tidak — introspeksi **dinonaktifkan** dan skema tidak dapat dipetakan  
- [ ] Ya — introspeksi **diaktifkan** tetapi memerlukan autentikasi administratif  
- [ ] Ya — introspeksi **diaktifkan** dan dapat diakses secara publik *(Tinggi)*  

### Apakah batasan sumber daya (kedalaman dan kompleksitas) diterapkan pada kueri?
- [ ] Ya — batasan kedalaman dan kompleksitas kueri yang ketat **diterapkan** dan dijalankan  
- [ ] Ya — batasan **diterapkan** tetapi dapat dilewati menggunakan alias atau fragmen  
- [ ] Tidak — tidak ada batasan yang **diterapkan**, memungkinkan kueri rekursif dan DoS  

### Apakah Broken Object-Level Authorization (BOLA) terdapat pada resolver?
- [ ] Tidak — otorisasi **diterapkan** pada tingkat bidang dan objek untuk setiap kueri  
- [ ] Ya — otorisasi **diterapkan** pada beberapa bidang tetapi objek sensitif terekspos  
- [ ] Ya — otorisasi **tidak diterapkan**, memungkinkan akses ke objek apa pun melalui manipulasi ID  

### Apakah mutasi (mutations) GraphQL dilindungi dengan benar dari penggunaan yang tidak sah?
- [ ] Tidak — mutasi **tidak** ada di dalam skema  
- [ ] Ya — mutasi memerlukan autentikasi yang valid dan pemeriksaan otorisasi yang ketat  
- [ ] Ya — mutasi **mungkin dilakukan** oleh pengguna yang tidak terautentikasi atau kurang otorisasi  

### Apakah pesan kesalahan membocorkan detail implementasi atau skema yang sensitif?
- [ ] Tidak — pesan kesalahan bersifat umum dan **tidak** mengungkapkan logika internal  
- [ ] Ya — pesan kesalahan mengungkapkan tipe database yang mendasari atau saran melalui "Did you mean...?"  
- [ ] Ya — full stack traces dan data konfigurasi sensitif **terungkap** dalam respons