## WSTG-INPV-13 — Format String Injection

Format string injection (Injeksi string format) terjadi ketika aplikasi web meneruskan masukan yang dikendalikan pengguna secara langsung ke dalam argumen string format dari fungsi variadik (variadic function), seperti keluarga `printf` pada bahasa C atau pembungkus logging tingkat rendah (low-level logging wrappers). Meskipun kurang umum pada kerangka kerja web kode terkelola (managed-code) modern, kerentanan ini tetap kritis pada aplikasi CGI lama (legacy), ekstensi native, atau sistem logging kasus tepi (edge-case) yang berinteraksi dengan pustaka sistem tingkat rendah. Penyerang mengeksploitasi hal ini dengan memberikan spesifikasi format (format specifiers) seperti `%x` atau `%p` untuk membocorkan alamat memori sensitif dan data dari stack, atau spesifikasi `%n` untuk melakukan penulisan memori arbitrer (arbitrary memory writes). Eksploitasi yang berhasil biasanya berujung pada kompromi sistem secara penuh melalui Remote Code Execution (RCE) atau, setidaknya, kondisi Denial-of-Service (DoS) yang berdampak melalui kegagalan proses (crash).


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-INPV-13 |
| **CWE** | CWE-134 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi / Kritis |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/13-Testing_for_Format_String_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `GDB`, `strings`, `radare2`, `Checksec`, `AFL++`

### Apakah aplikasi memproses masukan pengguna melalui kode native atau antarmuka CGI lama?
- [ ] Tidak — aplikasi dibangun sepenuhnya pada kerangka kerja kode terkelola (misalnya, JVM, CLR)  
- [ ] Ya — aplikasi menggunakan JNI, ekstensi C/C++ native, atau CGI biner lama di mana kerentanan Format String **dapat** ada  

### Apakah masukan yang diberikan pengguna diteruskan sebagai argumen utama ke fungsi yang peka terhadap format (format-aware functions)?
- [ ] Tidak — masukan diteruskan sebagai argumen data, dan string format bersifat **statis** serta **hardcoded**  
- [ ] Ya — masukan pengguna digabungkan langsung ke dalam argumen string format, namun sanitisasi **telah diterapkan**  
- [ ] Ya — masukan pengguna digunakan secara langsung sebagai string format dan tidak ada sanitisasi yang **diterapkan**  

### Apakah mungkin untuk membocorkan konten memori menggunakan spesifikasi format?
- [ ] Tidak — masukan divalidasi dan spesifikasi seperti `%p`, `%x`, atau `%s` **tidak diproses**  
- [ ] Ya — pemberian spesifikasi `%p` atau `%x` menghasilkan refleksi alamat memori stack atau heap  
- [ ] Ya — data sensitif (misalnya, pointer, stack cookies) **dapat** dieksfiltrasi melalui spesifikasi format  

### Apakah aplikasi dapat dibuat crash atau dimanipulasi menggunakan spesifikasi `%n`?
- [ ] Tidak — spesifikasi yang mengizinkan penulisan memori **dinonaktifkan** atau diblokir oleh compiler/lingkungan  
- [ ] Ya — aplikasi mengalami crash (DoS) ketika `%s%s%s%s` atau string serupa diberikan  
- [ ] Ya — penulisan memori arbitrer via `%n` **mungkin dilakukan**, yang berpotensi menyebabkan Remote Code Execution (RCE)  

### Apakah perlindungan biner modern (ASLR, DEP, Stack Canaries) dapat dilewati (bypass) melalui injeksi ini?
- [ ] Tidak — lingkungan telah diperkeras dan kebocoran memori **tidak dapat** digunakan untuk melewati perlindungan  
- [ ] Ya — Format String Injection **dapat** digunakan untuk membocorkan alamat dasar, sehingga bypass ASLR/DEP **mungkin dilakukan**  

---