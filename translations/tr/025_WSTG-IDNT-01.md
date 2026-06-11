## WSTG-IDNT-01 — Rol Tanımlarının Test Edilmesi

Rol tanımlarının test edilmesi, uygulama içinde tanımlanan çeşitli kullanıcı rollerinin ve yetki seviyelerinin belirlenmesini ve belgelenmesini kapsar; bu, rollerin mantıksal olarak ayrılmasını ve en az ayrıcalık (least privilege) ilkesine bağlı kalınmasını sağlamak amacıyla yapılır. Bir saldırgan, yetki sınırları arasındaki belirsizlikleri veya belgelenmemiş yönetici işlevlerini arayarak yüksek ayrıcalıklı rolleri standart kullanıcı rollerine karşı haritalamak (map) için uygulamayı analiz eder. Bu rollerin tanımlanması, yetki yükseltme (privilege escalation) yollarını keşfetmenin ilk adımıdır; çünkü rol tanımlarındaki tutarsızlıklar genellikle hassas verilere veya yönetim özelliklerine yetkisiz erişime yol açar. Bu aşama tipik olarak ilk keşif ve iş mantığı haritalama (business logic mapping) sırasında gerçekleşir; kullanıcı profillerine, yönetici panellerine ve API (Application Programming Interface) yetkilendirme yapılarına odaklanır.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-IDNT-01 |
| **CWE** | CWE-285 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Düşük / Orta |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/01-Test_Role_Definitions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite (Compare Site Maps)`, `Postman`, `AuthMatrix`, `Authz`, `Browser Developer Tools`

### Roller, uygulamada veya uygulama belgelerinde açıkça tanımlanmış ve belgelenmiş mi?
- [ ] Evet — roller tamamen belgelenmiştir ve **en az ayrıcalık** (least privilege) ilkesine uygundur  
- [ ] Evet — roller belgelenmiştir ancak **çakışan izinler** içermektedir  
- [ ] Hayır — resmi bir rol belgelendirmesi veya tanımı mevcut değildir  

### Yönetici işlevleri ile standart kullanıcı işlevleri arasında net bir ayrım var mı?
- [ ] Evet — yönetici işlevleri, standart kullanıcı görünümlerinden **tamamen izole edilmiştir**  
- [ ] Evet — ayrım mevcuttur ancak yönetici uç noktaları (endpoints) **tahmin edilebilir veya saptanabilirdir**  
- [ ] Hayır — yönetici ve standart işlevler aynı arayüzler içinde **karışıktır**  

### Uygulama, Rol Tabanlı Erişim Kontrolü (Role-Based Access Control - RBAC) için merkezi bir mekanizma kullanıyor mu?
- [ ] Evet — merkezi RBAC veya ABAC uygulanmıştır ve tutarlı bir şekilde zorunlu kılınmaktadır  
- [ ] Evet — merkezi olmayan kontroller mevcuttur ancak modüller arasında **tutarsız bir şekilde uygulanmaktadır**  
- [ ] Hayır — erişim kontrol mantığı, bağımsız sayfalara veya bileşenlere **statik olarak kodlanmıştır** (hardcoded)  

### Sistemde belgelenmemiş veya gizli roller mevcut mu?
- [ ] Hayır — keşfedilen tüm roller belgelenmiş işlevlerle eşleşmektedir  
- [ ] Evet — gizli veya "gölge" (shadow) roller (örneğin `super_admin`, `support`, `test`) **mevcuttur**  

### Düşük ayrıcalıklı bir kullanıcı, diğer kullanıcıların izinlerini veya rollerini listeleyebilir (enumerate) mi?
- [ ] Hayır — kullanıcılar diğer varlıkların rollerini/izinlerini görüntüleyemez veya **listeleyemez**  
- [ ] Evet — kullanıcılar profil sayfaları, meta veriler veya API yanıtları aracılığıyla rolleri **listeleyebilir** *(Orta)*  

---