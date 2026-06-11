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