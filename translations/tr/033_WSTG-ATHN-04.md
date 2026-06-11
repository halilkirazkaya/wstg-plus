## WSTG-ATHN-04 — Kimlik Doğrulama Şemasını Atlatma Testi (Testing for Bypassing Authentication Schema)

Kimlik doğrulama şemasını atlatma (Bypassing the authentication schema), bir saldırganın geçerli kimlik bilgileri (credentials) sağlamadan korunan kaynaklara erişmesine izin veren uygulama mantığındaki açıkların tespit edilmesini ve istismar edilmesini içerir. Bu zafiyet genellikle uygulamanın istemci taraflı kontrollere (client-side controls) güvenmesi, her istekte sunucu taraflı yetkilendirmeyi (server-side authorization) zorunlu kılmaması veya üretim ortamında yönetici "arka kapılarını" (backdoors) ve hata ayıklama (debug) uç noktalarını (endpoints) açık bırakması durumunda ortaya çıkar. Saldırganlar; hassas URL'lere zorlamalı gezinti (Forced Browsing) yaparak, `authenticated=true` gibi HTTP parametrelerini manipüle ederek veya uygulamanın bir oturumun (Session) halihazırda kurulduğunu varsaymasını sağlamak için üstbilgi sahteciliği (Header Spoofing) yaparak bu zayıflıkları istismar ederler. Başarılı bir istismar (Exploit), kimlik doğrulama sınırının tamamen çökmesine neden olur; bu da potansiyel olarak yetkisiz veri sızdırmasına (Data Exfiltration) veya sistemin tamamen ele geçirilmesine yol açabilir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-ATHN-04 |
| **CWE** | CWE-287 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Kritik |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/04-Testing_for_Bypassing_Authentication_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Araçlar:** `Burp Suite`, `Ffuf`, `Gobuster`, `Dirsearch`, `Postman`

### Hassas dahili sayfalara aktif bir oturum (Session) olmadan doğrudan erişilebiliyor mu?
- [ ] Hayır — doğrudan erişim, giriş sayfasına yönlendirme veya 403/404 yanıtı ile sonuçlanır  
- [ ] Evet — bazı hassas sayfalara erişilebiliyor ancak sınırlı veya hassas olmayan veriler sağlanıyor  
- [ ] Evet — hassas yönetici veya kullanıcıya özel sayfalara kimlik doğrulaması olmadan tam olarak **erişilebiliyor** *(Kritik)*  

### Uygulama, kimlik doğrulama durumunu belirlemek için istemci taraflı bayraklara (flags) veya parametrelere güveniyor mu?
- [ ] Hayır — kimlik doğrulama durumu kesinlikle sunucu taraflı oturum durumu (session state) üzerinden yönetilir  
- [ ] Evet — istemci taraflı bayraklar mevcuttur ancak değişiklik yapılması korunan alanlara erişim **sağlamaz**  
- [ ] Evet — parametreleri (örneğin `is_authenticated=true`, `user_role=admin`) değiştirmek kimlik doğrulamasının atlatılmasına (Authentication Bypass) **izin verir**  

### Belirli HTTP üstbilgileri (headers) manipüle edilerek veya sahtecilik yapılarak kimlik doğrulaması atlatılabilir mi?
- [ ] Hayır — `X-Forwarded-For`, `Referer` veya özel üstbilgilerin (custom headers) kimlik doğrulama üzerinde hiçbir etkisi yoktur  
- [ ] Evet — üstbilgileri (örneğin `X-Custom-IP-Authorization`, `X-Remote-User`) manipüle etmek **mümkündür** ve yetkisiz erişim sağlar  

### Standart giriş akışını (login flow) devre dışı bırakan alternatif yollar veya geliştirme artıkları (artifacts) var mı?
- [ ] Hayır — tüm korunan kaynaklar merkezi ve canlı ortama hazır (production-ready) bir kimlik doğrulama kontrolü ile korunmaktadır  
- [ ] Evet — eski uç noktalar (legacy endpoints), hata ayıklama yolları (örneğin `/debug/login`) veya unutulmuş API sürümlerine kimlik bilgileri olmadan **erişilebiliyor**  

### Uygulama, yüksek ayrıcalıklı işlemler için yeniden kimlik doğrulama yapmada veya oturumları doğrulamada başarısız oluyor mu?
- [ ] Hayır — yüksek ayrıcalıklı veya hassas işlemler yeni veya geçerli bir oturum belirteci (Session Token) gerektirir  
- [ ] Evet — bir uç noktada (endpoint) atlatma bulunduğunda, bu durum uygulama genelinde yönetici işlemlerini gerçekleştirmek için **kullanılabilir**  

---