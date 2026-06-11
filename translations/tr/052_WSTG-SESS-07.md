## WSTG-SESS-07 — Oturum Zaman Aşımının Test Edilmesi (Testing Session Timeout)

Oturum zaman aşımı testi, bir uygulamanın önceden tanımlanmış bir hareketsizlik süresinden veya toplam süreden sonra kullanıcının oturumunu etkili bir şekilde sonlandırıp sonlandırmadığını değerlendirir. Bu kontrol; özellikle ortak kullanılan iş istasyonlarında veya bir saldırganın ağ dinleme (network interception) ya da Cross-Site Scripting (XSS) yoluyla bir oturum tanımlayıcısını ele geçirdiği senaryolarda Oturum Ele Geçirme (Session Hijacking) riskini azaltmak için kritiktir. Sızma testi uzmanları (pentesters), hem hareketsizlik süresinden sonra tetiklenen boşta kalma zaman aşımlarını (idle timeouts), hem de aktiviteden bağımsız olarak bir oturumun toplam ömrünü sınırlayan mutlak zaman aşımlarını (absolute timeouts) analiz eder. Bir saldırganın bakış açısından, belirsiz veya aşırı uzun oturumlar, yetkisiz erişimi sürdürmek ve yeniden kimlik doğrulama gereksinimini atlatmak için genişletilmiş bir fırsat penceresi sunar.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-SESS-07 |
| **CWE** | CWE-613 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/07-Testing_Session_Timeout  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Browser DevTools`

### Uygulama bir boşta kalma oturum zaman aşımı (idle session timeout) uyguluyor mu?
- [ ] Evet — oturum, kısa bir hareketsizlik süresinden (örn. 15-30 dakika) sonra sona eriyor  
- [ ] Evet — oturum sona eriyor ancak zaman aşımı süresi **aşırı uzun** (örn. > 24 saat)  
- [ ] Hayır — oturum, hareketsizlik sırasında süresiz olarak **aktif kalıyor**  

### Oturum zaman aşımı sunucu tarafında (server-side) zorunlu kılınıyor mu?
- [ ] Evet — sunucu, istemci tarafındaki durumdan bağımsız olarak süresi dolmuş belirteçlere (tokens) sahip istekleri reddediyor  
- [ ] Hayır — zaman aşımı **yalnızca** istemci tarafı JavaScript veya meta-refresh aracılığıyla zorunlu kılınıyor  

### Uygulama mutlak bir oturum zaman aşımı (absolute session timeout) uyguluyor mu?
- [ ] Evet — oturum, aktiviteden bağımsız olarak sabit bir maksimum süreden sonra sonlandırılıyor  
- [ ] Hayır — sürekli aktivite olduğu sürece oturum süresiz olarak **uzatılabiliyor**  

### Zaman aşımı durumunda oturum tanımlayıcısı sunucuda geçersiz kılınıyor mu?
- [ ] Evet — zaman aşımı eşiğine ulaşıldığında oturum belirteci (session token) **tekrar kullanılamaz**  
- [ ] Hayır — istemci oturum açma sayfasına yönlendirildikten sonra bile oturum belirteci sunucuda **hâlâ geçerlidir**  

### Otomatik kalp atışı (heartbeat) istekleri kullanılarak oturum süresiz olarak canlı tutulabilir mi?
- [ ] Hayır — mutlak zaman aşımı veya ikincil kontroller oturumun süresiz uzatılmasını **engelliyor**  
- [ ] Evet — periyodik arka plan istekleri (örn. AJAX), oturumu süresiz olarak **sürdürebilir**  

---