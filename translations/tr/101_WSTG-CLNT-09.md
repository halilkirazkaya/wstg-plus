## WSTG-CLNT-09 — Clickjacking Testi

Clickjacking (Kullanıcı Arayüzü Düzenleme veya UI Redressing olarak da bilinir), bir saldırganın, kullanıcının üst seviye bir sayfaya tıkladığını sanmasını sağlayarak, şeffaf veya opak katmanlar aracılığıyla başka bir sayfadaki bir butona veya bağlantıya tıklamaya zorlaması durumunda ortaya çıkar. Bu güvenlik açığı, kullanıcı eylemlerinin bütünlüğünü bozarak, mağdur adına yetkisiz veri değişikliğine, hesap değişikliklerine veya finansal transferlere yol açabilir. Saldırganlar genellikle hedef web sitesini, kontrol ettikleri kötü niyetli bir sitedeki bir `<iframe>` içine yerleştirerek ve üzerini aldatıcı içeriklerle kapatarak bu açıktan faydalanırlar. Bu durum en sık "Hesabı Sil" butonları, parola sıfırlama formları veya yönetici yapılandırma panelleri gibi hassas durum değiştiren eylemlerin gerçekleştirildiği sayfalarda gözlemlenir.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CLNT-09 |
| **CWE** | CWE-1021 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek* |

> *Hassas eylemler (örneğin; para transferleri, hesap silme veya yetki yükseltme) Clickjacking yoluyla tetiklenebiliyorsa önem derecesi Yüksek olur.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/09-Testing_for_Clickjacking  
* https://hacktricks.wiki/en/pentesting-web/clickjacking.html  
* https://portswigger.net/web-security/clickjacking  

**Araçlar:** `Burp Suite Clickbandit`, `Browser Developer Tools`, `OWASP ZAP`, `Online Clickjack Test`

### Uygulama, yetkisiz çerçevelemeyi (framing) önlemek için sunucu tarafı başlıkları (headers) uyguluyor mu?
- [ ] Evet — `X-Frame-Options` veya `Content-Security-Policy` **uygulanmıştır** ve çerçevelemeyi engeller *(En güvenli)*  
- [ ] Evet — başlıklar mevcuttur ancak **yanlış yapılandırılmıştır** (örneğin, geniş tarayıcı desteği bulunmayan `X-Frame-Options: ALLOW-FROM`)  
- [ ] Hayır — herhangi bir çerçeveleme koruma başlığı **uygulanmamıştır**  

### `Content-Security-Policy` (CSP) `frame-ancestors` direktifi uygulanmış mı?
- [ ] Evet — `frame-ancestors 'none'` veya `'self'` **uygulanmıştır**  
- [ ] Evet — `frame-ancestors` mevcuttur ancak güvenilmeyen veya aşırı geniş kaynaklara (origins) **izin vermektedir**  
- [ ] Hayır — direktif **eksiktir** veya CSP uygulanmamıştır  

### `X-Frame-Options` (XFO) başlığı, eski tarayıcı desteği için doğru yapılandırılmış mı?
- [ ] Evet — `DENY` veya `SAMEORIGIN` **uygulanmıştır**  
- [ ] Evet — XFO mevcuttur ancak bir CSP `frame-ancestors` direktifi bulunduğu için modern tarayıcılar tarafından **yok sayılmaktadır**  
- [ ] Hayır — XFO başlığı **eksiktir** veya güvensiz bir değere ayarlanmıştır  

### Çerçeveleme (framing) korumaları bilinen tekniklerle atlatılabilir mi (bypass)?
- [ ] Hayır — standart tekniklerle (double-framing vb.) atlatma **mümkün değildir**  
- [ ] Evet — `frame-ancestors` beyaz listesindeki (white-list) zayıf düzenli ifade (regex) veya mantık hatası nedeniyle koruma **atlatılabilir**  
- [ ] Evet — uygulama yalnızca JavaScript tabanlı çerçeve kırıcıya (frame-busting) güvendiği için koruma **atlatılabilir**  

### Hassas bir eylem, bir Clickjacking kavram kanıtı (proof-of-concept) aracılığıyla istismar edilebilir mi?
- [ ] Hayır — çerçeveleme engellenmiştir veya hassas eylemlere ulaşılamamaktadır  
- [ ] Evet — kullanıcı arayüzü düzenleme (UI redressing) **mümkündür** ancak karmaşık kullanıcı etkileşimi gerektirir  
- [ ] Evet — kullanıcı arayüzü düzenleme (UI redressing) **mümkündür** ve anında durum değiştiren eylemlere izin verir *(Kritik)*  

---