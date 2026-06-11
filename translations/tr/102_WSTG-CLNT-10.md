## WSTG-CLNT-10 — WebSocket Testi

WebSocket testi, geleneksel HTTP istek-yanıt modellerini devre dışı bırakan, istemci ile sunucu arasında kurulan tam çift yönlü (full-duplex) iletişim kanalına odaklanır. Sızma testi uzmanları (Pentesters); el sıkışma (Handshake) sürecinin güvenliğini, Cross-Site WebSocket Hijacking (CSWSH) korumalarının mevcudiyetini ve mesaj yüklerine (Payload) uygulanan girdi doğrulamanın (Input Validation) titizliğini değerlendirir. İstismar (Exploitation) süreci tipik olarak, verileri gerçek zamanlı olarak sızdırmak (exfiltrate) için aktif bir oturumun ele geçirilmesini veya kalıcı soket üzerinden Komut Enjeksiyonu (Command Injection) veya SQLi gibi sunucu tarafı zafiyetlerini tetikleyen kötü niyetli yüklerin (Payload) enjekte edilmesini içerir. Birçok geleneksel Web Uygulaması Güvenlik Duvarı (WAF), WebSocket trafiğini etkili bir şekilde denetlemediğinden, bu protokol genellikle çevre savunmalarını atlatmak (Bypass) için gizli bir vektör sağlar.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CLNT-10 |
| **CWE** | CWE-1385 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Orta |

**Kaynaklar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/10-Testing_WebSockets  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/websockets  

**Araçlar:** `Burp Suite (WebSockets tab)`, `OWASP ZAP`, `wscat`, `Socket.io-client`, `Wireshark`

### WebSocket bağlantısı güvenli bir kanal üzerinden mi kuruluyor?
- [ ] Evet — Tüm bağlantılar için `wss://` (WebSocket Secure) kullanımı zorunlu kılınmıştır.
- [ ] Hayır — `ws://` kullanılıyor; bu durum aradaki adam (person-in-the-middle - PITM) dinlemesine olanak tanır.

### Uygulama, Cross-Site WebSocket Hijacking (CSWSH) saldırılarına karşı korunuyor mu?
- [ ] Hayır — Sunucu, el sıkışma (Handshake) sırasında `Origin` başlığını (Header) sıkı bir şekilde doğrular.
- [ ] Evet — `Origin` başlığı doğrulaması zayıf veya null/sahte origin'ler aracılığıyla atlatılabilir (Bypass).
- [ ] Evet — `Origin` başlığı doğrulaması yapılmıyor; bu durum siteler arası saldırganların bağlantı başlatmasına olanak tanıyor *(Kritik)*.

### WebSocket mesaj yüklerine (Payload) girdi doğrulaması (Input Validation) ve temizlemesi (Sanitization) uygulanıyor mu?
- [ ] Evet — Gelen tüm mesajlar temizlenir ve bir şemaya karşı doğrulanır.
- [ ] Evet — Bazı doğrulamalar mevcut ancak kodlanmış veya standart dışı yükler (Payload) kullanılarak atlatılması (Bypass) mümkündür.
- [ ] Hayır — Mesajlar doğrudan işlenir; bu durum enjeksiyona (XSS, SQLi, RCE) veya mantık manipülasyonuna izin verir.

### Her WebSocket mesajında kimlik doğrulama (Authentication) ve yetkilendirme (Authorization) kontrolleri yapılıyor mu?
- [ ] Evet — Her mesaj için oturum doğrulanır veya protokol token'lar ile durumsuz (Stateless) çalışır.
- [ ] Evet — Kimlik doğrulama el sıkışma (Handshake) sırasında gerçekleşir ancak belirli eylemler için yetkilendirme (Authorization) mesaj başına zorunlu kılınmaz.
- [ ] Hayır — Kimlik doğrulama yalnızca el sıkışma (Handshake) sırasında kontrol edilir; bu durum oturumun ele geçirilmesine (Session Hijacking) ve tam erişim sağlanmasına olanak tanır.

### Sunucu, WebSocket üzerinde hız sınırlama (Rate Limiting) veya kaynak kısıtlamaları uyguluyor mu?
- [ ] Evet — Hız sınırlama (Rate Limiting) ve maksimum mesaj boyutları etkinleştirilmiş ve zorunlu kılınmıştır.
- [ ] Hayır — Hiçbir sınır mevcut değil; mesaj yağmuru (flooding) veya büyük mesaj boyutları aracılığıyla Hizmet Dışı Bırakma (DoS) saldırılarına olanak tanır.

---