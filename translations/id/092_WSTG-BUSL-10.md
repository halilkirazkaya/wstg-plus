## WSTG-BUSL-10 — Menguji Fungsionalitas Pembayaran

Pengujian fungsionalitas pembayaran melibatkan identifikasi cacat logika bisnis (business logic flaws) yang memungkinkan penyerang melewati gateway pembayaran (payment gateways), memanipulasi total pesanan, atau memalsukan sinyal transaksi yang berhasil. Kerentanan ini biasanya terletak pada komunikasi antara aplikasi, browser klien, dan pemroses pembayaran pihak ketiga seperti Stripe, PayPal, atau sistem buku besar internal. Penyerang mungkin mencoba memodifikasi parameter harga saat dalam pengiriman (in transit), mengirimkan jumlah negatif untuk mengurangi total biaya, atau melakukan replay pada callback transaksi yang berhasil untuk memenuhi pesanan tanpa pembayaran yang sebenarnya. Eksploitasi yang berhasil menyebabkan kerugian finansial langsung, ketidaksesuaian inventaris, dan potensi kompromi pada integritas e-commerce aplikasi.


| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-BUSL-10 |
| **CWE** | CWE-840 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi / Kritis |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/10-Test-Payment-Functionality  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite`, `Turbo Intruder`, `Postman`, `Hackvertor`

### Apakah jumlah transaksi divalidasi di sisi server sebelum diproses?
- [ ] Ya — jumlah divalidasi secara ketat terhadap database produk di sisi server *(Paling aman)*  
- [ ] Ya — jumlah divalidasi tetapi bypass logika **mungkin terjadi** melalui parameter pollution atau penggantian mata uang  
- [ ] Tidak — jumlah transaksi **dapat** dimodifikasi dalam permintaan sisi klien dan diterima oleh backend  

### Apakah jumlah item dapat dimanipulasi sehingga menghasilkan total nol atau negatif?
- [ ] Tidak — jumlah negatif atau nol ditolak oleh logika validasi aplikasi  
- [ ] Ya — jumlah negatif diterima di keranjang tetapi total diperbaiki saat checkout  
- [ ] Ya — jumlah negatif **dapat** digunakan untuk mengurangi total pesanan secara keseluruhan atau mendapatkan kredit *(Kritis)*  

### Apakah aplikasi menangani callback atau webhook gateway pembayaran dengan aman?
- [ ] Ya — callback memerlukan tanda tangan kriptografi valid yang **tidak dapat** dipalsukan  
- [ ] Ya — tanda tangan diperiksa tetapi rahasia (secret) lemah atau verifikasi **dapat** dilewati  
- [ ] Tidak — aplikasi mengandalkan callback yang tidak terautentikasi atau tidak terverifikasi untuk mengonfirmasi status pembayaran  

### Apakah aplikasi rentan terhadap race conditions selama fase checkout atau pembayaran?
- [ ] Tidak — integritas transaksional dan mekanisme penguncian database **diaktifkan**  
- [ ] Ya — race conditions **mungkin terjadi** tetapi tidak berdampak pada harga akhir atau pemenuhan pesanan  
- [ ] Ya — permintaan konkuren **dapat** menyebabkan beberapa pemenuhan pesanan untuk satu pembayaran atau "double-spending" pada kartu hadiah (gift cards)  

### Apakah kode diskon atau poin loyalitas dapat dimanipulasi atau digunakan kembali?
- [ ] Tidak — kode divalidasi untuk satu kali penggunaan dan batasan logika **diterapkan**  
- [ ] Ya — kode dapat digunakan kembali atau diterapkan pada item yang tidak memenuhi syarat, tetapi dampaknya terbatas  
- [ ] Ya — penyerang **dapat** menerapkan beberapa kode yang bertentangan atau melewati batas kedaluwarsa dan penggunaan  

---