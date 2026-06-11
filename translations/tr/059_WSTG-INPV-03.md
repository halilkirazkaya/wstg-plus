## WSTG-INPV-03 — HTTP Verb Tampering (HTTP Fiil Kurcalama) Testi

HTTP Verb Tampering (HTTP Fiil Kurcalama), web sunucularının ve uygulama çatılarının kullanılan HTTP yöntemine bağlı olarak belirli kaynaklara erişimi yetkilendirme şeklindeki zayıflıkları istismar eder. Saldırganlar, `GET` veya `POST` gibi standart yöntemleri; `HEAD`, `PUT`, `OPTIONS` gibi alternatiflerle veya arka ucun (backend) tutarsız bir şekilde işleyebileceği rastgele, standart dışı dizelerle değiştirerek güvenlik kısıtlamalarını atlatmaya çalışırlar. Bu güvenlik zafiyeti genellikle, Java EE web.xml filtreleri veya .NET yetkilendirme kuralları gibi güvenlik yapılandırmalarının izin verilen yöntemleri açıkça listelediği ancak diğerlerini hesaba katmadığı durumlarda veya "her şeyi reddet" (deny-all) yaklaşımı yerine "yönteme göre reddet" (deny-by-method) yaklaşımı kullanıldığında ortaya çıkar. Başarılı bir istismar; yönetim işlevlerine yetkisiz erişim, veri değişikliği veya sunucunun dahili yapılandırmasıyla ilgili bilgilerin ifşa edilmesiyle sonuçlanabilir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INPV-03 |
| **CWE** | CWE-288 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek* |

> *Yönetim işlevleri veya veri değişikliği (PUT/DELETE) kimlik doğrulaması olmadan gerçekleştirilebiliyorsa önem derecesi Yüksek olur.

**Kaynaklar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/03-Testing_for_HTTP_Verb_Tampering  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite (Repeater/Intruder)`, `curl`, `nmap`, `ffuf`

### Uygulama, standart dışı veya rastgele HTTP yöntemlerine yanıt veriyor mu?
- [ ] Hayır — uygulama, açıkça gerekli olanlar dışındaki tüm yöntemleri reddeder  
- [ ] Evet — uygulama standart yöntemlere (OPTIONS, TRACE) yanıt verir ancak güvenlik atlatılamaz  
- [ ] Evet — uygulama rastgele dizeleri (örneğin FOO, TEST) kabul eder ve bunları `GET` veya `POST` istekleri gibi işler  

### Kimlik doğrulama/yetkilendirme, HTTP yöntemi değiştirilerek atlatılabilir mi?
- [ ] Hayır — güvenlik denetimleri, HTTP fiilinden bağımsız olarak küresel olarak uygulanır  
- [ ] Evet — denetimler mevcuttur ancak `HEAD` yöntemi kullanılarak bypass (atlatma) mümkündür  
- [ ] Evet — güvenlik denetimleri alternatif yöntemlere uygulanmaz, bu da kısıtlanmış uç noktalara (endpoints) yetkisiz erişime izin verir  

### Sunucuda `PUT` veya `DELETE` gibi tehlikeli yöntemler etkin mi?
- [ ] Hayır — tehlikeli yöntemler devre dışıdır veya 405 Method Not Allowed döndürür  
- [ ] Evet — yöntemler etkindir ancak geçerli, yüksek ayrıcalıklı kimlik doğrulaması gerektirir  
- [ ] Evet — yöntemler etkindir ve yetkisiz dosya yüklemeye veya kaynak silmeye izin verir *(Kritik)*  

### `OPTIONS` yöntemi, izin verilen fiiller hakkında hassas bilgiler açığa çıkarıyor mu?
- [ ] Hayır — `OPTIONS` devre dışıdır veya genel bir yanıt döndürür  
- [ ] Evet — `OPTIONS`, `Allow` üst bilgisini döndürür ancak kısıtlanmış dahili yöntemleri ifşa etmez  
- [ ] Evet — `OPTIONS`, daha fazla istismara (exploit) yardımcı olan yalnızca dahili veya yönetici yöntemlerini ifşa eder  

### `TRACE` yöntemi etkin mi ve potansiyel olarak Cross-Site Tracing (XST) (Siteler Arası İzleme) işlemine izin veriyor mu?
- [ ] Hayır — `TRACE` ve `TRACK` yöntemleri devre dışıdır  
- [ ] Evet — `TRACE` etkindir ve hassas çerezler/belirteçler (tokens) dahil olmak üzere HTTP üst bilgilerini geri yansıtır  

---