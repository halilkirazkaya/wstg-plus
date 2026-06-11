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