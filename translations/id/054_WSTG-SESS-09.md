## WSTG-SESS-09 — Menguji Pengambilalihan Sesi (Session Hijacking)

Pengambilalihan sesi (Session Hijacking) terjadi ketika penyerang menangkap, memprediksi, atau menetapkan pengidentifikasi sesi (SID) yang valid untuk mendapatkan akses tidak sah ke sesi aktif pengguna. Kerentanan ini biasanya muncul karena keamanan lapisan transport yang tidak memadai, kurangnya flag keamanan cookie, atau algoritma pembuatan SID yang dapat diprediksi yang memungkinkan penyerang untuk melewati autentikasi sepenuhnya. Dari perspektif penyerang, eksploitasi yang berhasil memberikan kemampuan penyamaran penuh terhadap korban tanpa memerlukan kredensial mereka, yang sering kali dicapai melalui sniffing jaringan, Cross-Site Scripting (XSS), atau serangan fiksasi sesi (Session Fixation).


| Bidang | Nilai |
|---|---|
| **ID WSTG** | WSTG-SESS-09 |
| **CWE** | CWE-287 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Tinggi / Kritis |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/09-Testing_for_Session_Hijacking  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Alat:** `Burp Suite`, `OWASP ZAP`, `EditThisCookie`, `Wireshark`, `Bettercap`

### Apakah pengidentifikasi sesi terlindungi selama transit?
- [ ] Ya — flag `Secure` **diaktifkan** dan HSTS diterapkan secara ketat  
- [ ] Ya — flag `Secure` **diaktifkan** tetapi HSTS **dinonaktifkan**  
- [ ] Tidak — flag `Secure` **tidak diterapkan**, memungkinkan intersepsi SID melalui saluran yang tidak terenkripsi *(Tinggi)*  

### Apakah pengidentifikasi sesi terlindungi dari akses skrip sisi klien?
- [ ] Ya — flag `HttpOnly` **diterapkan** pada semua cookie yang terkait dengan sesi  
- [ ] Tidak — flag `HttpOnly` **tidak diterapkan**, memungkinkan eksfiltrasi SID melalui XSS *(Kritis)*  

### Apakah aplikasi menerapkan pengikatan sesi (session binding) ke atribut klien?
- [ ] Ya — sesi terikat ke atribut klien (IP/User-Agent) dan penggunaan kembali **tidak dimungkinkan**  
- [ ] Ya — pengikatan sesi ada tetapi bypass **dimungkinkan** melalui header spoofing  
- [ ] Tidak — tidak ada pengikatan sesi; SID **tetap valid** saat digunakan dari sumber mana pun  

### Apakah pengidentifikasi sesi cukup acak untuk mencegah prediksi?
- [ ] Ya — SID menggunakan generator angka pseudo-acak yang aman secara kriptografis (CSPRNG)  
- [ ] Tidak — panjang SID tidak memadai atau mengikuti urutan/pola yang **dapat diprediksi**  

### Apakah sesi bersamaan (concurrent sessions) dikelola untuk membatasi jendela serangan?
- [ ] Tidak — aplikasi secara ketat memberlakukan sesi aktif tunggal per pengguna  
- [ ] Ya — beberapa sesi bersamaan **diaktifkan** tetapi dipantau  
- [ ] Ya — sesi bersamaan yang tidak terbatas **dimungkinkan** di berbagai perangkat/lokasi  

---