## WSTG-BUSL-02 — Menguji Kemampuan Memalsukan Permintaan (Test Ability to Forge Requests)

Memalsukan permintaan (forging requests) dalam konteks logika bisnis (business logic) melibatkan manipulasi parameter yang ditentukan aplikasi untuk melakukan tindakan di luar alur kerja (workflow) atau tingkat otorisasi (authorization) yang dimaksudkan. Penyerang menargetkan proses multi-tahap, seperti sistem checkout atau pendaftaran akun, di mana status (state) dipertahankan melalui parameter sisi klien (client-side) yang gagal divalidasi ulang oleh server terhadap sumber backend yang tepercaya. Dengan mengubah bidang tersembunyi (hidden fields), nilai JSON, atau parameter REST seperti harga (prices), jumlah (quantities), atau bendera status internal (internal status flags), penyerang dapat melewati persyaratan pembayaran, meningkatkan hak istimewa (escalate privileges), atau memicu perubahan status yang tidak diinginkan. Kerentanan ini biasanya muncul pada formulir web stateful atau API di mana server secara implisit memercayai representasi klien tentang status bisnis selama transaksi.

| Bidang | Nilai |
|---|---|
| **WSTG ID** | WSTG-BUSL-02 |
| **CWE** | CWE-602 |
| **Status Pengujian** | Belum Dilakukan |
| **Keparahan** | Tinggi / Kritis* |

> *Tingkat keparahan menjadi Kritis jika permintaan yang dipalsukan memungkinkan kerugian finansial, tindakan administratif yang tidak sah, atau pengambilalihan akun secara penuh (full account takeover).

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/02-Test_Ability_to_Forge_Requests  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Alat:** `Burp Suite (Proxy, Repeater, Intruder)`, `Postman`, `OWASP ZAP`, `CyberChef`

### Apakah aplikasi memvalidasi parameter transaksional terhadap "source of truth" di sisi server?
- [ ] Ya — semua parameter sensitif (harga, peran, ID) divalidasi terhadap database dan bypass **tidak dimungkinkan**  
- [ ] Ya — validasi diterapkan tetapi dapat dilewati melalui parameter pollution atau pengkodean alternatif  
- [ ] No — aplikasi memercayai nilai sisi klien untuk menentukan biaya atau hasil transaksi *(Kritis)*  

### Apakah nilai-nilai kritis bisnis (misalnya, jumlah, nominal) dapat diubah menjadi nilai yang tidak sah?
- [ ] Tidak — nilai negatif, nilai nol, atau nilai yang sangat tinggi ditolak oleh server  
- [ ] Ya — server menerima nilai yang dimodifikasi tetapi hanya dalam rentang terbatas  
- [ ] Ya — nilai negatif atau nol **dapat** dikirimkan, yang menyebabkan bypass finansial atau logika  

### Apakah bidang tersembunyi atau parameter API digunakan untuk mempertahankan status di seluruh proses multi-tahap?
- [ ] Tidak — status dipertahankan secara aman melalui sesi sisi server (server-side sessions) atau token yang ditandatangani  
- [ ] Ya — status diteruskan melalui klien tetapi pemeriksaan integritas (HMAC/Signatures) **diaktifkan** dan kuat  
- [ ] Ya — status diteruskan melalui klien dan pemeriksaan integritas **dinonaktifkan** atau dapat dihapus  

### Apakah mungkin untuk memalsukan permintaan yang melewatkan langkah-langkah perantara wajib dalam suatu alur kerja (workflow)?
- [ ] Tidak — server memaksakan urutan operasi yang ketat untuk setiap transaksi  
- [ ] Ya — langkah-langkah **dapat** dilewati tetapi tindakan akhir tetap memerlukan data yang valid dari langkah-langkah sebelumnya  
- [ ] Ya — permintaan "submit" atau "complete" akhir **dapat** dipalsukan secara langsung untuk melewati langkah otorisasi atau pembayaran  

---