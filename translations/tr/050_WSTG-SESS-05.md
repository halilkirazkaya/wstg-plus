## WSTG-SESS-05 — Testing for Cross Site Request Forgery

Cross-Site Request Forgery (Siteler Arası İstek Sahteciliği - CSRF), bir saldırganın mağdurun tarayıcısını, mağdurun halihazırda kimlik doğrulaması yaptığı farklı bir web sitesinde istenmeyen bir eylem gerçekleştirmeye zorladığı bir zafiyettir. Bu istismar (exploit), tarayıcının oturum çerezleri (session cookies) veya Authorization (Yetkilendirme) üstbilgileri gibi "ortam" kimlik bilgilerini (ambient credentials) giden isteklere otomatik olarak ekleme davranışından yararlanır. Saldırganlar genellikle üçüncü taraf bir sitede kötü amaçlı betikler veya gizli formlar barındırarak şifre değiştirme, e-posta adresi güncelleme veya finansal transferleri yürütme gibi durum değiştiren (state-changing) işlemleri hedef alır. Başarılı bir istismar, kullanıcının bilgisi veya rızası olmadan tam hesap ele geçirme (account takeover) veya yetkisiz veri değişikliği ile sonuçlanabilir.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-SESS-05 |
| **CWE** | CWE-352 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek* |

> *Zafiyetin savunmasız eylemi; hesap ele geçirme, yetki yükseltme (privilege escalation) veya yetkisiz finansal işlemlere izin vermesi durumunda önem derecesi Yüksek olur.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/05-Testing_for_Cross_Site_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/csrf-cross-site-request-forgery.html  
* https://portswigger.net/web-security/csrf  

**Araçlar:** `Burp Suite Professional (CSRF PoC Generator)`, `OWASP ZAP`, `CSRFTester`, `python3 (SimpleHTTPServer for PoC hosting)`

### Durum değiştiren istekler için anti-CSRF token'ları uygulanmış mı?
- [ ] Evet — tüm durum değiştiren eylemler için benzersiz, kriptografik olarak güçlü token'lar zorunludur  
- [ ] Evet — token'lar mevcut ancak oturum başına benzersiz değil veya tahmin edilebilir  
- [ ] Hayır — anti-CSRF token'ları uygulanmamış  

### Anti-CSRF token'ının sunucu tarafı doğrulaması sağlam mı?
- [ ] Evet — sunucu token'ı sıkı bir şekilde doğrular ve atlatma (bypass) mümkün değildir  
- [ ] Evet — doğrulama gerçekleştiriliyor ancak token parametresi kaldırılarak atlatılabiliyor  
- [ ] Evet — doğrulama gerçekleştiriliyor ancak boş veya sahte (dummy) bir token sağlanarak atlatılabiliyor  
- [ ] Evet — doğrulama gerçekleştiriliyor ancak token kullanıcının oturumuna bağlı değil  

### Uygulama, CSRF koruması için kolayca atlatılabilir yöntemlere mi güveniyor?
- [ ] Hayır — uygulama yalnızca zayıf üstbilgilere (headers) veya kaynak (origin) kontrollerine güvenmiyor  
- [ ] Evet — koruma yalnızca sahtesi oluşturulabilen veya silinebilen `Referer` veya `Origin` üstbilgisine dayanıyor  
- [ ] Evet — koruma `X-Requested-With` üstbilgisinin kontrolüne dayanıyor ve bu, CORS yapılandırma hataları aracılığıyla atlatılabiliyor  

### Oturum çerezleri `SameSite` özniteliği ile yapılandırılmış mı?
- [ ] Evet — tüm oturumla ilgili çerezlerde `SameSite` değeri `Strict` veya `Lax` olarak ayarlanmış  
- [ ] Hayır — `SameSite` özniteliği eksik, tarayıcıya özgü varsayılan davranışa bırakılmış  
- [ ] Hayır — `SameSite` değeri, `Secure` bayrağı veya ek azaltıcı önlemler olmaksızın açıkça `None` olarak ayarlanmış  

### CSRF koruması, HTTP istek yöntemi değiştirilerek atlatılabilir mi?
- [ ] Hayır — koruma, kullanılan HTTP yönteminden bağımsız olarak zorunlu kılınmıştır  
- [ ] Evet — token'lar yalnızca `POST` isteklerinde doğrulanıyor, ancak eylem `GET` üzerinden gerçekleştirilebiliyor  
- [ ] Evet — yöntemi değiştirmek (örneğin `PUT` veya `DELETE` olarak) token kontrolünü atlatıyor  

---