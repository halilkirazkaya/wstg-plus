## WSTG-BUSL-07 — Uygulama Suistimaline Karşı Savunmaların Test Edilmesi

Uygulama suistimaline karşı savunmalar; uygulamanın, amaçlanan iş mantığından (Business Logic) sapan anormal kullanıcı davranışlarını tanımlama, günlüğe kaydetme (logging) ve bunlara yanıt verme yeteneğini değerlendirir. Geleneksel imza tabanlı güvenliğin aksine bu savunmalar; hızlı istekler (rapid-fire requests), kimlik bilgisi doldurma (Credential Stuffing) veya otomatik suistimali işaret eden ardışık kaynak numaralandırma (sequential resource enumeration) gibi kalıpları tespit etmeye odaklanır. Bir saldırganın perspektifinden bu test; uygulamanın, standart güvenlik uyarılarını tetiklemeden veri sızdırmak veya kaynakları tüketmek için tasarlanan otomasyona ve "düşük ve yavaş" (low and slow) saldırılara karşı direncini belirler. Bu savunmaların uygulanmaması; genellikle büyük ölçekli veri kazıma (data scraping), hesap ele geçirme (account takeover) veya meşru ancak kaynak yoğun uygulama özellikleri üzerinden hizmet dışı bırakma (Denial of Service - DoS) ile sonuçlanır.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-BUSL-07 |
| **CWE** | CWE-799 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/07-Test_Defenses_Against_Application_Misuse  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite (Intruder/Turbo Intruder)`, `OWASP ZAP`, `Python (Requests/Selenium)`, `Caido`, `K6`

### Hassas uç noktalarda hız sınırlama veya kısıtlama mekanizmaları zorunlu kılınmış mı?
- [ ] Evet — tüm hassas uç noktalarda katı hız sınırlama (Rate Limiting) **uygulanmaktadır** ve zorunludur *(En güvenli)*  
- [ ] Evet — hız sınırlama **uygulanmaktadır** ancak IP rotasyonu veya başlık (header) manipülasyonu ile atlatılabilmektedir  
- [ ] Evet — hız sınırlama yalnızca kimlik doğrulama uç noktalarına **uygulanmaktadır**, diğerleri açıkta bırakılmıştır  
- [ ] Hayır — hiçbir uygulama özelliği için hız sınırlama veya kısıtlama (throttling) **uygulanmamaktadır**  

### Uygulama, otomatik etkileşim kalıplarını tespit edip engelliyor mu?
- [ ] Evet — davranışsal analiz veya CAPTCHA'lar **etkindir** ve botları etkili bir şekilde engellemektedir  
- [ ] Evet — otomasyon karşıtı önlemler **etkindir** ancak başlıksız tarayıcılar (headless browsers) veya oturum döngüsü (session cycling) kullanılarak atlatma **mümkündür**  
- [ ] Hayır — uygulama, insan kullanıcı ile otomatik bir betik (script) arasındaki farkı **ayırt edememektedir**  

### Anormal davranışlar günlüğe kaydediliyor mu ve ardından savunmacı bir yanıt veriliyor mu?
- [ ] Evet — suistimal, otomatik engellemeyi tetikler ve güvenlik ekibini uyarır  
- [ ] Evet — suistimal denetim için günlüğe kaydedilir ancak otomatik bir savunma eylemi **gerçekleştirilmez**  
- [ ] Hayır — uygulama, olağan dışı aktivite kalıplarını günlüğe **kaydetmemekte** veya bunlara yanıt **vermemektedir**  

### Savunmalar, istemci tarafı tanımlayıcıları veya başlıkları manipüle edilerek atlatılabilir mi?
- [ ] Hayır — savunmalar, sunucu tarafı durumuna ve doğrulanmış telemetriye dayanmaktadır  
- [ ] Evet — kontroller, çerezleri (cookies) temizleyerek veya `User-Agent` dizelerini döndürerek **atlatılabilmektedir**  
- [ ] Evet — kontroller, `X-Forwarded-For` veya `X-Real-IP` gibi başlıklar (headers) enjekte edilerek **atlatılabilmektedir**  

---