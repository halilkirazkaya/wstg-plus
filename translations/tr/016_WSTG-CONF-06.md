## WSTG-CONF-06 — HTTP Metotlarını Test Etme

HTTP metotlarının test edilmesi, yalnızca gerekli işlevselliğin dışarıya açıldığından emin olmak için web sunucusu ve uygulama tarafından hangi fiillerin (verbs) desteklendiğinin belirlenmesini içerir. Standart `GET` ve `POST` metotlarının ötesinde, sunucular yanlışlıkla `PUT`, `DELETE` veya `TRACE` gibi tehlikeli metotları etkinleştirebilir; bu durum saldırganların kötü amaçlı dosyalar yüklemesine, mevcut içeriği silmesine veya Cross-Site Tracing (XST) aracılığıyla oturum çerezlerini sızdırmasına olanak tanıyabilir. Sızma testi uzmanları (Pentesters), `Allow` veya `Public` gibi yanıt başlıklarını inceler ve yetkisiz işlevlere erişmek için metot geçersiz kılma (method overriding) tekniklerini veya fiil kurcalama (verb tampering) yöntemlerini kullanarak kısıtlamaları atlatmaya çalışır. Bu test, saldırı yüzeyini sertleştirmek ve sunucunun ele geçirilmesine veya yetkisiz veri manipülasyonuna yol açabilecek hatalı yapılandırmaları önlemek için kritik öneme sahiptir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CONF-06 |
| **CWE** | CWE-650 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Düşük / Orta* |

> *`PUT` veya `DELETE` yetkisiz dosya manipülasyonuna izin veriyorsa veya `TRACE` oturum belirtecinin (session token) sızdırılmasını kolaylaştırıyorsa Önem Derecesi Yüksek (High) olur.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/06-Test_HTTP_Methods  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `curl`, `nmap`, `Burp Suite (Repeater)`, `ZAP`, `Metasploit`

### `PUT` veya `DELETE` gibi tehlikeli HTTP metotları uygulama genelinde devre dışı bırakılmış mı?
- [ ] Evet — yalnızca güvenli metotlar (GET, POST, HEAD) **etkindir**  
- [ ] Hayır — `PUT` veya `DELETE` **etkindir** ancak geçerli kimlik doğrulaması gerektirir  
- [ ] Hayır — `PUT` veya `DELETE` **etkindir** ve kimlik doğrulaması olmadan erişilebilirdir *(Kritik)*  

### Cross-Site Tracing (XST) saldırısını önlemek için `TRACE` metodu devre dışı bırakılmış mı?
- [ ] Evet — `TRACE` metodu **devre dışıdır** veya 405 Method Not Allowed döndürür  
- [ ] Hayır — `TRACE` metodu **etkindir** ancak hassas başlıkları yansıtmaz  
- [ ] Hayır — `TRACE` metodu **etkindir** ve `Cookie` veya `Authorization` başlıklarını yansıtır *(Orta)*  

### HTTP Metot Geçersiz Kılma (örneğin `X-HTTP-Method-Override`) sunucu tarafından destekleniyor mu?
- [ ] Hayır — sunucu metot geçersiz kılma başlıklarını **işlemez**  
- [ ] Evet — sunucu geçersiz kılma başlıklarını işler ancak erişim kontrolleri **uygulanır**  
- [ ] Evet — sunucu geçersiz kılma başlıklarını işler ve erişim kontrolü atlatma (bypass) **mümkündür**  

### Sunucu, rastgele veya hatalı biçimlendirilmiş HTTP metotlarına güvenli bir şekilde yanıt veriyor mu?
- [ ] Evet — sunucu 405 Method Not Allowed veya 501 Not Implemented döndürür  
- [ ] Hayır — sunucu `JEFF` veya `TEST` gibi rastgele metotlar için 200 OK veya 500 Error döndürür  
- [ ] Hayır — rastgele metotların kullanılması, kimlik doğrulama veya yetkilendirme filtrelerinin atlatılmasına olanak tanır  

### `OPTIONS` metodu, bilgi ifşasını en aza indirecek şekilde kısıtlanmış veya yapılandırılmış mı?
- [ ] Evet — `OPTIONS` devre dışıdır veya minimal bir `Allow` başlığı döndürür  
- [ ] Hayır — `OPTIONS`, DAV veya hata ayıklama (debugging) fiilleri de dahil olmak üzere etkinleştirilmiş çok çeşitli metotları açığa çıkarır  

---