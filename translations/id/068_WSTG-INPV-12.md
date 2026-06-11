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