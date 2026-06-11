## WSTG-CONF-12 — Content Security Policy (CSP)

İçerik Güvenliği Politikası (Content Security Policy - CSP), Cross-Site Scripting (XSS), clickjacking ve veri enjeksiyonu saldırıları gibi riskleri azaltmak amacıyla HTTP yanıt başlıkları aracılığıyla uygulanan kritik bir derinlemesine savunma (defense-in-depth) mekanizmasıdır. Hangi dinamik kaynakların yüklenmesine izin verileceğini tanımlayarak, iyi yapılandırılmış bir CSP, bir saldırganın yetkisiz script çalıştırma veya hassas verileri harici domainlere sızdırma (exfiltrate) yeteneğini kısıtlar. Pentesterlar, politikayı aşırı izin verici direktifler, `'unsafe-inline'` gibi güvenli olmayan anahtar kelimelerin kullanımı ve atlatmaları (bypass) kolaylaştırabilecek güvensiz CDN'lere güvenme açısından değerlendirir. Zayıf veya eksik bir CSP doğrudan bir zafiyet oluşturmaz, ancak ikincil bir koruma katmanı sağlamayarak başarılı enjeksiyon saldırılarının etkisini önemli ölçüde artırır.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CONF-12 |
| **CWE** | CWE-693 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Düşük / Orta |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/12-Test_for_Content_Security_Policy  
* https://hacktricks.wiki/en/pentesting-web/content-security-policy-csp-bypass/index.html  

**Araçlar:** `Google CSP Evaluator`, `Burp Suite (CSP Pro)`, `Mozilla Observatory`, `ZAP`, `CSP Mitigator`

### Uygulama yanıtlarında bir Content Security Policy (CSP) başlığı mevcut mu?
- [ ] Evet — `Content-Security-Policy` başlığı **mevcut** ve **zorunlu kılınmış (enforced)**  
- [ ] Evet — Test amaçlı `Content-Security-Policy-Report-Only` **mevcut**  
- [ ] Hayır — Yanıtlarda herhangi bir CSP başlığı **mevcut değil**  

### `script-src` veya `default-src` direktifleri uygun şekilde kısıtlanmış mı?
- [ ] Evet — Direktifler katı beyaz listeye alma (whitelisting), nonce'lar veya hash'ler kullanıyor ve atlatma (bypass) **mümkün değil**  
- [ ] Evet — Direktifler **uygun şekilde yapılandırılmış** ancak beyaz liste, bilinen ve atlatılabilir CDN'leri içeriyor  
- [ ] Hayır — Direktifler joker karakterler (`*`) veya `data:` şemaları kullanıyor, bu da atlatmayı (bypass) **mümkün kılıyor**  

### Politika, güvenli olmayan inline scriptlerin veya eval fonksiyonunun çalıştırılmasına izin veriyor mu?
- [ ] Hayır — `'unsafe-inline'` ve `'unsafe-eval'` **mevcut değil**  
- [ ] Evet — `'unsafe-inline'` **mevcut** ancak bir `nonce` veya `hash` ile korunuyor  
- [ ] Evet — `'unsafe-inline'` veya `'unsafe-eval'` ek koruma olmaksızın **etkinleştirilmiş** *(Kritik)*  

### Uygulama, `frame-ancestors` direktifi aracılığıyla clickjacking'e karşı korunuyor mu?
- [ ] Evet — `frame-ancestors` değeri `'none'` veya `'self'` olarak ayarlanmış  
- [ ] Evet — `frame-ancestors` **etkinleştirilmiş** ancak belirli güvenilir üçüncü taraf domainlere izin veriyor  
- [ ] Hayır — `frame-ancestors` **eksik**; eski `X-Frame-Options` başlığına güveniliyor veya hiç koruma yok  

### CSP ihlallerini raporlamak için bir mekanizma var mı?
- [ ] Evet — `report-uri` veya `report-to` **yapılandırılmış** ve aktif  
- [ ] Hayır — İhlal raporlaması **devre dışı** veya **yapılandırılmamış**  

---