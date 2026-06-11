## WSTG-CLNT-04 — İstemci Taraflı URL Yönlendirmesi Testi (Testing for Client-Side URL Redirect)

İstemci taraflı URL yönlendirmesi, bir web uygulamasının genellikle URL parametreleri veya hash fragmanları aracılığıyla kullanıcı kontrollü girdi kabul etmesi ve bu girdiyi `window.location` veya `document.location.href` gibi istemci taraflı bir yönlendirme havuzunda (client-side redirection sink) kullanmasıyla oluşur. Bu zafiyet, kurbanın başlangıçta meşru alana güvenmesi ve ardından sessizce kötü niyetli bir siteye yönlendirilmesi nedeniyle, saldırganlar tarafından karmaşık oltalama (phishing) kampanyaları yürütmek için öncelikli olarak kullanılır. Oltalama saldırılarının ötesinde, istemci taraflı yönlendirmeler; yönlendirme sırasında URL'de veya `Referer` başlığında OAuth erişim belirteçleri (OAuth access tokens) veya oturum kimlikleyicileri (session identifiers) gibi hassas veriler bulunuyorsa, bu verilerin sızdırılması için zincirlenebilir. Modern Tek Sayfalı Uygulamalarda (Single Page Applications - SPA), bu zafiyet sıklıkla yönlendirme mantığında (routing logic), kimlik doğrulama sonrası "geri dönüş" (return to) parametrelerinde veya dil değiştirme işlevlerinde görülür.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CLNT-04 |
| **CWE** | CWE-601 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta* |

> *Yönlendirme; OAuth belirteçlerini, oturum kimliklerini (session IDs) sızdırmak veya bir kimlik doğrulama akışındaki güvenlik kontrollerini atlamak için kullanılabiliyorsa önem derecesi Yüksek olur.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/04-Testing_for_Client-side_URL_Redirect  
* https://hacktricks.wiki/en/pentesting-web/open-redirect.html  

**Araçlar:** `Burp Suite (DOM Invader)`, `Browser DevTools`, `LinkFinder`, `Arjun`, `B-Config`

### Uygulama, kullanıcı girdisine dayalı yönlendirmeleri işlemek için istemci taraflı betikler kullanıyor mu?
- [ ] Hayır — yönlendirme, sunucu tarafında yalnızca `3xx` HTTP durum kodları aracılığıyla yönetilir  
- [ ] Evet — uygulama, URL parametrelerine dayalı gezinme için `window.location` gibi JavaScript havuzlarını (sinks) kullanır  
- [ ] Evet — uygulama, kullanıcı tarafından sağlanan yolları işleyen bir istemci taraflı uygulama çatısı yönlendiricisi (örneğin React Router, Vue Router) kullanır  

### Hedef URL için girdi doğrulama veya beyaz listeye alma (whitelisting) kontrolleri uygulanmış mı?
- [ ] Evet — izin verilen alan adları/yollardan oluşan katı bir **beyaz liste** (whitelist) zorunludur ve atlatılması **mümkün değildir**  
- [ ] Evet — doğrulama mevcuttur ancak zayıf regex veya kara listelere (blacklists) dayanmaktadır ve atlatılması **mümkündür**  
- [ ] Hayır — uygulama, yönlendirme hedefi olarak herhangi bir rastgele dizgiyi kabul eder  

### Yönlendirme mantığı yaygın gizleme (obfuscation) teknikleri kullanılarak atlatılabilir mi?
- [ ] Hayır — kontroller; kodlanmış karakterleri, protokolden bağımsız URL'leri (`//`) ve ters eğik çizgileri (backslashes) doğru şekilde işler  
- [ ] Evet — URL kodlama (URL encoding) veya çift kodlama (double-encoding) kullanılarak atlatma **mümkündür**  
- [ ] Evet — protokolden bağımsız URL'ler veya `@` karakterleri (örneğin `https://expected.com@attacker.com`) kullanılarak atlatma **mümkündür**  
- [ ] Evet — belirli tarayıcı tuhaflıkları (quirks) veya null byte enjeksiyonu kullanılarak atlatma **mümkündür**  

### Yönlendirme işlemi hedef siteye hassas veri ifşa ediyor mu?
- [ ] Hayır — URL'de hassas veri bulunmamaktadır veya yönlendirme sırasında başlıklar (headers) aracılığıyla iletilmemektedir  
- [ ] Evet — hassas veriler (örneğin oturum belirteçleri, API anahtarları) `Referer` başlığı aracılığıyla harici siteye **sızdırılmaktadır**  
- [ ] Evet — URL fragmanında (`#`) bulunan hassas verilere hedef sitenin betikleri tarafından **erişilebilmektedir**  

### Yönlendirme havuzu (sink) aracılığıyla JavaScript çalıştırmak mümkün mü (DOM tabanlı XSS)?
- [ ] Hayır — havuz yalnızca `http` veya `https` protokollerine geçişe izin verir  
- [ ] Evet — yönlendirme havuzu `javascript:` veya `data:` URI'larına izin vererek DOM tabanlı XSS'i (DOM-based XSS) **mümkün kılar**  

---