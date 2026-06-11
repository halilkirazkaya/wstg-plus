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