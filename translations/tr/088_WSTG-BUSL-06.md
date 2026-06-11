## WSTG-BUSL-06 — İş Akışlarını Atlatma Testi (Work Flow Circumvention)

İş akışını atlatma (Workflow circumvention), çok adımlı süreçlerde hedeflenen sıralamaları veya ön koşulları devre dışı bırakmak için uygulama mantığının manipüle edilmesini içerir. Saldırganlar; ödeme onayları, idari onaylar veya MFA tamamlama ekranları gibi hassas uç noktaları (endpoints) tespit eder ve zorunlu ara adımları atlayarak bu noktalara doğrudan erişmeye çalışırlar. Bu zafiyet, sunucu tarafı mantığının sağlam bir durum makinesi (state machine) sürdürmemesi ve bunun yerine kullanıcının kullanıcı arayüzü (UI) odaklı yolu izleyeceği varsayımına güvenmesi durumunda ortaya çıkar. Başarılı bir istismar; yetkisiz işlemler, yetki yükseltme (privilege escalation) ve kimlik doğrulama mekanizmalarının atlatılması dahil olmak üzere kritik iş mantığı (business logic) hatalarına yol açabilir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-BUSL-06 |
| **CWE** | CWE-841 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek* |

> *Atlatmanın yetkisiz finansal işlemlere, yetki yükseltmeye veya idari erişime izin vermesi durumunda önem derecesi Yüksek olur.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/06-Testing_for_the_Circumvention_of_Work_Flows  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**Araçlar:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Postman`, `Caido`

### Uygulama çok aşamalı iş süreçleri içeriyor mu?
- [ ] Hayır — uygulama yalnızca tek isteklik eylemlerden oluşuyor  
- [ ] Evet — çok aşamalı süreçler (örneğin; alışveriş sepetleri, sihirbaz formları, kayıt işlemleri) mevcuttur  

### Adım sıralaması sunucu tarafından sıkı bir şekilde denetleniyor mu?
- [ ] Evet — sunucu tarafı durum takibi (state tracking), adımların atlanamamasını sağlar  
- [ ] Evet — istemci tarafı kontroller bir sıralama önerir, ancak sunucu ilerlemeyi doğrulamaz  
- [ ] Hayır — uygulama belirli bir işlem sırasını zorunlu kılmaz  

### Bir saldırgan, terminal URL'sini veya uç noktasını (endpoint) doğrudan talep ederek bir sürecin son aşamasına ulaşabilir mi?
- [ ] Hayır — terminal adımlarına, önceki adımlar tamamlanmadan doğrudan erişim mümkün değildir  
- [ ] Evet — terminal adımlarına (örneğin; `/api/v1/checkout/complete`) doğrudan istek ile ulaşılabilir  

### Uygulama, önceki adımlardan gelen ön koşul verilerinin mevcut ve geçerli olduğunu doğruluyor mu?
- [ ] Evet — sunucu devam etmeden önce tüm önceki adım verilerini doğrular  
- [ ] Hayır — sunucu, verinin beklendiği ancak doğrulanmadığı durumlarda adımların "atlanmasına" karşı zafiyet barındırır  

### MFA veya kimlik doğrulama gibi güvenlik kontrolleri iş akışı manipüle edilerek atlatılabilir mi?
- [ ] Hayır — kritik kontroller akış manipülasyonu yoluyla atlatılamaz  
- [ ] Evet — doğrulama sonrası adıma atlayarak MFA veya kimlik kontrollerinin atlatılması mümkündür  

---