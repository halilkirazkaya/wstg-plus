## WSTG-CLNT-01 — DOM Tabanlı Cross-Site Scripting (XSS) Testi

DOM tabanlı Cross-Site Scripting (DOM XSS), bir uygulama güvenilmeyen bir kaynaktan gelen veriyi güvenli olmayan bir şekilde işleyen istemci tarafı JavaScript (client-side JavaScript) içerdiğinde, genellikle veriyi tehlikeli bir alıcı (sink) aracılığıyla Belge Nesne Modeli (DOM) üzerine geri yazdığında meydana gelir. Reflected veya Stored XSS'in aksine, bu zafiyet tamamen istemci tarafı kodunda mevcuttur; `innerHTML`, `eval()` veya `document.write()` gibi alıcılar tarafından işlendiği için yük (Payload) genellikle sunucuya hiç gönderilmez. Saldırganlar, kurbanın tarayıcısı tarafından yüklendiğinde kullanıcının oturum bağlamında rastgele JavaScript yürüten, kötü amaçlı fragmanlar veya parametreler içeren URL'ler hazırlayarak bu durumdan faydalanırlar. Bu durum, oturum belirteçlerinin (session tokens) sızdırılmasına, hassas veri hırsızlığına ve kimliği doğrulanmış kullanıcı adına yetkisiz işlemler gerçekleştirilmesine yol açabilir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CLNT-01 |
| **CWE** | CWE-79 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/01-Testing_for_DOM-based_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  
* https://portswigger.net/web-security/cross-site-scripting  
* https://portswigger.net/web-security/dom-based  

**Araçlar:** `Burp Suite (DOM Invader)`, `Browser DevTools`, `KNOXSS`, `XSStrike`, `Snyk (SAST)`

### JavaScript içerisinde kullanıcı kontrollü verileri yakalamak için güvenilmeyen kaynaklar kullanılıyor mu?
- [ ] Hayır — JavaScript `location.hash`, `location.search` veya `document.referrer` **kullanmamaktadır**  
- [ ] Evet — güvenilmeyen kaynaklar kullanılmaktadır ancak veri herhangi bir yürütme alıcısına (execution sink) **iletilmemektedir**  
- [ ] Evet — güvenilmeyen kaynaklar kullanılmaktadır ve veri doğrudan istemci tarafı mantığına akmaktadır  

### Kullanıcı kontrollü veri, tehlikeli DOM alıcılarına (sinks) iletiliyor mu?
- [ ] Hayır — `innerHTML`, `outerHTML`, `document.write()` veya `eval()` gibi alıcılar **kullanılmamaktadır**  
- [ ] Evet — tehlikeli alıcılar mevcuttur ancak veri `DOMPurify` gibi güvenli bir kütüphane kullanılarak sıkı bir şekilde **temizlenmektedir (sanitized)**  
- [ ] Evet — tehlikeli alıcılar mevcuttur ve veri **yetersiz** veya özel regex tabanlı filtreleme ile işlenmektedir  
- [ ] Evet — tehlikeli alıcılar mevcuttur ve veri herhangi bir temizleme veya kodlama (encoding) **yapılmadan** iletilmektedir *(Kritik)*  

### Yürütme alıcısına (execution sink) URL tabanlı bir yük (Payload) ile ulaşılabilir mi?
- [ ] Hayır — kaynaktan alıcıya (source-to-sink) akış, harici girdi yoluyla **tetiklenemez**  
- [ ] Evet — akış **mümkündür** ancak karmaşık kullanıcı etkileşimi gerektirir  
- [ ] Evet — akış **mümkündür** ve basit bir URL fragmanı veya sorgu parametresi ile tetiklenebilir  

### Modern güvenlik başlıkları veya tarayıcı tabanlı hafifletmeler riski etkisiz hale getiriyor mu?
- [ ] Evet — `script-src` ve `trusted-types` içeren sıkı bir `Content-Security-Policy` (CSP) **etkindir** ve yürütmeyi engeller  
- [ ] Evet — bir CSP mevcuttur ancak `unsafe-inline` veya zayıf hash değerleri bir atlatmayı (bypass) **mümkün** kılmaktadır  
- [ ] Hayır — DOM tabanlı yürütmeyi hafifletmek için bir CSP bulunmamaktadır  

### Uygulama, yerleşik korumalara sahip istemci tarafı framework'leri kullanıyor mu?
- [ ] Evet — framework (örneğin; Angular, React) veriyi otomatik olarak kodlar ve atlatma (bypass) **mümkün değildir**  
- [ ] Evet — framework kullanılmaktadır ancak `dangerouslySetInnerHTML` gibi "kaçış noktaları" **etkindir**  
- [ ] Hayır — uygulama, otomatik kaçış (auto-escaping) özelliği olmayan yalın JavaScript (vanilla JavaScript) veya eski kütüphaneler (örneğin; eski jQuery sürümleri) kullanmaktadır  

---