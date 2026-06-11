## WSTG-BUSL-01 — İş Mantığı Veri Doğrulama Testi

İş mantığı veri doğrulama testi (Business Logic Data Validation), uygulamanın sözdizimsel (syntactic) olarak doğru ancak iş alanı bağlamında mantıksal olarak geçersiz olan verileri tanımlama ve reddetme yeteneğine odaklanır. Saldırganlar, kısıtlamaları aşmak ve uygulama durumunu manipüle etmek için bir alışveriş sepetindeki negatif miktarlar, gelecekteki etkinlikler için geçmiş tarihler veya aşırı büyük tam sayılar gibi beklenmedik değerler göndererek bu açıkları istismar ederler. Bu zafiyetler genellikle sunucu tarafı mantığının (server-side logic), kullanıcı tarafından sağlanan verilerin bağlamsal bütünlüğünü doğrulamada başarısız olduğu çok aşamalı iş akışlarında, ödeme süreçlerinde ve hesap yönetimi modüllerinde bulunur. Başarılı bir istismar (Exploit); finansal kayba, veri bütünlüğünün bozulmasına ve kısıtlanmış özelliklere veya verilere yetkisiz erişime yol açabilir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-BUSL-01 |
| **CWE** | CWE-20 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Orta |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/01-Test_Business_Logic_Data_Validation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**Araçlar:** `Burp Suite (Repeater/Intruder)`, `Postman`, `Python (Requests)`, `OWASP ZAP`

### Uygulama, sayısal girdiler üzerinde mantıksal aralık sınırlarını zorunlu kılıyor mu?
- [ ] Evet — uygulama, uygunsuz durumlarda negatif, sıfır veya aşırı büyük değerleri doğru bir şekilde reddeder *(En güvenli)*  
- [ ] Evet — uygulama bazı geçersiz aralıkları reddeder ancak tam sayı taşması (integer overflow) veya sınır atlatmaya karşı **savunmasızdır**  
- [ ] Hayır — uygulama, iş bağlamına bakılmaksızın her türlü sayısal değeri kabul eder *(Kritik)*  

### Sunucu tarafı doğrulama kontrolleri uygulamanın tüm katmanlarında tutarlı mı?
- [ ] Evet — doğrulama, hem API/servis hem de veritabanı katmanlarında tutarlı bir şekilde uygulanır  
- [ ] Evet — doğrulama UI/Frontend tarafında **uygulanır** ancak doğrudan API isteği yoluyla atlatma (bypass) **mümkündür**  
- [ ] Hayır — doğrulama **uygulanmaz** veya tutarsızdır, bu da mantıksal veri bozulmasına izin verir  

### İş mantığı, çok aşamalı iş akışlarındaki veriler manipüle edilerek bozulabilir mi?
- [ ] No — durum (state) sunucuda güvenli bir şekilde tutulur ve önceki adımlardan gelen veriler **değiştirilemez**  
- [ ] Evet — doğrulama ilk adımlarda gerçekleşir ancak son gönderim verilerin bütünlüğünü **tekrar doğrulamaz**  
- [ ] Evet — adımları atlayarak veya sıra dışı veriler göndererek iş akışı atlatma (workflow bypass) **mümkündür**  

### Uygulama, iş kısıtlamalarını aşan "uç durum" (edge case) verilerini düzgün bir şekilde yönetiyor mu?
- [ ] Evet — kısıtlamalar sağlamdır ve olağandışı ancak sözdizimsel olarak geçerli girdileri (örneğin artık yıllar, saat dilimi değişimleri) işler  
- [ ] Evet — bazı uç durumlar işlenir ancak belirli kombinasyonlar beklenmedik davranışlara yol **açabilir**  
- [ ] Hayır — uç durumlar; ücretsiz satın alımlar veya yetkisiz durum değişiklikleri gibi doğrudan mantık hatalarına yol açar  

---