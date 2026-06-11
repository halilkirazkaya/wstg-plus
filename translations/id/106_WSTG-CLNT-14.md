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