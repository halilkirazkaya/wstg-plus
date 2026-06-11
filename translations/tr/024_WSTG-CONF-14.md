## WSTG-CONF-14 — Diğer HTTP Güvenlik Başlığı Yanlış Yapılandırmalarının Test Edilmesi

HTTP güvenlik başlığı yanlış yapılandırmalarının test edilmesi, yaygın web tabanlı saldırı vektörlerine karşı temel koruma sağlayan savunma başlıklarının varlığının ve doğru uygulandığının doğrulanmasını içerir. Bu başlıklar temel kod kusurlarını düzeltmese de, eksiklikleri; Ortadaki Adam (Man-in-the-Middle - MITM) saldırıları, MIME-sniffing (MIME türü koklama) istismarı ve yönlendiriciler (referrers) aracılığıyla istenmeyen kaynaklar arası veri sızıntısı riskini önemli ölçüde artırır. Bir saldırganın perspektifinden bakıldığında, eksik güvenlik başlıkları, kötü sıkılaştırılmış bir ortamın yüksek görünürlüklü göstergeleridir ve genellikle oturum çalma (session hijacking) veya bilgi sızdırma (information exfiltration) gibi daha karmaşık istismar zincirlerini kolaylaştırır. Bu yapılandırmalar genellikle sunucu düzeyinde değerlendirilir ve API uç noktaları ile statik kaynaklar dahil olmak üzere tüm yanıt türlerinde tutarlı bir şekilde uygulanmalıdır.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CONF-14 |
| **CWE** | CWE-16 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Düşük / Orta |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/14-Test_Other_HTTP_Security_Header_Misconfigurations  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `curl`, `Burp Suite (Repeater/Scanner)`, `OWASP ZAP`, `Hardenize`, `securityheaders.com`

### Güvenlik Başlığı Varlık Denetimi
| Başlık | Mevcut | Eksik | Önerilen Yapılandırma |
|--------|:-------:|:-------:|---------------------------|
| `Strict-Transport-Security` |  |  | `max-age=63072000; includeSubDomains; preload` |
| `X-Content-Type-Options` |  |  | `nosniff` |
| `Referrer-Policy` |  |  | `no-referrer` veya `strict-origin-when-cross-origin` |
| `Permissions-Policy` |  |  | (Özelliğe özgü, örn. `geolocation=()`) |
| `Cross-Origin-Opener-Policy` |  |  | `same-origin` |

### Güvenlik başlıkları uygulama kapsamında nasıl zorunlu kılınmaktadır?
- [ ] Evet — başlıklar merkezi bir yük dengeleyici (load balancer) veya ters vekil sunucu (reverse proxy) aracılığıyla global olarak uygulanır ve **geçersiz kılınamaz**  
- [ ] Evet — başlıklar ana doküman yanıtlarına uygulanır ancak API yanıtlarında veya statik varlıklarda **eksiktir**  
- [ ] Hayır — başlıklar tutarsız uygulanmıştır veya tüm uygulama alan adında **eksiktir**  

### Strict-Transport-Security (HSTS) başlığı doğru yapılandırılmış mı?
- [ ] Evet — `max-age` uzun bir süreye (örn. 2 yıl) ayarlanmış ve `includeSubDomains` **etkinleştirilmiştir**  
- [ ] Evet — `max-age` ayarlanmış ancak `includeSubDomains` **devre dışı bırakılmıştır**, bu durum alt alan adlarını savunmasız bırakır  
- [ ] Hayır — HSTS başlığı **mevcut değil** veya `max-age` 0 olarak ayarlanmış; bu durum protokol düşürme (protocol downgrade) saldırılarına izin verir  

### Referrer-Policy hassas URL parametrelerinin sızmasını engelliyor mu?
- [ ] Evet — politika `no-referrer` veya `same-origin` olarak ayarlanmış *(En güvenli)*  
- [ ] Evet — politika `strict-origin-when-cross-origin` olarak ayarlanmış  
- [ ] Hayır — politika **ayarlanmamış** veya `unsafe-url` olarak ayarlanmış; bu durum referer başlıklarında token'ların veya hassas Kişisel Olarak Tanımlanabilir Bilgilerin (PII) sızmasına neden olabilir  

### X-Content-Type-Options başlığı MIME-sniffing'i engelliyor mu?
- [ ] Evet — `nosniff` direktifi **mevcuttur** ve doğru şekilde uygulanmıştır  
- [ ] Hayır — başlık **eksiktir**; tarayıcının yürütülebilir olmayan dosyaları (örn. resimler) betik (script) olarak çalıştırmasına potansiyel olarak izin verir  

---