## WSTG-BUSL-05 — Menguji Batas Berapa Kali Fungsi Dapat Digunakan

Pengujian ini mengevaluasi apakah aplikasi menerapkan batasan logika bisnis (business logic) pada frekuensi dan jumlah total berapa kali tindakan atau fungsi tertentu dapat dijalankan oleh satu pengguna, sesi, atau alamat IP. Kegagalan dalam menerapkan batasan ini memungkinkan penyerang untuk menyalahgunakan fungsi bernilai tinggi—seperti pengiriman SMS OTP, memicu email pengaturan ulang kata sandi (password reset), menerapkan kode diskon, atau melakukan ekspor data yang berat—yang menyebabkan kerugian finansial, kehabisan sumber daya (resource exhaustion), atau kerusakan reputasi. Kerentanan ini biasanya ditemukan pada endpoint transaksional atau fitur komunikasi dan dieksploitasi melalui skrip otomatis yang mengulangi tindakan jauh melampaui ambang batas bisnis yang dimaksudkan. Dari perspektif penyerang, ini adalah target utama untuk SMS pumping, mail bombing, atau alur kerja Brute Force yang tidak memiliki Rate Limiting atau pembatasan berbasis jumlah yang eksplisit.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-BUSL-05 |
| **CWE** | CWE-799 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Sedang / Tinggi* |

> *Keparahan menjadi Tinggi jika fungsi tersebut melibatkan transaksi keuangan, biaya SMS, atau berdampak pada ketersediaan sistem secara keseluruhan.

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/05-Test_Number_of_Times_a_Function_Can_Be_Used_Limits  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite (Intruder/Repeater)`, `Turbo Intruder`, `Python (requests)`, `OWASP ZAP`

### Apakah aplikasi menentukan dan menerapkan batasan pada fungsi bisnis yang sensitif?
- [ ] Ya — batasan diterapkan secara ketat dan bypass **tidak mungkin dilakukan**  
- [ ] Ya — batasan diterapkan tetapi terlalu tinggi untuk mencegah penyalahgunaan  
- [ ] Tidak — tidak ada batasan yang diterapkan pada eksekusi fungsi  

### Apakah pembatasan laju (rate limits) dan penghitung penggunaan diterapkan di sisi server?
- [ ] Ya — penerapan dilakukan secara ketat di sisi server (server-side) dan **tidak dapat** dimanipulasi  
- [ ] Tidak — penerapan bergantung pada logika sisi klien (client-side), misalnya JavaScript atau field tersembunyi  
- [ ] Tidak — tidak ada penerapan batasan di level mana pun  

### Dapatkah batasan penggunaan dilewati melalui manipulasi header atau sesi?
- [ ] Tidak — bypass melalui `X-Forwarded-For`, `Origin`, atau rotasi sesi **tidak mungkin dilakukan**  
- [ ] Ya — batasan **dapat** dilewati dengan melakukan spoofing pada header IP atau menghapus cookie  
- [ ] Ya — batasan **dapat** dilewati dengan membuat banyak akun atau sesi  

### Apakah melampaui batas memicu respons pertahanan yang tepat?
- [ ] Ya — aplikasi mengembalikan `429 Too Many Requests` dan mencatat log kejadian tersebut *(Paling aman)*  
- [ ] Ya — aplikasi mengembalikan kesalahan tetapi **tidak** mencatat log potensi penyalahgunaan  
- [ ] Tidak — aplikasi terus memproses permintaan tetapi mengabaikan output-nya  
- [ ] Tidak — aplikasi tidak memberikan respons atau mengalami crash di bawah beban tinggi  

### Apakah dampak dari penyalahgunaan fungsi tersebut signifikan bagi bisnis?
- [ ] Tidak — penyalahgunaan fungsi memiliki biaya atau dampak sumber daya yang sangat kecil  
- [ ] Ya — penyalahgunaan **dapat** menyebabkan konsumsi sumber daya minor atau gangguan pada pengguna  
- [ ] Ya — penyalahgunaan **dapat** menyebabkan biaya finansial yang signifikan (misalnya biaya SMS) atau penolakan layanan (Denial of Service) *(Kritis)*  

---