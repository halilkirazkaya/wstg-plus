## WSTG-INPV-19 — Testing for Server-Side Request Forgery (SSRF)

Server-Side Request Forgery (SSRF) terjadi ketika penyerang dapat memengaruhi aplikasi web untuk membuat permintaan yang tidak sah dari server ke sumber daya internal atau eksternal. Dengan memanipulasi parameter yang mengharapkan URL, hostname, atau alamat IP, penyerang dapat melewati kontrol tingkat jaringan seperti firewall dan ACL untuk melakukan probe pada segmen jaringan internal, mengakses layanan metadata cloud (IMDS), atau berinteraksi dengan API internal dan basis data yang rentan. Kerentanan ini biasanya muncul pada fitur-fitur yang melibatkan pengambilan gambar (image fetching), pembuatan PDF (PDF generation), webhook, atau unggahan berkas melalui URL. Dari sudut pandang penyerang, SSRF adalah primitif yang kuat yang digunakan untuk melakukan pivot ke lingkungan yang terisolasi, mengeksfiltrasi data konfigurasi yang sensitif, atau berpotensi mencapai eksekusi kode jarak jauh (Remote Code Execution) melalui interaksi dengan layanan internal seperti Redis atau Memcached.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-19 |
| **CWE** | CWE-918 |
| **Status Pengujian** | Belum Dilakukan |
| **Tingkat Keparahan** | Tinggi / Kritis |

**Referensi:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/19-Testing_for_Server-Side_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/ssrf-server-side-request-forgery/index.html  
* https://portswigger.net/web-security/ssrf  

**Alat:** `Burp Suite (Collaborator/Interactsh)`, `Interact.sh`, `Gopherus`, `ffuf`, `cURL`, `DNSRebind Tool`

### Apakah aplikasi memproses URL atau hostname yang diberikan oleh pengguna?
- [ ] Tidak — tidak ada fitur yang menerima URL atau hostname eksternal  
- [ ] Ya — fitur tersedia tetapi input **divalidasi secara ketat** terhadap daftar izin (allow-list)  
- [ ] Ya — fitur tersedia dan menerima URL yang diberikan pengguna  

### Apakah kontrol validasi atau daftar blokir diterapkan pada tujuan yang diminta?
- [ ] Ya — sebuah **allow-list** ketat untuk domain/IP tepercaya diterapkan  
- [ ] Ya — sebuah **deny-list** digunakan (misalnya, memblokir `127.0.0.1`, `169.254.169.254`)  
- [ ] No — **tidak ada validasi** atau pemfilteran yang diterapkan pada input  

### Apakah mungkin untuk melewati filter melalui encoding, redirect, atau DNS rebinding?
- [ ] Tidak — filter kuat dan menangani redirect/resolusi DNS dengan aman  
- [ ] Ya — bypass **dimungkinkan** menggunakan URL encoding atau format IP alternatif (hex, octal)  
- [ ] Ya — bypass **dimungkinkan** melalui HTTP redirect 3xx ke target internal  
- [ ] Ya — bypass **dimungkinkan** melalui serangan DNS rebinding untuk melakukan resolusi ke IP internal  

### Dapatkah aplikasi menjangkau layanan metadata internal atau antarmuka lokal?
- [ ] Tidak — jaringan lokal dan endpoint metadata cloud **tidak dapat dijangkau**  
- [ ] Ya — akses ke localhost (`127.0.0.1`) atau layanan loopback **dimungkinkan**  
- [ ] Ya — akses ke layanan metadata cloud (misalnya, AWS/GCP/Azure IMDS) **dimungkinkan** *(Kritis)*  

### Bagaimana karakteristik respons SSRF tersebut?
- [ ] Blind — tidak ada respons atau metadata yang dikembalikan ke pengguna  
- [ ] Partial — metadata respons (header, ukuran, waktu) dikembalikan ke pengguna  
- [ ] Full — seluruh isi respons (response body) dari sumber daya internal **direfleksikan** ke pengguna  

---