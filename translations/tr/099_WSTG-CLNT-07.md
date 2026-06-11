## WSTG-CLNT-07 — Kaynaklar Arası Kaynak Paylaşımı (CORS)

Kaynaklar Arası Kaynak Paylaşımı (CORS), bir sunucunun farklı kökenlerden (origins) kendi kaynaklarına erişime açıkça izin vermesini sağlayan ve Aynı Köken Politikasını (SOP) esneten tarayıcı tabanlı bir mekanizmadır. Yanlış yapılandırmalar, bir uygulamanın rastgele kökenlere aşırı güvenmesi durumunda ortaya çıkar; bu durum genellikle `Access-Control-Allow-Origin` yanıt başlığında `Origin` başlığının yansıtılması (reflecting) veya kötü yapılandırılmış regex beyaz listelerinin (whitelists) kullanılmasıyla gerçekleşir. Saldırgan perspektifinden bakıldığında, gevşek bir CORS politikası, kontrol edilen bir alan adında (domain) barındırılan ve zafiyetli uygulamaya kimlik doğrulaması yapılmış (authenticated) istekler gönderen kötü niyetli bir betik (script) aracılığıyla istismar edilebilir. Bu durum, saldırganın CSRF tokenları veya Kişisel Veriler (PII) gibi hassas verileri doğrudan yanıt gövdesinden (response body) sızdırmasına (exfiltrate) olanak tanır. Bu zafiyet, kimlik doğrulaması yapılmış oturumları işleyen ve halka açık olmayan bilgileri döndüren API uç noktalarında (endpoints) en kritik seviyededir.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CLNT-07 |
| **CWE** | CWE-942 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek |

**Kaynaklar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/07-Testing_Cross_Origin_Resource_Sharing  
* https://hacktricks.wiki/en/pentesting-web/cors-bypass.html  
* https://portswigger.net/web-security/cors  

**Araçlar:** `Burp Suite (CORS Raider)`, `curl`, `Postman`, `Corsy`

### Uygulama, kimlik doğrulaması yapılmış uç noktalarında CORS başlıklarını uyguluyor mu?
- [ ] Hayır — CORS **etkin değil**; tarayıcı varsayılan olarak katı Aynı Köken Politikasına (SOP) döner  
- [ ] Evet — CORS başlıkları mevcut ve katı bir **beyaz liste** (whitelist) ile sınırlandırılmış  
- [ ] Evet — CORS başlıkları mevcut ancak **gevşek (permissive)** yapılandırılmış  

### Sunucu `Access-Control-Allow-Origin` (ACAO) başlığını nasıl işliyor?
- [ ] ACAO belirli, **güvenilir** ve statik bir alan adına ayarlanmış  
- [ ] ACAO `*` (wildcard) olarak ayarlanmış ve kimlik bilgileri (credentials) **gönderilemez**  
- [ ] ACAO `*` (wildcard) olarak ayarlanmış ve kimlik bilgileri **gönderilebilir** *(Not: Çoğu tarayıcı bunu engeller)*  
- [ ] ACAO, istekteki `Origin` başlığının değerini **dinamik olarak yansıtıyor** (reflect)  

### `Access-Control-Allow-Credentials` (ACAC) başlığı `true` olarak ayarlanmış mı?
- [ ] Hayır — ACAC **mevcut değil** veya `false` olarak ayarlanmış  
- [ ] Evet — ACAC `true` olarak ayarlanmış, ancak ACAO güvenilir kökenlerle **sınırlandırılmış**  
- [ ] Evet — ACAC `true` olarak ayarlanmış ve ACAO rastgele kökenleri **yansıtıyor** *(Kritik)*  

### Köken beyaz listesi, alt alan adı (subdomain) veya null origin manipülasyonu ile atlatılabilir mi?
- [ ] Hayır — beyaz liste katı bir şekilde uygulanıyor ve **atılatılamaz**  
- [ ] Evet — beyaz liste hedefin **herhangi bir** alt alan adını kabul ediyor (örneğin, `attacker.target.com`)  
- [ ] Evet — beyaz liste sandbox uygulanmış iframe'ler aracılığıyla `null` origin değerini kabul ediyor  
- [ ] Evet — beyaz liste regex atlatma (bypass) yöntemlerine karşı savunmasız (örneğin, `target.com.attacker.com` veya `target.com-safe.com`)  

### CORS yapılandırması hassas veri sızıntısına (exfiltration) izin veriyor mu?
- [ ] Hayır — dışa açık uç noktalar hassas veya oturuma özel veriler **içermiyor**  
- [ ] Evet — kimlik doğrulaması yapılmış uç noktalar, yetkisiz bir köken tarafından **okunabilen** PII, kimlik bilgileri veya CSRF tokenları döndürüyor  

---