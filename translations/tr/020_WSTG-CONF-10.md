## WSTG-CONF-10 — Alt Alan Adı Ele Geçirme (Subdomain Takeover)

Alt alan adı ele geçirme (Subdomain Takeover), bir DNS kaydının (tipik olarak CNAME, ancak bazen A veya MX kayıtları) üçüncü taraf bir bulut sağlayıcısı veya barındırma hizmetindeki kullanımdan kaldırılmış veya mevcut olmayan bir kaynağa işaret etmesiyle meydana gelir. Saldırganlar, AWS, GitHub veya Azure gibi sağlayıcılardan gelen ve bir kaynağın artık sahiplenilmediğini belirten belirli hata mesajlarını arayarak bu "boşta kalan" (dangling) DNS kayıtlarını tespit ederler. Saldırgan, terk edilmiş kaynak adını sağlayıcının platformunda kendi adına kaydederek alt alan adı üzerinde tam kontrol sahibi olur; bu da kötü amaçlı içerik barındırmasına, ana alan adı kapsamında tanımlanmış oturum çerezlerini sızdırmasına (exfiltrate) ve İçerik Güvenlik Politikası (CSP) veya CORS korumalarını atlatmasına olanak tanır. Bu güvenlik açığı, yüksek etkili oltalama (phishing) ve veri hırsızlığını kolaylaştırmak için kuruluşun meşru alan adı yapısıyla ilişkili güvenden yararlandığı için özellikle tehlikelidir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CONF-10 |
| **CWE** | CWE-1329 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Kritik* |

> *Alt alan adı, ana alan adından oturum çerezlerini çalmak veya SSO/OAuth yönlendirmelerini atlatmak için kullanılabiliyorsa Önem Derecesi Kritik hale gelir.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/10-Test_for_Subdomain_Takeover  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://github.com/EdOverflow/can-i-take-over-xyz  

**Araçlar:** `Subjack`, `SubOver`, `dnscan`, `Amass`, `Nuclei`, `dig`, `nslookup`

### Uygulama, alt alan adları aracılığıyla üçüncü taraf barındırma veya bulut hizmetlerinden yararlanıyor mu?
- [ ] Hayır — tüm alt alan adları kuruluş tarafından kontrol edilen IP alanına işaret ediyor  
- [ ] Evet — alt alan adları üçüncü taraf sağlayıcılara (S3, GitHub Pages, Heroku, vb.) işaret ediyor  

### Mevcut olmayan kaynaklara işaret eden herhangi bir "boşta kalan" (dangling) DNS kaydı var mı?
- [ ] Hayır — tespit edilen tüm DNS kayıtları aktif ve kontrol edilen kaynaklara çözümleniyor  
- [ ] Evet — sağlayıcı tarafında **artık mevcut olmayan** kaynaklar için CNAME veya A kayıtları mevcut  
- [ ] Evet — DNS kayıtları NXDOMAIN veya sağlayıcıya özgü hata sayfalarına çözümleniyor *(örneğin, "NoSuchBucket")*  

### Tespit edilen boşta kalan kaynak başarıyla sahiplenilebilir mi?
- [ ] Hayır — sağlayıcının terk edilmiş adların sahiplenilmesine karşı korumaları var veya hesap doğrulaması gerekiyor  
- [ ] Evet — kaynak adı, herhangi bir kullanıcı tarafından üçüncü taraf platformunda **kaydedilebilir**  

### Ana alan adı, güvenliği ihlal edilebilecek "Domain" kapsamlı çerezler kullanıyor mu?
- [ ] Hayır — çerezler kesinlikle belirli alt alan adları ile sınırlandırılmıştır veya `Host-Only` bayraklarını kullanır  
- [ ] Evet — hassas çerezler (oturum kimlikleri, JWT'ler) üst alan adı kapsamında tanımlanmıştır ve ele geçirilmiş bir alt alan adı aracılığıyla **sızdırılabilir**  

### Alt alan adı, güvenlik başlıklarını veya kimlik doğrulama akışlarını atlatmak için kullanılabilir mi?
- [ ] Hayır — tespit edilen alt alan adlarına yönelik herhangi bir güvenlik bağımlılığı bulunmuyor  
- [ ] Evet — alt alan adı, CSP `script-src` veya `connect-src` direktiflerinde beyaz listeye eklenmiş  
- [ ] Evet — alt alan adı, OAuth veya SSO yapılandırmaları için geçerli bir yönlendirme URI'sidir (redirect URI)  

---