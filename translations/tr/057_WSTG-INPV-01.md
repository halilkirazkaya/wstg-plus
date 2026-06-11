## WSTG-INPV-01 — Yansıtılmış Siteler Arası Betik Çalıştırma (Reflected Cross Site Scripting - XSS)

Yansıtılmış Siteler Arası Betik Çalıştırma (Reflected Cross Site Scripting - XSS), bir uygulama güvenilmeyen verileri yeterli doğrulama veya kodlama (encoding) yapmadan bir HTTP yanıtına dahil ettiğinde ve payload'un kurbanın tarayıcı bağlamında çalışmasına neden olduğunda meydana gelir. Saldırganlar; kullanıcı oturumlarını ele geçirmek, hassas çerezleri (cookies) sızdırmak veya kullanıcı adına yetkisiz eylemler gerçekleştirmek için sosyal mühendislik yoluyla, genellikle özel olarak hazırlanmış URL'ler veya formlar aracılığıyla kötü amaçlı payload'lar iletirler. Bu zafiyet; arama parametrelerinde, hata mesajlarında ve girdiyi doğrudan kullanıcı arayüzüne geri yansıtan her türlü uç noktada (endpoint) yaygın olarak bulunur. Sömürü (exploitation), kurbanın kötü amaçlı bir bağlantıyla etkileşime girmesine dayanır; bu da onu kalıcı olmayan (non-persistent) ancak belirli yönetici veya kimliği doğrulanmış kullanıcıları hedeflemek için oldukça etkili bir saldırı vektörü haline getirir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INPV-01 |
| **CWE** | CWE-79 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/01-Testing_for_Reflected_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/cross-site-scripting  

**Araçlar:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `XSStrike`, `KXS`, `dalfox`

### Kullanıcı tarafından sağlanan girdiler yanıt gövdesinde (response body) yansıtılıyor mu?
- [ ] Hayır — girdi kullanıcıya **hiçbir zaman** geri yansıtılmıyor  
- [ ] Evet — girdi yansıtılıyor ancak uygun şekilde kodlanmış (encoded) ve atlatma (bypass) **mümkün değil**  
- [ ] Evet — girdi herhangi bir kodlama veya temizleme (sanitization) **olmadan** yansıtılıyor  

### Bağlam duyarlı çıktı kodlaması (context-aware output encoding) uygulanmış mı?
- [ ] Evet — belirli yansıma bağlamına göre uygun kodlama (HTML, Attribute, JavaScript) **uygulanmış**  
- [ ] Evet — kodlama **uygulanmış** ancak belirli bağlam için yetersiz (örneğin, bir `<script>` etiketi içinde HTML kodlaması)  
- [ ] Hayır — kodlama **uygulanmamış**  

### Girdi doğrulaması veya Web Uygulaması Güvenlik Duvarı (WAF) filtreleri atlatılabilir (bypass) mi?
- [ ] Hayır — filtreler tüm yaygın ve gizlenmiş (obfuscated) XSS payload'larını etkili bir şekilde engelliyor  
- [ ] Evet — filtreler mevcut ancak karakter kodlaması, standart dışı etiketler veya poliglolar (polyglots) kullanılarak atlatma (bypass) **mümkün**  
- [ ] Hayır — hiçbir filtre veya doğrulama mekanizması devrede değil  

### Yansıtılan girdinin yürütme bağlamı (execution context) nedir?
- [ ] Güvenli — girdi, yürütülemez bir konumda yansıtılıyor (örneğin, standart `<div>` veya `<span>` etiketleri içinde)  
- [ ] Riskli — girdi HTML öznitelikleri (attributes) içinde veya etiketlerin `src`/`href` bölümlerinde yansıtılıyor  
- [ ] Kritik — girdi doğrudan `<script>` blokları, olay işleyicileri (event handlers) veya şablon sabitleri (template literals) içinde yansıtılıyor  

### XSS etkisini azaltmak için modern tarayıcı güvenlik başlıkları (security headers) mevcut mu?
- [ ] Evet — `Content-Security-Policy` (CSP), kısıtlayıcı bir script-src ile **etkinleştirilmiş**  
- [ ] Evet — `Content-Security-Policy` (CSP) **etkinleştirilmiş** ancak `unsafe-inline` veya zayıf beyaz listeler (whitelists) içeriyor  
- [ ] Hayır — hiçbir CSP veya eski `X-XSS-Protection` başlığı mevcut değil  

---