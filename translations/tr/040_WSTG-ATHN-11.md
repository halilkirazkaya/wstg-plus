## WSTG-ATHN-11 — Çok Faktörlü Kimlik Doğrulamanın (MFA) Test Edilmesi

Çok faktörlü kimlik doğrulama (Multi-Factor Authentication - MFA) testi, birincil kimlik bilgilerinin ele geçirilmesi durumunda bile yetkisiz erişimi önlemek için tasarlanmış ikincil güvenlik katmanının dayanıklılığını değerlendirir. Saldırganlar sıklıkla eski API versiyonları, mobil arka uçlar veya parola sıfırlama iş akışları gibi MFA'nın tutarsız uygulandığı uç noktaları tespit ederek MFA'yı atlatmaya çalışırlar. İstismar süreci genellikle sunucu yanıtlarının manipüle edilmesini, kısa ömürlü kodların (OTP) Brute Force ile kırılmasını veya kullanıcının ikinci faktörü sağlamadan kimlik doğrulanmış bir duruma geçmesine izin veren Race Condition ve oturum yönetimi kusurlarının kötüye kullanılmasını içerir.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-ATHN-11 |
| **CWE** | CWE-287 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Kritik |

**Kaynaklar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/11-Testing_Multi-Factor_Authentication  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite (Intruder/Repeater/Turbo Intruder)`, `Postman`, `Mitproxy`

### MFA tüm kimlik doğrulama portallarında tutarlı bir şekilde zorunlu tutuluyor mu?
- [ ] Evet — Tüm web, mobil ve API tabanlı giriş denemeleri için MFA gereklidir  
- [ ] Evet — ancak belirli uç noktalarda (örneğin eski `/api/v1/login` veya mobile özel portallar) MFA **zorunlu değildir**  
- [ ] Hayır — Uygulamada MFA **uygulanmamıştır** *(Kritik)*  

### MFA doğrulama adımı, doğrudan URL navigasyonu veya yanıt manipülasyonu yoluyla atlatılabilir mi?
- [ ] Hayır — doğrudan navigasyon veya parametre tahrifatı doğrulamayı **atlatamaz**  
- [ ] Evet — MFA doğrulaması tamamlanmadan doğrudan dahili dashboard URL'lerine gitmek **mümkündür**  
- [ ] Evet — Sunucu yanıtını manipüle etmek (örneğin `401 Unauthorized` yanıtını `200 OK` olarak değiştirmek) erişim elde etmek için **mümkündür**  

### MFA kod doğrulama süreci Brute Force saldırılarına karşı korunuyor mu?
- [ ] Evet — Birden fazla başarısız OTP denemesinden sonra katı Rate Limiting veya hesap kilitleme **uygulanmaktadır**  
- [ ] Evet — Rate Limiting mevcuttur ancak IP rotasyonu veya başlık manipülasyonu ile atlatılması **mümkündür**  
- [ ] Hayır — Herhangi bir Rate Limiting uygulanmamaktadır ve kodların otomatik Brute Force ile kırılması **mümkündür**  

### Uygulama, MFA geçişi sırasında güvenli bir oturum durumunu koruyor mu?
- [ ] Evet — Yüksek yetkili oturum belirteçleri (session tokens) **yalnızca** başarılı MFA tamamlandıktan sonra oluşturulur  
- [ ] Hayır — MFA tamamlanmadan **önce** tam işlevsel bir oturum çerezi (session cookie) oluşturularak bazı özelliklere erişime izin veriliyor  
- [ ] Hayır — Uygulama, ele geçirilebilecek statik bir tanımlayıcı veya öngörülebilir bir oturum geçişi kullanıyor  

### Alternatif MFA faktörleri (SMS, E-posta, Yedek Kodlar) istismara karşı savunmasız mı?
- [ ] Hayır — Tüm faktörler güvenli, öngörülemez ve zaman sınırlı değerler kullanıyor  
- [ ] Evet — Yedek kodlar öngörülebilirdir veya **numaralandırılabilir** (enumerate)  
- [ ] Evet — "Kodu Yeniden Gönder" işlevi, SMS/E-posta flooding yapmak veya kısmi iletişim bilgilerini ifşa etmek için kötüye kullanılabilir  

---