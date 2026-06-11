## WSTG-SESS-01 — Oturum Yönetimi Şeması Testi

Oturum yönetimi şeması testi, uygulamanın oturum tanımlayıcılarının (session identifiers) yaşam döngüsünü ve yapısını, tahmin edilmeye ve ele geçirilmeye (hijacking) karşı dirençli olduklarından emin olmak amacıyla nasıl işlediğini analiz etmeye odaklanır. Saldırganlar; örüntüler, düşük entropi veya diğer kullanıcılar için geçerli oturumlar oluşturmalarına (forge) olanak tanıyan öngörülebilir artışlar aramak için birden fazla oturum belirteci (token) yakalayarak istatistiksel analiz gerçekleştirirler. Şemadaki zayıflıklar genellikle oturum sabitleme (Session Fixation) zafiyetleri veya genellikle HTTP çerezlerinde (cookies), URL parametrelerinde veya özel üstbilgi (header) uygulamalarında bulunan yetersiz rastgele değerlerin kullanımı şeklinde ortaya çıkar. Oturum şemasının tehlikeye atılması, bir saldırganın kurbanın kimlik doğrulaması yapılmış durumuna (authenticated state), kurbanın birincil kimlik bilgilerine ihtiyaç duymadan tam erişim sağlamasına olanak tanıyan yüksek etkili bir saldırıdır.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-SESS-01 |
| **CWE** | CWE-330 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/01-Testing_for_Session_Management_Schema  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Araçlar:** `Burp Suite (Sequencer)`, `OWASP ZAP`, `Python (pandas/numpy for entropy analysis)`, `Cookie Editor`

### Oturum tanımlayıcı yeterince karmaşık ve rastgele mi?
- [ ] Evet — belirteç (token) istatistiksel rastgelelik testlerini geçer (yüksek entropi) ve atlatılması (bypass) **mümkün değildir**  
- [ ] Evet — belirteç uzun ancak öngörülebilir örüntüler veya statik bölümler içeriyor  
- [ ] Hayır — belirteç sıralıdır, öngörülebilir zaman damgalarına dayanır veya **kritik düzeyde düşük** entropiye sahiptir *(Kritik)*  

### Oturum tanımlayıcı nerede iletilir ve saklanır?
- [ ] Tanımlayıcı, `Secure` ve `HttpOnly` bayraklarına sahip bir çerezde saklanır *(En güvenli)*  
- [ ] Tanımlayıcı bir çerezde saklanır ancak `Secure` veya `HttpOnly` bayraklarından yoksundur  
- [ ] Tanımlayıcı **yalnızca** URL parametreleri veya `GET` istekleri aracılığıyla iletilir *(Yüksek Risk)*  

### Uygulama, başarılı kimlik doğrulamasından sonra yeni bir oturum tanımlayıcı oluşturuyor mu?
- [ ] Evet — girişten hemen sonra yeni ve benzersiz bir oturum kimliği (Session ID) **oluşturulur**  
- [ ] Hayır — oturum kimliği girişten önce ve sonra aynı kalır *(Oturum Sabitleme/Session Fixation mümkündür)*  

### Yeniden kullanımı önlemek için oturum tanımlayıcı belirli bir IP adresine veya User-Agent bilgisine bağlı mı?
- [ ] Evet — istemcinin parmak izi (fingerprint) önemli ölçüde değişirse oturum geçersiz kılınır  
- [ ] Hayır — oturum, ek doğrulama olmaksızın farklı IP adresleri veya cihazlar arasında **tekrar kullanılabilir**  

### Oturum tanımlayıcıları, oturum kapatıldıktan veya zaman aşımına uğradıktan sonra sunucu tarafında düzgün bir şekilde geçersiz kılınıyor mu?
- [ ] Evet — oturum kapatıldığında sunucu tarafındaki oturum durumu **tamamen yok edilir**  
- [ ] Hayır — istemci tarafındaki çerez silinse bile oturum sunucuda geçerli kalmaya devam eder  
- [ ] Hayır — oturumun **süresi asla dolmaz** veya aşırı uzun bir zaman aşımı süresine sahiptir  

---