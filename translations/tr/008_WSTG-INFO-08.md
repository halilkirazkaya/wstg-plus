## WSTG-INFO-08 — Web Uygulama Çerçevesi Parmak İzi Belirleme (Fingerprint Web Application Framework)

Bir web uygulama çerçevesinin (framework) parmak izinin belirlenmesi (fingerprinting), uygulamayı oluşturmak ve sunmak için kullanılan spesifik teknoloji yığınını ve yazılım sürümlerini tanımlamayı içerir. Saldırganlar; hedefin React, Angular, Django veya Spring gibi platformlar üzerinde çalışıp çalışmadığını belirlemek için HTTP yanıt başlıklarını (headers), çerçeveye özgü çerezleri (cookies), öngörülebilir dosya yapılarını ve benzersiz JavaScript bileşenlerini analiz ederler. Bu keşif (reconnaissance) aşaması, test uzmanının saldırı yüzeyini (attack surface) bilinen zafiyetlere (CVE'ler) ve söz konusu çerçeveye özgü yaygın yapılandırma zayıflıklarına göre daraltmasına olanak tanıdığı için hayati önem taşır. Saldırgan, temel mimariyi anlayarak genel testlerden yüksek düzeyde hedeflenmiş sömürme (exploitation) stratejilerine geçiş yapabilir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INFO-08 |
| **CWE** | CWE-200 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Bilgi Amaçlı (Informational) |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/08-Fingerprint_Web_Application_Framework  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Wappalyzer`, `BuiltWith`, `WhatWeb`, `nmap`, `Burp Suite`, `curl`

### HTTP yanıt başlıkları çerçeve adını veya sürümünü ifşa ediyor mu?
- [ ] Hayır — `X-Powered-By` veya `Server` gibi başlıklar mevcut **değil** veya sanitize edilmiş (arındırılmış)  
- [ ] Evet — çerçeve adı mevcut ancak sürüm **gizli**  
- [ ] Evet — hem çerçeve adı hem de sürüm **ifşa ediliyor**  

### Çerçeveye özgü çerezler veya dizin yapıları tanımlanabiliyor mu?
- [ ] Hayır — yaygın çerçeve bileşenleri (örneğin `.js` yolları, çerez adları) karartılmış (obfuscated) veya yeniden adlandırılmış  
- [ ] Evet — çerçeveye özgü çerezler (örneğin `JSESSIONID`, `PHPSESSID`, `csrftoken`) tanımlanabiliyor  
- [ ] Evet — dizin yapıları (örneğin `/wp-content/`, `_next/static/`) görünüyor ve çerçeveyi doğruluyor  

### Hata mesajları veya kaynak kod yorumları çerçeve ayrıntılarını açığa çıkarıyor mu?
- [ ] Hayır — hata yönetimi genel (generic) yapılmış ve yorumlar temizlenmiş  
- [ ] Evet — çerçeveye özgü yığın izleri (stack traces) **etkinleştirilmiş** ve yanıtlarda görünüyor  
- [ ] Evet — HTML kaynak kodu, çerçeveyi tanımlayan yorumlar veya meta veri etiketleri içeriyor  

### Tanımlanan çerçeve sürümü ile ilişkili bilinen zafiyetler var mı?
- [ ] Hayır — tanımlanan çerçeve güncel ve bilinen herhangi bir genel zafiyeti **yok**  
- [ ] Evet — çerçeve sürümü güncel değil ve spesifik CVE'ler tanımlanabiliyor  

---