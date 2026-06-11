## WSTG-INPV-16 — HTTP Request Smuggling

HTTP Request Smuggling (HRS), genellikle çelişen `Content-Length` ve `Transfer-Encoding` başlıkları nedeniyle bir HTTP sunucu zincirinin (yük dengeleyici ve arka uç web sunucusu gibi) bir isteğin sınırlarını farklı şekilde yorumlamasıyla oluşur. Bir saldırgan, bu desenkronizasyonu kullanarak arka uç sunucusunun istek arabelleğine bir giriş "sızdırır" (smuggle) ve bir sonraki meşru kullanıcının isteğinin başına saldırgan kontrollü bir segment ekleyerek bu isteği fiilen ön ekler. Bu teknik; oturum ele geçirme (session hijacking), güvenlik kontrollerini atlatma ve önbellek zehirlemesi (cache poisoning) gibi ciddi saldırılara olanak tanır. İstismar genellikle zaman tabanlı analizle veya sunucuya sonraki istekler gönderildiğinde arka uçtan gelen beklenmedik yanıtların gözlemlenmesiyle gerçekleştirilir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INPV-16 |
| **CWE** | CWE-444 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Kritik / Yüksek |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/16-Testing_for_HTTP_Request_Smuggling  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  
* https://portswigger.net/web-security/request-smuggling  

**Araçlar:** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `curl`, `SmuggleHunter`

### Ön uç ve arka uç sunucuları `Transfer-Encoding: chunked` başlığını tutarlı bir şekilde işliyor mu?
- [ ] Evet — her iki sunucu da chunked kodlamayı aynı şekilde işler ve desenkronizasyon **mümkün değildir**  
- [ ] Hayır — sunucular başlıkları farklı yorumlar ancak sanitizasyon/normalizasyon **uygulanmaktadır**  
- [ ] Hayır — CL.TE (Content-Length/Transfer-Encoding) desenkronizasyonu **mümkündür** *(Kritik)*  
- [ ] Hayır — TE.CL (Transfer-Encoding/Content-Length) desenkronizasyonu **mümkündür** *(Kritik)*  

### Saldırgan, sızdırılmış (smuggled) bir istek kullanarak sunucu veya istemci tarafı önbelleğini zehirleyebilir mi?
- [ ] Hayır — önbellekleme **devre dışıdır** veya zehirlenmeye karşı duyarlı değildir  
- [ ] Evet — sızdırılmış istekler, önbellek zehirlemesi (cache poisoning) yoluyla kullanıcıları kötü amaçlı alan adlarına yönlendirebilir veya betik enjekte edebilir  

### İstek birleştirme (request concatenation) yoluyla diğer kullanıcıların isteklerini veya oturum belirteçlerini (session tokens) ele geçirmek mümkün müdür?
- [ ] Hayır — sızdırma (smuggling), kullanıcılar arası veri ifşasına **izin vermez**  
- [ ] Evet — sızdırma, bir sonraki kullanıcının isteğinin saldırgan kontrollü bir `POST` gövdesine eklenmesine izin vererek veri sızdırmayı (exfiltration) **mümkün kılar**  

### Altyapı HTTP/2 kullanıyor mu ve H2.CL veya H2.TE desenkronizasyonuna karşı duyarlı mı?
- [ ] Hayır — uygulama yalnızca HTTP/1.1 kullanıyor veya HTTP/2, sürüm düşürme (downgrade) olmaksızın güvenli bir şekilde uygulanmış  
- [ ] Evet — HTTP/2'den HTTP/1.1'e düz metin sürüm düşürmeleri (cleartext downgrades) gerçekleşiyor ve desenkronizasyon **mümkündür**  

### Hatalı biçimlendirilmiş başlıklar (örneğin, boşluk içeren `Transfer-Encoding:  chunked`) güvenli bir şekilde işleniyor mu?
- [ ] Evet — hatalı biçimlendirilmiş başlıklar tüm katmanlarda reddedilir veya normalize edilir  
- [ ] Hayır — hatalı biçimlendirilmiş veya gizlenmiş başlıkların tutarsız işlenmesi nedeniyle TE.TE desenkronizasyonu **mümkündür**  

---