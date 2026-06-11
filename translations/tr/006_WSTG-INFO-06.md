## WSTG-INFO-06 — Uygulama Giriş Noktalarının Belirlenmesi

Uygulama giriş noktalarının belirlenmesi, bir kullanıcının veya harici sistemin uygulama ile etkileşime girebileceği tüm olası yolların numaralandırılarak (enumeration) tüm saldırı yüzeyinin (attack surface) haritalanmasını içerir. Bu süreç; tüm URL'lerin, API uç noktalarının (endpoints), parametrelerin (GET, POST, çerez tabanlı), HTTP başlıklarının (headers) ve WebSockets veya gRPC gibi özelleşmiş protokollerin keşfedilmesini kapsar. Bir sızma testi uzmanı (penetration tester) için bu aşama hayati öneme sahiptir; çünkü zafiyetler genellikle geliştirme sırasında gözden kaçan belgelenmemiş "gölge" (shadow) API'lerde, eski (legacy) uç noktalarda veya karmaşık parametre yapılarında bulunur. Kapsamlı bir numaralandırma, uygulamanın hiçbir bileşeninin test edilmemiş bir "kara kutu" (black box) olarak kalmamasını sağlar ve böylece saldırganların sisteme giden izlenmeyen yollar bulmasını engeller.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INFO-06 |
| **CWE** | CWE-1059 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Bilgi Düzeyi (Informational) |

**Kaynaklar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/06-Identify_Application_Entry_Points  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite (Target/Proxy)`, `OWASP ZAP`, `ffuf`, `Arjun`, `LinkFinder`, `GoSpider`, `KiteRunner`

### Uygulama genelindeki tüm GET ve POST parametreleri numaralandırıldı mı?
- [ ] Evet — otomatik ve manuel tarama (crawling) yoluyla kapsamlı numaralandırma **tamamlanmıştır**  
- [ ] Evet — standart tarama yoluyla yalnızca yaygın parametreler belirlenmiştir  
- [ ] Hayır — parametreler büyük ölçüde **belirlenmemiştir** veya test edilmemiştir  

### Belgelenmemiş veya gizli parametreler (örneğin; debug, admin) fuzzing kullanılarak araştırıldı mı?
- [ ] Hayır — bu özel kapsam için gizli parametre keşfi **gerekli değildir**  
- [ ] Evet — parametre fuzzing işlemi (örneğin; `Arjun` kullanılarak) **gerçekleştirilmiştir** ve herhangi bir bulguya rastlanmamıştır  
- [ ] Evet — gizli parametreler **keşfedilmiştir** ve daha ileri testler için haritalanmıştır  
- [ ] Hayır — gizli parametre keşfi **gerçekleştirilmemiştir**  

### Her bir uç nokta için desteklenen tüm HTTP yöntemleri (PUT, DELETE, PATCH vb.) belirlendi mi?
- [ ] Evet — yöntem keşfi keşfedilen tüm uç noktalara **uygulanmıştır**  
- [ ] Evet — yöntem keşfi yalnızca yüksek öncelikli veya hassas uç noktalara **uygulanmıştır**  
- [ ] Hayır — yalnızca standart GET ve POST yöntemleri **belirlenmiştir**  

### WebSockets, gRPC gibi standart dışı giriş noktaları veya özel başlıklar haritalandı mı?
- [ ] Hayır — uygulama standart dışı protokoller veya özel başlıklar (custom headers) **kullanmamaktadır**  
- [ ] Evet — protokole özgü tüm giriş noktaları ve özel başlıklar **belirlenmiştir**  
- [ ] Hayır — standart dışı giriş noktaları **mevcut** ancak haritalanmamıştır  

### API yüzeyi (v1/ veya /v2/ gibi sürümlendirilmiş uç noktalar dahil) tamamen haritalandı mı?
- [ ] Hayır — uygulama bir API **sunmamaktadır**  
- [ ] Evet — tüm API sürümleri ve uç noktaları **belirlenmiştir** ve belgelenmiştir  
- [ ] Evet — yalnızca mevcut üretim (production) API sürümü **belirlenmiştir**  
- [ ] Hayır — API uç noktaları **haritalanmamış** veya yalnızca kısmen keşfedilmiştir  

---