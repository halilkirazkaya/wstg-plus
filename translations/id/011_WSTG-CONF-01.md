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