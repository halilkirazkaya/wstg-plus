## WSTG-INPV-15 — HTTP Splitting Smuggling

HTTP Splitting ve Smuggling zafiyetleri, ön uç (frontend) vekilleri ile arka uç (backend) sunucularının, özellikle `Content-Length` ve `Transfer-Encoding` başlıkları açısından HTTP istek sınırlarını nasıl yorumladığı ve işlediği arasındaki tutarsızlıklardan kaynaklanır. Saldırgan, belirsiz istekler kurgulayarak arka uca gizli bir istek "sızdırabilir" (smuggle) veya bir yanıtı bölmek için CRLF dizileri enjekte edebilir; bu da önbellek zehirlenmesi (cache poisoning), istek ele geçirme (request hijacking) veya güvenlik kontrollerinin atlatılmasına yol açar. Bu kusurlar genellikle tutarsız ayrıştırma (parsing) mantığına sahip ters vekiller (reverse proxy), yük dengeleyiciler (load balancer) veya CDN'lerin kullanıldığı karmaşık ortamlarda ortaya çıkar. Bir saldırgan açısından bu durum, kullanıcı trafiğinin yönlendirilmesine, hassas oturum jetonlarının (session tokens) çalınmasına ve diğer kullanıcıların oturumları bağlamında yetkisiz eylemlerin gerçekleştirilmesine olanak tanır.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INPV-15 |
| **CWE** | CWE-444 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Kritik |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/15-Testing_for_HTTP_Response_Splitting  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  

**Araçlar:** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `smuggler.py`, `curl`

### Ortam, CL.TE veya TE.CL tutarsızlıkları yoluyla istek sızdırmaya (request smuggling) karşı duyarlı mı?
- [ ] Hayır — ön uç ve arka uç sunucuları istek sınırlarını tutarlı bir şekilde işlemektedir  
- [ ] Evet — tutarsızlıklar mevcuttur ancak altyapıdaki iyileştirmeler nedeniyle istismar **mümkün değildir**  
- [ ] Evet — CL.TE veya TE.CL sızdırma (smuggling) **mümkündür**, gizli isteklerin arka uca ulaşmasına izin verir *(Yüksek)*  
- [ ] Evet — ön uç filtrelerini atlatmak için TE.TE (çift kodlama/gizleme) **mümkündür** *(Kritik)*  

### Başlıklardaki CRLF enjeksiyonu (CRLF injection) yoluyla HTTP Yanıt Bölme (HTTP Response Splitting) gerçekleştirilebilir mi?
- [ ] Hayır — uygulama, tüm başlık girişlerindeki CRLF dizilerini uygun şekilde temizlemektedir  
- [ ] Evet — CRLF dizileri başlıklara yansıtılmaktadır ancak yanıt bölme (response splitting) **mümkün değildir**  
- [ ] Evet — CRLF enjeksiyonu **mümkündür**, başlık enjeksiyonuna veya önbellek zehirlenmesine (cache poisoning) izin verir  

### Güvenlik kontrolleri (WAF/ACL'ler), sızdırılan istekler kullanılarak atlatılabiliyor mu?
- [ ] Hayır — güvenlik kontrolleri hem dış isteklere hem de sızdırılan isteklere uygulanmaktadır  
- [ ] Evet — sızdırılan istekler ön uç WAF kurallarını veya IP tabanlı ACL'leri **atlatabilir**  

### Diğer kullanıcıların oturumlarını ele geçirmek veya trafiklerini yönlendirmek mümkün mü?
- [ ] Hayır — istek/yanıt akışları izoledir ve birbirine karıştırılamaz  
- [ ] Evet — istek sızdırma (request smuggling) **mümkündür** ve diğer kullanıcıların isteklerinin ele geçirilmesine izin verir *(Kritik)*  
- [ ] Evet — yanıt bölme (response splitting) **mümkündür** ve tarayıcı tarafında önbellek zehirlenmesine veya XSS'e izin verir  

---