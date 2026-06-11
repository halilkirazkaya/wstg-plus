## WSTG-CLNT-03 — HTML Injection

HTML Injection, bir uygulama kullanıcı tarafından sağlanan girdiyi tarayıcıda render etmeden önce uygun şekilde sanitize etmediğinde (temizlemediğinde) ortaya çıkar ve bir saldırganın web sayfasının Belge Nesne Modeline (DOM) rastgele HTML etiketleri enjekte etmesine olanak tanır. Cross-Site Scripting (XSS) ile benzerlik gösterse de, HTML Injection'ın temel amacı sayfanın görsel sunumunu manipüle etmek veya kötü niyetli formlar ve aldatıcı içerikler enjekte ederek oltalama (phishing) saldırılarını kolaylaştırmaktır. Saldırganlar; arama terimleri, kullanıcı profil alanları veya hata mesajları gibi kullanıcı arayüzünde (UI) yansıyan parametreleri hedef alarak mağdurları hassas kimlik bilgilerini ifşa etmeye veya yetkisiz üçüncü taraf bağlantılarla etkileşime girmeye zorlar. Bu güvenlik açığı, uygulamanın kullanıcı arayüzünün bütünlüğü için önemli bir risk teşkil eder ve daha karmaşık sosyal mühendislik veya oturumla ilgili saldırıların öncüsü olabilir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CLNT-03 |
| **CWE** | CWE-80 |
| **Test Durumu** | Yürütülmedi |
| **Önem Derecesi** | Düşük / Orta |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/03-Testing_for_HTML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite`, `OWASP ZAP`, `DOM Invader`, `Wfuzz`

### Kullanıcı kontrollü girdiler, uygun HTML kodlaması (HTML encoding) yapılmadan DOM'a yansıtılıyor mu?
- [ ] Hayır — yansıyan tüm girdiler, render edilmeden önce kesinlikle HTML kodlamasından geçirilmektedir  
- [ ] Evet — bazı karakterler kodlanmaktadır ancak belirli etiketler için atlatmalar (bypasses) mümkündür  
- [ ] Evet — girdi ham (raw) olarak yansıtılmaktadır ve HTML injection mümkündür  

### Sayfa düzenini değiştirmek için yapısal HTML öğeleri enjekte edilebiliyor mu?
- [ ] Hayır — uygulama `<div>`, `<table>` veya `<iframe>` gibi yapısal etiketleri filtrelemekte veya temizlemektedir  
- [ ] Evet — yapısal öğeler enjekte edilebilmektedir ancak kullanıcı arayüzü (UI) üzerinde önemli bir etkisi yoktur  
- [ ] Evet — enjekte edilen etiketler aracılığıyla önemli ölçüde kullanıcı arayüzü tahrifatı (UI defacement) mümkündür  

### Formlar veya kötü niyetli bağlantılar gibi işlevsel oltalama (phishing) öğeleri enjekte etmek mümkün mü?
- [ ] Hayır — `<form>`, `<input>` ve `<a>` etiketleri etkili bir şekilde engellenmekte veya sanitize edilmektedir  
- [ ] Evet — kullanıcıları harici sitelere yönlendirmek için kötü niyetli bağlantılar enjekte edilebilmektedir  
- [ ] Evet — kimlik bilgilerini toplamak için işlevsel `<form>` öğeleri enjekte edilebilmektedir *(Orta)*  

### Güvenlik başlıkları veya İçerik Güvenliği Politikaları (CSP), enjeksiyonun etkisini azaltıyor mu?
- [ ] Evet — kısıtlayıcı bir CSP yürürlüktedir ve `form-action` veya `frame-src` uygulanmaktadır  
- [ ] No — güvenlik başlıkları mevcuttur ancak CSP devre dışıdır veya unsafe-inline/wildcards (joker karakterler) içermektedir  
- [ ] No — hiçbir CSP veya ilgili güvenlik başlığı etkin değildir  

---