## WSTG-INPV-08 — SSI Enjeksiyonu Testi (Testing for SSI Injection)

Sunucu Tarafı İçerme (SSI) Enjeksiyonu (Server-Side Includes (SSI) Injection), bir uygulama kullanıcı girdisini sunucunun SSI motoru tarafından işlenen HTML dosyalarına dahil etmeden önce arındırmadığında meydana gelir. Saldırganlar, rastgele kabuk (shell) komutları yürütmek, yerel dosyaları okumak veya ortam değişkenlerini dışarı sızdırmak için `<!--#exec cmd="id" -->` gibi SSI yönergelerini (directives) enjekte ederek bu durumu istismar ederler. Bu zafiyet genellikle `.shtml`, `.shtm` veya `.stm` gibi eski dosya uzantılarını kullanan uygulamalarda veya web sunucusunun standart HTML dosyaları içinde SSI çözümlemesi yapacak şekilde açıkça yapılandırıldığı durumlarda bulunur. Başarılı bir istismar (exploit), saldırgana web sunucusu süreciyle aynı yetkileri vererek genellikle tam sistem ele geçirme veya hassas veri maruziyeti (sensitive data exposure) ile sonuçlanır.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INPV-08 |
| **CWE** | CWE-97 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Kritiklik** | Yüksek |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/08-Testing_for_SSI_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite (Intruder/Repeater)`, `ffuf`, `Wfuzz`, `curl`

### Web sunucusu SSI yönergelerini destekliyor ve işliyor mu?
- [ ] Hayır — sunucu SSI desteklemiyor veya `.shtml` gibi dosya uzantıları devre dışı bırakılmış  
- [ ] Evet — SSI etkin ancak kullanıcı girdisi işlenen sayfalara yansıtılmıyor  
- [ ] Evet — SSI etkin ve kullanıcı girdisi işlenen sayfalara yansıtılıyor  

### `#exec` yönergesi aracılığıyla rastgele sistem komutları yürütülebiliyor mu?
- [ ] Hayır — `#exec` yönergesi devre dışı bırakılmış veya sunucu yapılandırmasıyla sınırlandırılmış  
- [ ] Evet — `#exec cmd` veya `#exec cgi` yükleri (payloads) aracılığıyla komut yürütme mümkündür  

### Hassas dosyaların veya ortam değişkenlerinin dışarı sızdırılması mümkün mü?
- [ ] Hayır — `#include` veya `#config` gibi yönergeler devre dışı bırakılmış  
- [ ] Evet — `#include virtual` aracılığıyla yerel dosyaları (örn. `/etc/passwd`) okumak mümkündür  
- [ ] Evet — sunucu ortam değişkenleri `#printenv` veya `#echo` aracılığıyla dışarı sızdırılabilir  

### Girdi doğrulama veya WAF kontrolleri SSI yüklerine karşı etkili mi?
- [ ] Evet — sıkı girdi doğrulama uygulanmaktadır ve bypass mümkün değildir  
- [ ] Evet — doğrulama/WAF uygulanmaktadır ancak karakter kodlaması veya farklı SSI sözdizimi kullanılarak bypass mümkündür  
- [ ] Hayır — SSI karakterleri (`<`, `!`, `#`, `-`, `"`) için herhangi bir doğrulama veya WAF koruması mevcut değildir  

---