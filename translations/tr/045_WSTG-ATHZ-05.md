## WSTG-ATHZ-05 — OAuth Zafiyetleri Testi

OAuth zafiyetleri, OAuth 2.0 veya OpenID Connect (OIDC) protokollerinin uygulanmasındaki çeşitli hataları kapsar ve genellikle tam hesap ele geçirme (account takeover) veya yetkisiz veri erişimi ile sonuçlanır. Bu güvenlik açıkları genellikle yetkilendirme akışı (authorization flow) sırasında, özellikle `redirect_uri` doğrulaması, `state` parametresinin entropisi ve doğrulanması ile erişim (access) veya kimlik (ID) token'larının güvenli işlenmesi konularında ortaya çıkar. Saldırganlar, yetkilendirme kodlarını ele geçirerek, açık yönlendirmeler (open redirect) aracılığıyla token'ları saldırgan kontrolündeki alan adlarına yönlendirerek veya bir mağdurun oturumunu saldırganın hesabına bağlamak için Siteler Arası İstek Sahteciliği (Cross-Site Request Forgery - CSRF) gerçekleştirerek bu açıklardan yararlanırlar. OAuth genellikle birincil kimlik doğrulama mekanizması olarak hizmet ettiğinden, tek bir hatalı yapılandırma tüm kullanıcı kimlik sisteminin bütünlüğünü tehlikeye atabilir.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-ATHZ-05 |
| **CWE** | CWE-287 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Kritik |

**Kaynaklar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/05-Testing_for_OAuth_Weaknesses  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/oauth  

**Araçlar:** `Burp Suite (Repeater/Collaborator)`, `CyberChef`, `OIDC-Checker`, `OAuth-Scanner`

### CSRF'yi önlemek için `state` parametresi uygulanmış ve doğrulanmış mı?
- [ ] Evet — `state` zorunludur, oturum başına benzersizdir ve sunucuda sıkı bir şekilde doğrulanır  
- [ ] Evet — `state` mevcuttur ancak statiktir veya farklı oturumlar arasında tahmin edilebilirdir  
- [ ] Evet — `state` mevcuttur ancak uygulama geri çağırma (callback) sırasında bunu **doğrulamaz**  
- [ ] Hayır — yetkilendirme isteğinde `state` parametresi **kullanılmamaktadır** *(Kritik)*  

### `redirect_uri`, bir beyaz liste (whitelist) üzerinden sıkı bir şekilde doğrulanıyor mu?
- [ ] Evet — yalnızca önceden kaydedilmiş bir beyaz listeye karşı tam eşleşmeler **kabul edilir**  
- [ ] Evet — doğrulama uygulanmaktadır ancak path traversal veya alt alan adı (subdomain) manipülasyonu ile atlatılması **mümkündür**  
- [ ] Evet — doğrulama uygulanmaktadır ancak parametre kirliliği (parameter pollution) veya fragment enjeksiyonu ile atlatılması **mümkündür**  
- [ ] Hayır — uygulama, `redirect_uri` parametresinde rastgele URL'lere **izin verir** *(Kritik)*  

### Akışı değiştirmek için `response_type` manipüle edilebilir mi?
- [ ] Hayır — uygulama yalnızca hedeflenen akışı (örneğin `code`) kabul eder ve diğerlerini reddeder  
- [ ] Evet — `code` yerine `token` (Implicit flow) kullanımına geçiş **mümkündür** ve token'ların URL üzerinden sızmasına neden olur  
- [ ] Evet — hibrit akışlar veya yetkisiz `response_type` değerleri **etkindir** ve işlenmektedir  

### Erişim token'ları veya yetkilendirme kodları, Referer başlıkları veya tarayıcı geçmişi aracılığıyla sızıyor mu?
- [ ] Hayır — token'lar/kodlar güvenli bir şekilde işlenir ve hassas sayfalarda uygun `Referrer-Policy` kullanılır  
- [ ] Evet — token'lar/kodlar `Referer` başlığı aracılığıyla üçüncü taraf alan adlarına **sızmaktadır**  
- [ ] Evet — güvensiz önbelleğe alma veya GET istekleri nedeniyle token'lar/kodlar tarayıcı geçmişinde **görünür** durumdadır  

### Uygulama, OAuth kapsamları (scopes) için "En Az Ayrıcalık" (Least Privilege) ilkesini uyguluyor mu?
- [ ] Evet — uygulama yalnızca işlevsellik için gerekli olan minimum kapsamları talep eder  
- [ ] Hayır — uygulama varsayılan olarak aşırı veya yönetici düzeyinde kapsamlar talep eder  
- [ ] Hayır — kullanıcı, onay (consent) aşamasında belirli kapsamları inceleyemez veya bunlardan **vazgeçemez**  

---