## WSTG-SESS-03 — Oturum Sabitleme (Session Fixation)

Oturum Sabitleme (Session Fixation), bir uygulamanın kullanıcı başarıyla kimlik doğruladıktan sonra oturum tanımlayıcısını (session identifier) geçersiz kılmaması veya yenilememesi (rotate) durumunda meydana gelir ve bir saldırganın bilinen bir oturum belirtecini (session token) bir mağdura zorla kullandırmasına olanak tanır. Uygulama, giriş işleminden sonra kimlik doğrulama öncesi oturum kimliğini (session ID) koruyorsa, saldırgan mağdura sabit bir oturum kimliği içeren özel olarak hazırlanmış bir bağlantı gönderebilir ve ardından kimliği doğrulanmış oturumu ele geçirebilir (hijack). Bu güvenlik açığı genellikle giriş formlarında veya URL parametreleri ve çerezler (cookies) aracılığıyla kabul edilen oturum tanımlayıcılarında kendini gösterir. Bir saldırganın perspektifinden bakıldığında, istismar süreci kötü amaçlı bir bağlantı veya bir başlık enjeksiyonu (header injection) güvenlik açığı aracılığıyla oturumu "sabitlemeyi" ve mağdurun kimlik bilgilerini girmesini beklemeyi içerir; bu da canlı bir belirteci çalma ihtiyacını etkili bir şekilde ortadan kaldırır.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-SESS-03 |
| **CWE** | CWE-384 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/03-Testing_for_Session_Fixation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite (Proxy/Repeater)`, `OWASP ZAP`, `EditThisCookie`, `Curl`

### Başarılı kimlik doğrulamasından sonra oturum tanımlayıcısı değişiyor mu?
- [ ] Evet — yeni bir oturum tanımlayıcısı **atanır** ve eskisi geçersiz kılınır *(En güvenli)*  
- [ ] Evet — yeni bir oturum tanımlayıcısı **atanır** ancak eskisi **geçerli kalmaya devam eder**  
- [ ] Hayır — oturum tanımlayıcısı kimlik doğrulama öncesinde ve sonrasında **aynı kalır** *(Kritik)*  

### Uygulama, URL parametreleri aracılığıyla sağlanan oturum tanımlayıcılarını kabul ediyor mu?
- [ ] Hayır — oturum kimlikleri (session IDs) **yalnızca** çerezler aracılığıyla yönetilir  
- [ ] Evet — oturum kimlikleri URL'de kabul edilir ancak giriş yapıldığında **yok sayılır** veya **yenilenir**  
- [ ] Evet — URL'deki oturum kimlikleri **kabul edilir** ve kimlik doğrulamasından sonra **korunur**  

### Saldırgan tarafından tanımlanan bir oturum kimliği uygulamaya dayatılabilir mi?
- [ ] Hayır — uygulama, kullanıcı tarafından sağlanan geçersiz veya mevcut olmayan oturum kimliklerini **reddeder**  
- [ ] Evet — uygulama, bir çerezde sağlanan herhangi bir rastgele kimliği kullanarak bir oturumu **kabul eder** ve başlatır  
- [ ] Evet — uygulama, bir URL parametresinde sağlanan herhangi bir rastgele kimliği kullanarak bir oturumu **kabul eder** ve başlatır  

### Kimlik doğrulama öncesi oturumlar doğru şekilde sonlandırılıyor mu?
- [ ] Evet — anonim oturum, giriş işlemi gerçekleştiğinde **tamamen yok edilir**  
- [ ] Hayır — anonim oturum **yok edilmez**, bu da potansiyel oturum toplama (session harvesting) işlemlerine izin verir  

### Oturum çerezi istemci tarafı enjeksiyonuna karşı korunuyor mu?
- [ ] Evet — betik tabanlı sabitlemeyi önlemek için `HttpOnly` ve `Secure` bayrakları **uygulanmıştır**  
- [ ] Hayır — bayraklar **uygulanmamıştır**, bu da oturum kimliklerinin Cross-Site Scripting (XSS) üzerinden ayarlanmasına izin verir  

---