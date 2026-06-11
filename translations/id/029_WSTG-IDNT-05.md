## WSTG-IDNT-05 — Pengujian Kebijakan Nama Pengguna (Username) yang Lemah atau Tidak Diterapkan

Pengujian terhadap kebijakan nama pengguna (username) yang lemah atau tidak diterapkan berfokus pada aturan yang mengatur pembuatan pengenal akun dan kerentanannya terhadap enumerasi atau spoofing. Kebijakan yang lemah sering kali mengizinkan urutan yang dapat diprediksi, kata-kata kamus umum, atau pengenal yang mencerminkan ID karyawan internal, yang secara signifikan mengurangi ruang pencarian untuk serangan Brute Force dan Credential Stuffing. Dari sudut pandang penyerang, mengidentifikasi pola-pola ini adalah langkah pertama dalam penemuan akun skala besar, yang sering kali dicapai dengan menganalisis pesan kesalahan pendaftaran, perilaku pengaturan ulang kata sandi (password reset), atau metadata dalam profil publik. Memastikan kebijakan yang kuat mencegah penyerang memetakan basis pengguna secara sistematis dan memfasilitasi pertahanan terhadap Phishing yang ditargetkan serta upaya bypass autentikasi otomatis.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-IDNT-05 |
| **CWE** | CWE-521 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Rendah / Sedang |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/05-Testing_for_Weak_or_Unenforced_Username_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite (Intruder)`, `ffuf`, `Custom Python Scripts`, `theHarvester`

### Apakah nama pengguna didasarkan pada pola yang sangat mudah diprediksi atau berurutan?
- [ ] Tidak — nama pengguna bersifat acak, ditentukan oleh pengguna, atau merupakan string dengan entropi tinggi  
- [ ] Ya — nama pengguna mengikuti format yang dapat diprediksi (misalnya, `user1001`, `user1002`) namun autentikasi bersifat kuat  
- [ ] Ya — nama pengguna bersifat **sangat berurutan** atau mengikuti format korporat yang diketahui, sehingga memudahkan enumerasi  

### Apakah aplikasi menerapkan persyaratan panjang minimum dan kompleksitas untuk nama pengguna?
- [ ] Ya — kebijakan panjang dan set karakter yang ketat **diterapkan** selama pendaftaran  
- [ ] Ya — kebijakan tersedia tetapi mengizinkan nama pengguna yang sangat pendek (misalnya, 1-2 karakter) atau terlalu sederhana  
- [ ] Tidak — **tidak ada kebijakan** yang diterapkan terkait panjang nama pengguna atau jenis karakter  

### Dapatkah nama pengguna yang valid dienumerasi melalui respons aplikasi?
- [ ] Tidak — aplikasi mengembalikan pesan kesalahan generik dan menunjukkan waktu respons yang konsisten untuk semua upaya  
- [ ] Ya — aplikasi mengembalikan pesan kesalahan yang berbeda (misalnya, "Nama pengguna sudah digunakan") tetapi Rate Limiting **diterapkan**  
- [ ] Ya — aplikasi **membocorkan** nama pengguna yang valid melalui kesalahan pendaftaran, pengaturan ulang kata sandi (password reset), atau perbedaan waktu (timing differences)  

### Apakah kebijakan mencegah pendaftaran nama pengguna administratif yang umum atau dicadangkan (reserved)?
- [ ] Ya — aplikasi memblokir nama-nama umum (misalnya, `admin`, `root`, `support`) dan istilah yang dicadangkan oleh sistem  
- [ ] Tidak — pengguna **dapat** mendaftarkan akun dengan nama yang sensitif atau administratif  

### Apakah kebijakan nama pengguna konsisten di semua titik masuk (API, Mobile, Web)?
- [ ] Ya — kebijakan **diterapkan** secara konsisten di semua antarmuka dan versi  
- [ ] Tidak — endpoint lama (legacy) atau versi API tertentu **tidak** menerapkan batasan nama pengguna yang sama  

---