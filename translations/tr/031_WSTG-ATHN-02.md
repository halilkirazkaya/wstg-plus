## WSTG-ATHN-02 — Varsayılan Kimlik Bilgilerinin Test Edilmesi

Varsayılan kimlik bilgilerinin test edilmesi; fabrika ayarlarında bırakılmış veya tedarikçi tarafından sağlanan kullanıcı adlarını ve parolalarını hala kullanan yönetici arayüzlerinin, servislerin veya uygulama bileşenlerinin tespit edilmesini içerir. Bu zafiyet, minimum çabayla tam sistem ele geçirme, yönetici erişimi veya uzaktan kod yürütme (Remote Code Execution) yolları sunduğu için saldırganlar için yüksek öncelikli bir hedeftir. Genellikle hazır yazılımlarda, CMS platformlarında, veritabanı yönetim araçlarında ve IoT cihazları veya ağ donanımları gibi entegre donanımlarda görülür. İstismar (Exploitation) işlemi genellikle, tespit edilen yazılım sürümlerinin halka açık varsayılan parola veritabanlarıyla karşılaştırılmasıyla veya ilk keşif ve istismar aşamalarında otomatik kimlik bilgisi doldurma (Credential Stuffing) araçlarının kullanılmasıyla gerçekleştirilir.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-ATHN-02 |
| **CWE** | CWE-1392 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Kritik / Yüksek |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/02-Testing_for_Default_Credentials  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Araçlar:** `Hydra`, `Medusa`, `Burp Suite (Intruder)`, `Nmap`, `Metasploit`, `DefaultPassword.com`

### Yönetici veya yönetim arayüzleri internete veya güvenilmeyen ağ segmentlerine açık mı?
- [ ] Hayır — hiçbir yönetim arayüzüne erişilemez  
- [ ] Evet — arayüzler mevcut ancak ağ düzeyindeki kontrollerle (örneğin VPN, IP İzin Listesi) sınırlandırılmış  
- [ ] Evet — arayüzler halka **açıktır** ve kimlik doğrulaması gerektirir  

### Tanımlanan tüm bileşenler için tedarikçi tarafından sağlanan varsayılan kimlik bilgileri değiştirildi mi?
- [ ] Evet — tüm bileşenlerdeki tüm varsayılan kimlik bilgileri **değiştirilmiştir** *(En güvenli)*  
- [ ] Evet — ana uygulama için kimlik bilgileri **değiştirilmiştir**, ancak ikincil bileşenler (örneğin CMS, DB) varsayılan olarak kalmıştır  
- [ ] Hayır — bir veya daha fazla tanımlanabilir arayüzde varsayılan kimlik bilgileri **aktiftir** *(Kritik)*  

### Uygulama, yeni veya yönetici hesapları için ilk girişte parola değişimini zorunlu kılıyor mu?
- [ ] Evet — herhangi bir yönetimsel işlem yapılmadan önce parola değişikliği **zorunludur**  
- [ ] Hayır — uygulama, varsayılan kimlik bilgilerinin süresiz olarak kullanılmasına **izin verir**  

### Otomatik varsayılan kimlik bilgisi testlerini önlemek için kilitleme mekanizmaları veya hız sınırlama (Rate Limiting) kontrolleri aktif mi?
- [ ] Evet — agresif hız sınırlama veya hesap kilitleme **uygulanmaktadır**  
- [ ] Evet — kontroller mevcuttur ancak başlık manipülasyonu veya IP rotasyonu ile atlatılması (Bypass) **mümkündür**  
- [ ] Hayır — hiçbir hız sınırlama veya kilitleme mekanizması **etkin değildir**  

### Üçüncü taraf eklentiler, ara katman yazılımları veya yönetim araçları (örneğin phpMyAdmin, Tomcat Manager) benzersiz kimlik bilgileri kullanıyor mu?
- [ ] Hayır — ortamda üçüncü taraf bileşen bulunmamaktadır  
- [ ] Evet — tüm üçüncü taraf bileşenler benzersiz, varsayılan olmayan kimlik bilgileri kullanmaktadır  
- [ ] Hayır — en az bir üçüncü taraf bileşene bilinen varsayılan kimlik bilgileriyle **erişilebilmektedir**

---