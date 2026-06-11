## WSTG-ATHZ-03 — Yetki Yükseltme Testleri (Testing for Privilege Escalation)

Yetki yükseltme (Privilege Escalation), bir saldırganın daha yüksek yetki seviyelerine veya farklı kimliklere sahip kullanıcılara ayrılmış kaynaklara veya işlevlere erişim sağlamak için güvenlik açıklarını istismar etmesiyle gerçekleşir. Dikey yetki yükseltmede (Vertical Escalation), standart bir kullanıcı yönetici işlevlerine erişmeye çalışırken; yatay yetki yükseltmede (Horizontal Escalation) ise aynı yetki seviyesindeki başka bir kullanıcıya ait verilere erişim söz konusudur. Bu kusurlar genellikle kötü yapılandırılmış erişim kontrol listelerinde (ACL), güvensiz doğrudan nesne referanslarında (IDOR) veya oturum veya kimlik belirteçlerindeki parametre manipülasyonlarında (Parameter Tampering) ortaya çıkar. Bir saldırgan açısından bu durum genellikle sayısal ID'lerin manipüle edilmesi, çerezlerdeki (Cookies) veya JWT'lerdeki rol tabanlı parametrelerin değiştirilmesi veya sunucu tarafında yetkilendirme kontrolü bulunmayan gizli API uç noktalarının (API Endpoints) doğrudan çağrılmasıyla elde edilir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-ATHZ-03 |
| **CWE** | CWE-269 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Kritik |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/03-Testing_for_Privilege_Escalation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**Araçlar:** `Burp Suite`, `Authorize`, `AuthMatrix`, `Authz`, `Postman`, `Ffuf`

### Uygulama birden fazla yetki seviyesi veya çoklu kiracılık (multi-tenancy) yapısını uyguluyor mu?
- [ ] Hayır — uygulama tek kullanıcılı veya tek rollü bir sistemdir  
- [ ] Evet — birden fazla rol veya kiracı tanımlanmıştır ve yetkilendirme testi gerektirir  

### Yatay yetki yükseltme (aynı seviye erişim) mümkün mü?
- [ ] Hayır — tüm kaynak tanımlayıcılarına ve oturum sahiplerine yetkilendirme kontrolleri uygulanmaktadır  
- [ ] Evet — IDOR veya parametre manipülasyonu yoluyla diğer kullanıcıların verilerine erişim mümkündür  
- [ ] Evet — veri sızıntısı mümkündür ancak diğer kullanıcıların verilerinin değiştirilmesi mümkün değildir  

### Dikey yetki yükseltme (düşükten yükseğe) mümkün mü?
- [ ] Hayır — yönetici uç noktaları rol tabanlı erişim kontrollerini (RBAC) sıkı bir şekilde uygulamaktadır  
- [ ] Evet — yönetici işlevlerine düşük yetkili kullanıcılar tarafından doğrudan URL erişimi ile erişilebilir  
- [ ] Evet — yönetici işlevlerine rolle ilgili başlıklar (headers), çerezler veya JWT talepleri (claims) manipüle edilerek erişilebilir  

### Kullanıcı rolleri veya izinleri Mass Assignment veya Parameter Pollution yoluyla değiştirilebilir mi?
- [ ] Hayır — rol tabanlı alanlar kesinlikle salt okunurdur ve kullanıcılar tarafından değiştirilemez  
- [ ] Evet — profil güncellemelerinde veya kayıt sırasında gizli parametrelerin (örn. `role=admin`) eklenmesiyle kullanıcı izinleri yükseltilebilir  

### Yetkilendirme kontrolleri tüm API versiyonlarında ve HTTP metodlarında tutarlı bir şekilde uygulanıyor mu?
- [ ] Evet — kontroller tüm versiyonlarda ve metodlarda tutarlı bir şekilde uygulanmaktadır  
- [ ] Hayır — eski API versiyonları (örn. `/v1/`) veya belirli HTTP metodları (örn. `PUT`, `DELETE`, `PATCH`) yetkilendirmeyi atlatmaktadır (bypass)  
- [ ] Hayır — yönetici işlevleri kullanıcı arayüzünde (UI) gizlenmiştir ancak API seviyesinde tüm kullanıcılar için etkindir  

---