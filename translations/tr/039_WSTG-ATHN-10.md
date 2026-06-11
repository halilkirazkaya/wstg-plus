## WSTG-ATHN-10 — Alternatif Kanallarda Daha Zayıf Kimlik Doğrulama Testi

Alternatif kanallarda daha zayıf kimlik doğrulama testi; mobil API'ler, parola kurtarma iş akışları, yardım masası (help desk) arayüzleri veya eski (legacy) alt alan adları gibi, ana web portalı ile aynı güvenlik sıkılığına sahip olmayabilecek ikincil hesap erişim yollarının belirlenmesini ve analiz edilmesini içerir. Bu alternatif kanallar genellikle güçlü çok faktörlü kimlik doğrulama (MFA), katı hız sınırlama (Rate Limiting) veya karmaşık parola gereksinimlerinden yoksundur; bu da tüm kimlik doğrulama sistemini zayıflatan bir "en zayıf halka" oluşturur. Saldırganlar, farklı arayüzlerin kimlik doğrulamasını ele alma biçimlerindeki tutarsızlıklardan yararlanarak Kimlik Bilgisi Doldurma (Credential Stuffing) gerçekleştirmek veya MFA'yı atlatmak için özellikle bu göz ardı edilen giriş noktalarını hedefler. Yetkisiz erişimi önlemek ve kullanıcının oturum ve veri bütünlüğünü korumak için tüm kimlik doğrulama yüzeylerinde eşitliğin sağlanması esastır.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-ATHN-10 |
| **CWE** | CWE-287 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/10-Testing_for_Weaker_Authentication_in_Alternative_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite`, `Postman`, `cURL`, `MobSF`, `Ffuf`

### Alternatif kimlik doğrulama kanalları mevcut mu ve tanımlanmış mı?
- [ ] Hayır — yalnızca tek bir kimlik doğrulama kanalı mevcut  
- [ ] Evet — birden fazla kanal tanımlandı (örneğin; mobil uygulama API'si, masaüstü istemcisi, eski portal, SOAP servisleri)  

### Alternatif kanallar, ana kanal ile güvenlik eşitliğini sağlıyor mu?
- [ ] Evet — tüm kanallar **aynı** parola karmaşıklığı ve MFA gereksinimlerini uygular  
- [ ] Evet — kanallar parola karmaşıklığını uygular ancak MFA **zorunlu değildir** veya **atlatılabilir**  
- [ ] Hayır — alternatif kanallar **önemli ölçüde daha zayıf** kimlik doğrulama gereksinimlerine sahiptir  

### Hız sınırlama ve hesap kilitleme tüm kanallarda tutarlı mı?
- [ ] Evet — hız sınırlama (Rate Limiting) ve kilitleme tüm uç noktalarda **tutarlı bir şekilde uygulanmaktadır**  
- [ ] Evet — kontroller mevcuttur ancak belirli kanallarda (örneğin; mobil API) atlatma **mümkündür**  
- [ ] Hayır — ikincil kimlik doğrulama yollarına hız sınırlama veya kilitleme **uygulanmamıştır**  

### Alternatif bir kanala geçiş yapılarak MFA atlatılabilir mi?
- [ ] Hayır — MFA zorunludur ve giriş noktasından bağımsız olarak **atlatılamaz**  
- [ ] Evet — MFA web portalında gereklidir ancak mobil veya eski API üzerinde **uygulanmaz**  

### Parola kurtarma veya "parolamı unuttum" akışları standart girişten daha mı zayıf?
- [ ] Hayır — kurtarma akışları güçlü, bant dışı (out-of-band) doğrulama kullanır ve bilgi sızdırmaz  
- [ ] Evet — kurtarma akışları kolayca tahmin edilebilen veya araştırılabilen zayıf güvenlik soruları kullanır  
- [ ] Evet — kurtarma akışları numaralandırmaya (Enumeration) karşı savunmasızdır veya standart bir girişle aynı düzeyde kanıt **gerektirmez**  

---