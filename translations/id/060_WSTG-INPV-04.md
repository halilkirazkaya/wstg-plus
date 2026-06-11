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