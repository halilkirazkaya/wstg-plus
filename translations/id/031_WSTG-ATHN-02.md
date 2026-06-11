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