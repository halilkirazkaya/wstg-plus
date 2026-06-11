## WSTG-CONF-05 — Melakukan Enumerasi Antarmuka Admin Infrastruktur dan Aplikasi

Antarmuka administratif mewakili kontrol manajemen (control plane) dari sebuah aplikasi dan infrastruktur yang mendasarinya, yang sering kali memberikan akses istimewa ke konfigurasi sistem, data pengguna, dan operasi sisi server (server-side operations). Penyerang memprioritaskan enumerasi antarmuka ini melalui directory brute-forcing, analisis `robots.txt`, atau mengamati pola URL yang dapat diprediksi untuk menemukan titik masuk (entry points) yang mungkin kurang memiliki autentikasi yang kuat atau mengandalkan kredensial default. Penemuan panel admin yang terekspos secara signifikan meningkatkan risiko kompromi sistem secara menyeluruh, modifikasi data yang tidak sah, atau gangguan layanan. Antarmuka ini sering ditemukan pada port non-standar atau subdomain, menjadikannya target utama bagi pemindai otomatis dan pengintaian manual (manual reconnaissance).


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-CONF-05 |
| **CWE** | CWE-425 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Sedang / Tinggi* |

> *Tingkat keparahan menjadi Tinggi jika antarmuka memungkinkan perubahan konfigurasi di seluruh sistem, manajemen pengguna, atau eksfiltrasi data tanpa autentikasi multifaktor (MFA).

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/05-Enumerate_Infrastructure_and_Application_Admin_Interfaces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `ffuf`, `dirsearch`, `Gobuster`, `Nmap`, `Wappalyzer`, `Burp Suite (Intruder)`

### Apakah antarmuka administratif dapat ditemukan melalui directory fuzzing atau pengintaian (reconnaissance)?
- [ ] Tidak — tidak ada antarmuka administratif yang terekspos atau dapat ditemukan  
- [ ] Ya — antarmuka dapat ditemukan tetapi akses **dibatasi** pada rentang IP internal  
- [ ] Ya — antarmuka **dapat** ditemukan dan dapat diakses secara publik  

### Apakah autentikasi diterapkan pada portal administratif yang ditemukan?
- [ ] Ya — autentikasi multifaktor (MFA) **diterapkan**  
- [ ] Ya — autentikasi faktor tunggal (single-factor authentication) **diterapkan**  
- [ ] Tidak — tidak diperlukan autentikasi untuk mengakses antarmuka *(Kritis)*  

### Apakah jalur administratif disembunyikan atau menggunakan standar non-umum untuk mencegah penemuan?
- [ ] Ya — jalur menggunakan penamaan non-standar, acak, atau tidak dapat diprediksi  
- [ ] Tidak — jalur menggunakan konvensi penamaan umum (misalnya, `/admin`, `/manager`, `/console`) dan **dapat** ditebak dengan mudah  

### Apakah antarmuka menyediakan akses ke fungsionalitas sistem atau aplikasi yang sensitif?
- [ ] Tidak — antarmuka adalah dasbor status "read-only" dengan dampak minimal  
- [ ] Ya — antarmuka **dapat** memodifikasi konfigurasi aplikasi atau data pengguna  
- [ ] Ya — antarmuka **dapat** mengeksekusi perintah tingkat sistem atau melakukan manajemen basis data  

---