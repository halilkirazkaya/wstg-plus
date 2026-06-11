## WSTG-SESS-04 — Testing for Exposed Session Variables

Variabel sesi yang terpapar terjadi ketika pengidentifikasi sesi yang sensitif atau data terkait status ditransmisikan melalui saluran yang tidak aman seperti URL query strings, Referer headers, atau log sistem. Paparan ini secara signifikan meningkatkan risiko Session Hijacking, karena pengidentifikasi dapat disimpan dalam cache riwayat peramban (browser history), dicatat oleh proksi perantara (intermediate proxies), atau bocor ke situs pihak ketiga melalui Referer header. Pentester mengidentifikasi paparan ini dengan memantau semua permintaan keluar (outbound requests) dan memeriksa log aplikasi atau konfigurasi server untuk kebocoran data yang tidak disengaja. Dalam skenario dunia nyata, penyerang mengeksploitasi kebocoran ini dengan mengumpulkan token sesi yang valid dari mesin bersama atau menganalisis pola lalu lintas untuk melewati otentikasi dan mendapatkan akses tidak sah ke akun pengguna.


| Kolom | Nilai |
|---|---|
| **WSTG ID** | WSTG-SESS-04 |
| **CWE** | CWE-598 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Sedang / Tinggi* |

> *Tingkat keparahan menjadi Tinggi jika variabel yang terpapar adalah token sesi yang memungkinkan pengambilalihan akun (account takeover) secara langsung.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/04-Testing_for_Exposed_Session_Variables  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Alat:** `Burp Suite (Proxy/Logger)`, `OWASP ZAP`, `Browser DevTools`, `grep`

### Apakah pengidentifikasi sesi ditransmisikan dalam URL query string?
- [ ] Tidak — pengidentifikasi sesi **tidak** ditemukan dalam URL query string  
- [ ] Ya — pengidentifikasi ditemukan tetapi sesi bersifat jangka pendek atau berisiko rendah  
- [ ] Ya — ID sesi unik **ditemukan** dalam URL query string *(Kritis)*  

### Apakah aplikasi membocorkan token sesi melalui Referer header?
- [ ] Tidak — `Referrer-Policy` mencegah kebocoran atau tidak ada tautan pihak ketiga  
- [ ] Ya — token sesi **ditransmisikan** ke domain pihak ketiga melalui Referer header  

### Apakah variabel sesi disimpan dalam log sisi server atau aplikasi?
- [ ] Tidak — variabel sesi disamarkan (masked) atau dikecualikan dari log  
- [ ] Ya — variabel sesi **tercatat** dalam log aplikasi atau server web dalam bentuk teks polos (plain text)  

### Apakah aplikasi menggunakan permintaan GET untuk operasi yang mengubah status (state-changing operations)?
- [ ] Tidak — aplikasi menggunakan `POST` atau metode non-idempotent lainnya untuk operasi sensitif  
- [ ] Ya — `GET` digunakan, menyebabkan data sesi tersimpan dalam cache riwayat peramban dan proksi perantara  

### Apakah header `Cache-Control` dikonfigurasi untuk mencegah caching data terkait sesi?
- [ ] Ya — `Cache-Control: no-store` (atau serupa) **diterapkan** pada respons sensitif  
- [ ] Ya — kontrol telah diterapkan tetapi bypass **memungkinkan** melalui kasus tepi (edge cases) peramban  
- [ ] Tidak — respons sensitif yang berisi data sesi **dapat** disimpan dalam cache oleh peramban  

---