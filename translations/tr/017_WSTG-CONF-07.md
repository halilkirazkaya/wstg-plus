## WSTG-CONF-07 — HTTP Strict Transport Security Testi

HTTP Strict Transport Security (HSTS), bir web sunucusunun tarayıcılara kendisine yalnızca HTTPS üzerinden erişilmesi gerektiğini bildirmesini sağlayan; protokol düşürme saldırılarını (protocol downgrade attacks) ve çerez ele geçirmeyi (cookie hijacking) engelleyen bir güvenlik mekanizmasıdır. Şifrelenmiş bir bağlantıyı zorunlu kılarak HSTS, bir saldırganın TLS'e geçişi önlemek için başlangıçtaki güvensiz bir HTTP isteğine müdahale ettiği SSLStrip gibi Aradaki Adam (Man-in-the-Middle - MitM) saldırılarını azaltır. Saldırgan açısından bakıldığında, bu başlığın eksikliği veya kısa bir `max-age` süresi, ilk açık metin (cleartext) el sıkışması (handshake) sırasında hassas oturum belirteçlerini (session tokens) veya kimlik bilgilerini ele geçirmek için bir fırsat penceresi sunar. Bu test, tüm hassas uygulama uç noktalarında `Strict-Transport-Security` başlığının varlığını, süresini ve kapsamını doğrulamaya odaklanır.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CONF-07 |
| **CWE** | CWE-319 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Düşük / Orta* |

> *Uygulama; PII (Kişisel Veri), oturum belirteçleri veya kimlik bilgilerini işliyorsa, HSTS eksikliği MitM saldırılarının başarı oranını artırdığından önem derecesi Orta (Medium) olur.

**Kaynaklar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/07-Test_HTTP_Strict_Transport_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `curl`, `Burp Suite`, `nmap`, `SSLLabs`, `hsts-preload-checker`

### `Strict-Transport-Security` başlığı HTTPS yanıtlarında mevcut mu?
- [ ] Evet — başlık tüm hassas uç noktalarda **mevcut**  
- [ ] Evet — başlık **mevcut** ancak yalnızca giriş sayfasında (landing page)  
- [ ] Hayır — başlık uygulamada tamamen **eksik**  

### `max-age` direktifi yeterli bir süreye ayarlanmış mı?
- [ ] Evet — `max-age` bir yıl veya daha fazlasına ayarlanmış (örneğin, `31536000`)  
- [ ] Evet — `max-age` ayarlanmış ancak **çok kısa** (örneğin, 6 aydan az)  
- [ ] Hayır — `max-age` direktifi **eksik** veya `0` olarak ayarlanmış *(Kritik)*  

### HSTS politikası tüm alt alan adlarını (subdomains) kapsıyor mu?
- [ ] Evet — `includeSubDomains` direktifi **uygulanmış**  
- [ ] Hayır — `includeSubDomains` direktifi **eksik**, alt alan adları protokol düşürmeye karşı savunmasız bırakılmış  
- [ ] Hayır — alt alan adı bulunmadığından bu özellik geçerli değil  

### Uygulama HSTS Ön Yükleme (Preloading) için kayıtlı mı?
- [ ] Evet — `preload` direktifi **mevcut** ve alan adı HSTS ön yükleme listesinde  
- [ ] Evet — `preload` direktifi **mevcut** ancak alan adı henüz ön yükleme listesinde **değil**  
- [ ] Hayır — `preload` direktifi **eksik**, ilk ziyarette MitM saldırısı için bir açık kapı bırakıyor  

### HSTS başlığı hatalı bir şekilde açık metin HTTP üzerinden mi gönderiliyor?
- [ ] Hayır — başlık **yalnızca** güvenli HTTPS bağlantıları üzerinden gönderiliyor  
- [ ] Evet — başlık HTTP üzerinden **gönderiliyor**; bu durum tarayıcılar tarafından yok sayılır ve yapılandırma ayrıntılarını sızdırabilir  

---