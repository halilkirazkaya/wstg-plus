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