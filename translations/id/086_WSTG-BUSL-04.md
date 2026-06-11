## WSTG-BUSL-04 — Test for Process Timing

Serangan timing proses (Process timing attacks) melibatkan pengukuran waktu yang dibutuhkan aplikasi untuk memproses permintaan tertentu guna menyimpulkan informasi sensitif tentang status internal atau keberadaan data. Dengan menganalisis perbedaan waktu respons pada tingkat milidetik—yang sering kali disebabkan oleh logika kondisional early-exit, pencarian basis data, atau perbandingan kriptografi non-constant-time—penyerang dapat melakukan analisis side-channel untuk mengenumerasi nama pengguna yang valid, memverifikasi fragmen kata sandi, atau mengonfirmasi keberadaan catatan tertentu. Kerentanan ini biasanya terjadi pada endpoint autentikasi, formulir reset kata sandi, dan filter API yang berat sumber daya di mana logika backend bercabang berdasarkan validitas input. Dari perspektif penyerang, perbedaan waktu ini berfungsi sebagai "boolean oracle" yang mengungkapkan kebenaran tentang data backend bahkan ketika aplikasi mengembalikan pesan kesalahan umum yang identik kepada pengguna.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-BUSL-04 |
| **CWE** | CWE-208 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Medium |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/04-Test_for_Process_Timing  
* https://hacktricks.wiki/en/pentesting-web/timing-attacks.html  
* https://portswigger.net/web-security/race-conditions  

**Alat:** `Burp Suite (Timing Advisor / Logger++)`, `ffuf`, `curl`, `Python (time module)`, `THC-Hydra`

### Apakah aplikasi menunjukkan waktu respons yang bervariasi untuk permintaan sumber daya yang valid vs. tidak valid?
- [ ] Tidak — waktu respons identik terlepas dari validitas input  
- [ ] Ya — terdapat perbedaan waktu yang sangat kecil namun **tidak** signifikan secara statistik  
- [ ] Ya — perbedaan waktu yang konsisten dapat **diamati** dan menyediakan oracle yang andal  

### Apakah fungsi perbandingan waktu-konstan (constant-time comparison functions) atau penundaan buatan diimplementasikan untuk menormalkan waktu respons?
- [ ] Ya — algoritma waktu-konstan **diterapkan** pada semua operasi perbandingan sensitif *(Paling aman)*  
- [ ] Ya — penundaan buatan atau jitter **diaktifkan**, tetapi logika yang mendasarinya tetap non-constant-time  
- [ ] Tidak — tidak ada mitigasi timing yang **diterapkan**, memungkinkan pengukuran langsung pada cabang logika  

### Dapatkah penyerang menggunakan analisis statistik untuk mengisolasi sinyal timing dari noise jaringan?
- [ ] Tidak — jitter jaringan dan noise aplikasi membuat analisis timing **tidak mungkin dilakukan**  
- [ ] Ya — dengan ukuran sampel yang cukup, sinyal timing **dapat** diisolasi dari noise  
- [ ] Ya — selisih timing cukup besar sehingga normalisasi statistik **tidak** diperlukan  

### Apakah enumerasi akun (account enumeration) dimungkinkan melalui fungsionalitas login atau reset kata sandi?
- [ ] Tidak — keberadaan akun tidak dapat ditentukan melalui timing  
- [ ] Ya — perbedaan timing memungkinkan penyerang jarak jauh untuk **mengenumerasi** nama pengguna atau email yang valid  

### Apakah operasi yang intensif sumber daya (misalnya, pemrosesan file, pencarian kompleks) membocorkan keberadaan data melalui timing?
- [ ] Tidak — konsumsi sumber daya seragam terlepas dari apakah sebuah catatan ditemukan atau tidak  
- [ ] Ya — keberadaan catatan sensitif **dapat** disimpulkan dengan mengukur durasi pemrosesan backend  

---