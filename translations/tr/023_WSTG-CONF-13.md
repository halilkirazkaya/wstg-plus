## WSTG-CONF-13 — Yol Karışıklığı (Path Confusion)

Yol Karışıklığı (Path Confusion), ters vekil sunucular (reverse proxies), yük dengeleyiciler (load balancers) ve arka uç uygulama sunucuları (backend application servers) gibi farklı web bileşenlerinin URL yollarını ayrıştırma ve yorumlama biçimlerindeki tutarsızlıklardan kaynaklanır. Saldırganlar, güvenlik filtrelerini aldatmak ve kısıtlanmış uç noktalara (endpoints) veya statik kaynaklara erişmek için noktalı virgül, kodlanmış eğik çizgiler veya nokta segmentleri gibi belirli karakterleri enjekte ederek bu tutarsızlıkları istismar ederler. Ön uç (front-end) sistemin, arka ucun (back-end) farklı yorumladığı bir yola güvenlik kuralları uygulaması nedeniyle; bu güvenlik açığı kimlik doğrulama atlatma (authentication bypass), yetkisiz veri erişimi ve web önbellek zehirlenmesi (web cache poisoning) dahil olmak üzere kritik güvenlik ihlallerine yol açabilir. Başarılı bir istismar genellikle istek yönlendirme (request routing) ve normalleştirme (normalization) mantığının teknoloji yığını genelinde farklılaştığı mimari sınırlarda meydana gelir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CONF-13 |
| **CWE** | CWE-444 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek* |

> *Yol karışıklığı, hassas yönetim uç noktaları için kimlik doğrulama veya erişim kontrolünün atlatılmasıyla sonuçlanırsa önem derecesi "Yüksek" olur.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/13-Test_for_Path_Confusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite`, `Dirsearch`, `FFUF`, `ParamMiner`, `Arjun`

### Farklı mimari katmanlar yol ayırıcılarını (örneğin; `;`, `#`, `?`) tutarlı bir şekilde yorumluyor mu?
- [ ] Evet — tüm katmanlar yol ayırıcılarını aynı şekilde normalleştirir ve yorumlar  
- [ ] Hayır — tutarsızlıklar mevcuttur ancak kısıtlanmış kaynaklara erişmek için **kullanılamaz**  
- [ ] Hayır — farklılıklar, bir saldırganın ön uç filtrelerini atlatmasına ve arka uç mantığına ulaşmasına **izin vermektedir**  

### Erişim kontrolleri, yol aşımı (path traversal) dizileri veya kodlanmış karakterler kullanılarak atlatılabilir mi?
- [ ] Hayır — güvenlik kontrollerinden önce normalleştirme tutarlı bir şekilde uygulanmaktadır  
- [ ] Evet — nokta segmenti dizileri (örneğin; `/admin/..;/`) aracılığıyla atlatma **mümkündür**  
- [ ] Evet — URL kodlamalı karakterler (örneğin; `%2f`, `%2e%2e%2f`) aracılığıyla atlatma **mümkündür**  

### Uygulama, yol karışıklığı yoluyla Web Önbellek Zehirlenmesine (Web Cache Poisoning) karşı savunmasız mı?
- [ ] Hayır — önbelleğe alma mantığı, yol belirsizliğinden veya fazladan yol bilgisinden etkilenmez  
- [ ] Evet — eşleştirme tutarsızlıkları nedeniyle kötü niyetli içerik meşru yollar için önbelleğe **alınabilir**  
- [ ] Evet — hassas bilgiler, `RCD` (Göreceli Yol Üzerine Yazma - Relative Path Overwrite) teknikleri aracılığıyla halka açık dizinlerde önbelleğe **alınabilir**  

### Sunucu "ek yol bilgilerini" (PathInfo) güvenli bir şekilde işliyor mu?
- [ ] Hayır — özellik **devre dışı bırakılmıştır** veya mevcut değildir  
- [ ] Evet — `PathInfo` işlenmektedir ve güvenlik filtrelerine müdahale **etmemektedir**  
- [ ] Hayır — `PathInfo`, bir saldırganın uygulamanın yönlendirme mantığını karıştıran veriler eklemesine **izin vermektedir**  

---