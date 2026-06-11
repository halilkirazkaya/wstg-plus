## WSTG-CLNT-02 — JavaScript Yürütme Testi

JavaScript yürütme testi, bir uygulamanın kullanıcı tarafından sağlanan verileri hatalı şekilde işlemesi sonucunda mağdurun tarayıcı bağlamında yetkisiz betiklerin çalıştırılmasına neden olan durumların tespit edilmesine odaklanır. Bu zafiyet tipik olarak, saldırganların oturum çerezlerini (session cookies) ele geçirmesine, Document Object Model (DOM) yapısını manipüle etmesine veya kullanıcı adına eylemler gerçekleştirmesine olanak tanıyan Cross-Site Scripting (XSS) ile sonuçlanır. Sızma testi uzmanları; URL fragmanları, `localStorage` veya `window.name` gibi kaynaklardan (sources) gelen sanitize edilmemiş verilerin işlenebileceği `innerHTML`, `document.write()` ve `eval()` gibi alıcıları (sinks) inceler. Başarılı bir istismar, kullanıcı oturumunun bütünlüğünü ve gizliliğini tehlikeye atar ve daha karmaşık istemci taraflı (client-side) saldırılar için bir basamak (pivot) görevi görebilir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CLNT-02 |
| **CWE** | CWE-79 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/02-Testing_for_JavaScript_Execution  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/dom-based  
* https://portswigger.net/web-security/prototype-pollution  

**Araçlar:** `Burp Suite`, `Browser Developer Tools`, `XSStrike`, `DOMPurify`, `KNOXSS`

### Uygulama, kullanıcı tarafından sağlanan verileri tehlikeli JavaScript alıcılarında (sinks) işliyor mu?
- [ ] Hayır — tüm veriler sanitize edilmiştir veya `textContent` gibi güvenli alıcılarda kullanılmaktadır  
- [ ] Evet — veriler tehlikeli alıcılarda işlenmektedir ancak sanitization/encoding **uygulanmıştır**  
- [ ] Evet — veriler tehlikeli alıcılarda işlenmektedir ve sanitization **uygulanmamıştır**  

### Betik yürütmeyi hafifletmek için bir Content Security Policy (CSP) mevcut mu?
- [ ] Evet — kısıtlayıcı bir CSP **etkindir** ve bypass **mümkün değildir**  
- [ ] Evet — bir CSP **etkindir** ancak `unsafe-inline` veya güvensiz CDN'ler aracılığıyla bypass **mümkündür**  
- [ ] Hayır — herhangi bir CSP **etkin değildir**  

### JavaScript, URL parametreleri veya fragmanları aracılığıyla yürütülebiliyor mu (DOM-based XSS)?
- [ ] Hayır — girdi uygun şekilde encode edilmiştir ve yürütme **mümkün değildir**  
- [ ] Evet — girdi DOM'a yansıtılmaktadır ancak yürütme modern framework korumaları tarafından **engellenmektedir**  
- [ ] Evet — girdi doğrudan tarayıcıda yürütülmektedir ve istismar (exploitation) **mümkündür**  

### Olay işleyiciler (event handlers) veya URI şemaları betik enjeksiyonuna karşı savunmasız mı?
- [ ] Hayır — girdi, olay özniteliklerini ve `javascript:` şemalarını kaldıracak şekilde sanitize edilmiştir  
- [ ] Evet — olay işleyiciler enjekte **edilebilmektedir** ancak filtreler payload karmaşıklığını sınırlandırmaktadır  
- [ ] Evet — `onerror`, `onload` veya `href` öznitelikleri aracılığıyla keyfi JavaScript yürütülmesi **mümkündür**  

---