## WSTG-ATHZ-04 — Güvensiz Doğrudan Nesne Referanslarının (IDOR) Test Edilmesi

Güvensiz Doğrudan Nesne Referansları (Insecure Direct Object References - IDOR), bir uygulamanın, talepte bulunanın izinlerini doğrulamak için bir yetkilendirme kontrolü gerçekleştirmeden, kullanıcı tarafından sağlanan girdilere dayalı olarak nesnelere doğrudan erişim sağladığı durumlarda ortaya çıkar. Bu zafiyet, saldırganların diğer kullanıcılara veya sisteme ait verileri görüntülemesine, değiştirmesine veya silmesine olanak tanıdığı için gizlilik ve bütünlüğü önemli ölçüde etkiler. Genellikle dahili veritabanı anahtarlarına, dosya adlarına veya hesap tanımlayıcılarına atıfta bulunan URL parametrelerinde, POST gövde verilerinde veya JSON anahtarlarında görülür. Bir saldırgan açısından bakıldığında, sömürü süreci, yetkili kapsamlarının dışındaki kaynaklara erişmek için bu tanımlayıcıların (genellikle artan tamsayılar veya UUID'lerin Fuzzing yöntemiyle taranması yoluyla) manipüle edilmesini içerir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-ATHZ-04 |
| **CWE** | CWE-639 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/04-Testing_for_Insecure_Direct_Object_References  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/access-control  

**Araçlar:** `Burp Suite (Autorize extension)`, `Caido`, `Postman`, `ffuf`, `Intruder`

### Uygulama, istek parametrelerinde doğrudan nesne tanımlayıcıları kullanıyor mu?
- [ ] Hayır — uygulama dolaylı referanslar veya şifrelenmiş haritalar kullanıyor *(En güvenli)*  
- [ ] Evet — uygulama tahmin edilemeyen tanımlayıcılar kullanıyor (örneğin; uzun UUID'ler/hash'ler)  
- [ ] Evet — uygulama tahmin edilebilir tanımlayıcılar kullanıyor (örneğin; artan tamsayılar veya kullanıcı adları)  

### Nesne tanımlayıcısı içeren her istek için sunucu tarafı yetkilendirme kontrolü yapılıyor mu?
- [ ] Evet — sunucu tarafı kontrolleri hiçbir tanımlayıcı için atlatılamaz  
- [ ] Evet — sunucu tarafı kontrolleri mevcut ancak Parameter Pollution veya Header manipülasyonu yoluyla atlatılması mümkündür  
- [ ] Hayır — talep edilen nesnenin sahipliğini doğrulamak için hiçbir yetkilendirme kontrolü uygulanmıyor  

### Bir saldırgan, diğer kullanıcılara ait nesnelere erişebilir mi veya bunları değiştirebilir mi (Yatay IDOR)?
- [ ] Hayır — diğer kullanıcıların verilerine erişim mümkün değildir  
- [ ] Evet — diğer kullanıcıların verilerinin görüntülenmesi mümkündür ancak değiştirilmesi mümkün değildir  
- [ ] Evet — diğer kullanıcıların verilerinin hem görüntülenmesi hem de değiştirilmesi mümkündür *(Kritik)*  

### Yönetici veya sistem düzeyindeki nesnelere erişmek mümkün mü (Dikey IDOR)?
- [ ] Hayır — daha yüksek yetkili nesnelere erişim mümkün değildir  
- [ ] Evet — yönetici yapılandırmalarına veya sistem dosyalarına erişim mümkündür  

### Uygulama; arama, meta veriler veya diğer uç noktalar aracılığıyla nesne tanımlayıcılarını sızdırıyor mu?
- [ ] Hayır — tanımlayıcılar istenmeden ifşa edilmiyor  
- [ ] Evet — diğer kullanıcılar/nesneler için tanımlayıcılar, ikincil uç noktalar (endpoints) veya herkese açık profiller aracılığıyla numaralandırılabilir  

---