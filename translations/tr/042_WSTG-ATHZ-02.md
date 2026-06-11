## WSTG-ATHZ-02 — Yetkilendirme Şeması Atlatma Testi

Yetkilendirme şemalarının atlatılması, bir uygulamanın erişim kontrollerini zorunlu kılmada başarısız olması ve bir saldırganın amaçlanan izinlerinin dışındaki kaynaklara veya işlevlere erişmesine izin vermesi durumunda gerçekleşir. Bu zafiyet genellikle, bir saldırganın aynı seviyedeki başka bir kullanıcıya ait verilere eriştiği Yatay Yetki Yükseltme (Horizontal Privilege Escalation) veya düşük yetkili bir kullanıcının yönetici yetenekleri kazandığı Dikey Yetki Yükseltme (Vertical Privilege Escalation) olarak kendini gösterir. Saldırganlar, kaynak kimlikleri (Resource IDs) gibi istek parametrelerini manipüle ederek, oturum belirteçlerini (session tokens) değiştirerek veya yol tabanlı kısıtlamaları aşmak için `X-Original-URL` gibi HTTP başlıklarını (headers) kullanarak bu kusurlardan faydalanırlar. Güçlü bir yetkilendirme mekanizmasının sağlanması, veri gizliliğinin korunması ve tüm sistemi tehlikeye atabilecek yetkisiz yönetici eylemlerinin önlenmesi açısından kritiktir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-ATHZ-02 |
| **CWE** | CWE-285 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Şiddet** | Yüksek / Kritik |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/02-Testing_for_Bypassing_Authorization_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**Araçlar:** `Burp Suite (Autorize extension)`, `Burp Suite (Repeater/Intruder)`, `Postman`, `cURL`, `OWASP ZAP`

### Aynı roldeki kullanıcılar arasında yatay yetki yükseltme engelleniyor mu?
- [ ] Evet — yetkilendirme kontrolleri her kaynak isteğine **uygulanmaktadır** ve atlatma **mümkün değildir**  
- [ ] Evet — yetkilendirme kontrolleri **uygulanmaktadır** ancak IDOR veya parametre kurcalama (parameter tampering) yoluyla atlatılabilir  
- [ ] Hayır — kullanıcılar sadece bir kaynak kimliğini değiştirerek diğer kullanıcılara ait verilere **erişebilirler** *(Yüksek)*  

### Düşük yetkili kullanıcılar için dikey yetki yükseltme engelleniyor mu?
- [ ] Hayır — uygulama yalnızca tek bir yetki seviyesine sahiptir  
- [ ] Evet — yönetici işlevlerine düşük yetkili kullanıcılar tarafından **erişilemez**  
- [ ] Evet — yönetici işlevleri kullanıcı arayüzünde (UI) gizlidir ancak doğrudan URL üzerinden **erişilebilir**  
- [ ] Hayır — düşük yetkili kullanıcılar, rolleri veya izinleri manipüle ederek yönetici eylemlerini **gerçekleştirebilirler** *(Kritik)*  

### HTTP fiili (verb) kurcalama kullanılarak yetkilendirme atlatılabilir mi?
- [ ] Hayır — kullanılan HTTP yönteminden bağımsız olarak erişim kontrolleri zorunlu kılınmaktadır  
- [ ] Evet — yöntemi değiştirmek (örneğin `GET`'ten `POST` veya `PUT`'a) yetkilendirme kontrollerini **atlatır**  

### Yönetici veya kısıtlanmış uç noktalarına herhangi bir kimlik doğrulama olmadan erişilebiliyor mu?
- [ ] Hayır — tüm hassas uç noktaları (endpoints) geçerli bir oturum ve uygun izinler gerektirir  
- [ ] Evet — saldırgan doğrudan URL'yi biliyorsa bazı yönetici uç noktalarına erişilebilir  
- [ ] Evet — hassas işlevlere kimlik doğrulaması yapılmadan erişim **mümkündür** *(Kritik)*  

### HTTP başlıkları yol tabanlı yetkilendirmenin atlatılmasına izin veriyor mu?
- [ ] Hayır — uygulama başlık tabanlı (header-based) atlatmalara karşı duyarlı **değildir**  
- [ ] Evet — `X-Original-URL`, `X-Rewrite-URL` veya `X-Forwarded-For` gibi başlıklar erişim kontrollerini atlatmak için **kullanılabilir**  

---