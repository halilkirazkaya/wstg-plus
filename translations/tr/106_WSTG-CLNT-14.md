## WSTG-CLNT-14 — Testing for Reverse Tabnabbing

Reverse Tabnabbing, `target="_blank"` kullanılarak açılan bir sayfanın, `window.opener` nesnesi aracılığıyla üst sayfaya (parent page) ait işlevsel bir referansı koruduğu istemci taraflı bir zafiyettir. Bu durum, saldırganın kontrolündeki bir hedef sitenin, orijinal ve güvenilir sekmeyi programatik olarak genellikle kimlik bilgilerini ele geçirmek (credential harvesting) için tasarlanmış bir Kimlik Avı (Phishing) sayfası olan zararlı bir URL'ye yönlendirmesine olanak tanır. Saldırı, kullanıcının orijinal sekmeye duyduğu güveni istismar eder; zira kullanıcılar, az önce ayrıldıkları sayfanın farklı bir alan adına (domain) yönlendiğini genellikle fark etmezler. Modern güvenlik uygulamaları, bu pencereler arası iletişimi önlemek için çapa (anchor) etiketlerinde `rel="noopener"` veya `rel="noreferrer"` özniteliklerinin kullanılmasını ya da JavaScript'te `opener` özelliğinin null olarak ayarlanmasını gerektirir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CLNT-14 |
| **CWE** | CWE-1022 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Düşük / Orta* |

> *Modern tarayıcıların çoğu artık `target="_blank"` kullanımını varsayılan olarak `rel="noopener"` şeklinde işlediğinden, önem derecesi çoğu durumda Düşüktür. Uygulama eski nesil tarayıcıları hedefliyorsa veya `opener` özelliğini null olarak ayarlamadan `window.open()` kullanıyorsa önem derecesi Orta olur.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/14-Testing_for_Reverse_Tabnabbing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Browser DevTools (Inspector)`, `Burp Suite (Repeater)`, `ZAP`

### Yeni bir pencerede veya sekmede açılan bağlantılar mevcut mu?
- [ ] Hayır — `target="_blank"` veya eşdeğer JavaScript tabanlı yönlendirme kullanan bağlantı bulunmamaktadır.
- [ ] Evet — `target="_blank"` yalnızca dahili ve güvenilir bağlantılarda kullanılmaktadır.
- [ ] Evet — `target="_blank"` harici bağlantılarda veya kullanıcı tarafından sağlanan URL'lerde kullanılmaktadır.

### Çapa (anchor) etiketlerinde `window.opener` ilişkisi kısıtlanmış mı?
- [ ] Evet — `rel="noopener"` veya `rel="noreferrer"`, `target="_blank"` içeren tüm bağlantılara **tutarlı bir şekilde uygulanmıştır** *(En güvenli)*.
- [ ] Evet — öznitelikler uygulanmıştır ancak yüksek riskli veya kullanıcı tarafından oluşturulan bağlantılarda **eksiktir**.
- [ ] No — öznitelikler **uygulanmamıştır**, bu durum alt sayfanın `window.opener` nesnesine erişmesine izin vermektedir.

### Uygulama, JavaScript `window.open()` üzerinden Reverse Tabnabbing'e karşı savunmasız mı?
- [ ] Hayır — `window.open()` kullanılmamaktadır veya `opener` özelliği açıkça null olarak ayarlanmıştır.
- [ ] Evet — `window.open()` kullanılmaktadır ve `opener` referansı alt pencere için **erişilebilir durumdadır**.

### Bir saldırgan, yeni sekmede açılan bir bağlantının hedefini kontrol edebilir mi?
- [ ] Hayır — tüm bağlantı hedefleri sabit kodlanmıştır (hardcoded) ve güvenilir alan adlarını işaret etmektedir.
- [ ] Evet — bağlantı hedefleri kullanıcı tarafından sağlanmaktadır (örneğin sosyal medya profilleri, biyografi bağlantıları) ve tabnabbing korumaları için **sanitize edilmemiştir**.

---