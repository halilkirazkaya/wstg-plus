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