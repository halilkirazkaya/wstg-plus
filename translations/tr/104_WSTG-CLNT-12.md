## WSTG-CLNT-12 — Tarayıcı Depolama Alanının Test Edilmesi

Tarayıcı depolamasını test etmek, bir uygulamanın verileri kalıcı hale getirmek için LocalStorage, SessionStorage, IndexedDB ve WebSQL gibi istemci tarafı mekanizmalarını nasıl kullandığının analiz edilmesini içerir. Güvensiz şekilde depolanan hassas bilgiler — Kişisel Veriler (Personally Identifiable Information (PII)), kimlik doğrulama belirteçleri (tokens) veya oturum tanımlayıcıları dahil — bir saldırganın Siteler Arası Betik Çalıştırma (Cross-Site Scripting (XSS)) gerçekleştirmesi veya kullanıcının cihazına fiziksel erişim sağlaması durumunda ele geçirilebilir. Sızma testi uzmanları (Pentesters), verilerin açık metin (cleartext) olarak saklanıp saklanmadığını, çıkış yapıldıktan sonra erişilebilir kalıp kalmadığını ve veri sızıntısını önlemek için depolama yaşam döngüsünün uygun şekilde yönetilip yönetilmediğini belirlemelidir. Bir saldırganın bakış açısından bu depolama mekanizmaları, oturum çalma (session hijacking) veya uzun süreli hesap kalıcılığı sağlayan durum koruma bilgilerini sızdırmak (exfiltrating) için kazançlı hedeflerdir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CLNT-12 |
| **CWE** | CWE-922 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek* |

> *Oturum belirteçleri (tokens) veya hassas PII verilerinin LocalStorage'da saklanması ve XSS yoluyla erişilebilir olması durumunda Önem Derecesi Yüksek (High) olarak kabul edilir.

**Kaynaklar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/12-Testing_Browser_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Browser DevTools`, `Storage Explorer`, `Burp Suite`, `OWASP ZAP`

### Uygulama, tarayıcı depolama alanında hassas bilgi saklıyor mu?
- [ ] Hayır — uygulama tarayıcı depolamasını kullanmıyor veya yalnızca hassas olmayan kullanıcı arayüzü (UI) durumlarını saklıyor  
- [ ] Evet — hassas veriler saklanıyor ancak güçlü istemci tarafı kriptografi kullanılarak şifreleniyor  
- [ ] Evet — hassas veriler (tokens, PII, sırlar) açık metin (cleartext) olarak saklanıyor *(Orta)*  

### Depolanan verilere yetkisiz betikler tarafından erişilebilir mi (XSS riski)?
- [ ] Hayır — veriler HttpOnly çerezlerinde saklanıyor (tarayıcı depolamasında değil) veya depolama alanı kullanılmıyor  
- [ ] Evet — LocalStorage/SessionStorage kullanılıyor, bu da verileri herhangi bir XSS Payload aracılığıyla tamamen erişilebilir kılıyor *(Yüksek)*  

### Kullanıcı çıkışı yapıldığında tarayıcı depolama alanı uygun şekilde temizleniyor mu?
- [ ] Evet — uygulamayla ilgili tüm depolama alanları çıkış işlemi sırasında açıkça temizleniyor  
- [ ] Hayır — depolama alanı çıkıştan sonra da kalıyor ancak hassas oturum verileri içermiyor  
- [ ] Hayır — kimlik doğrulama belirteçleri veya PII, oturum sonlandırıldıktan sonra depolama alanında kalmaya devam ediyor *(Orta)*  

### Uygulama, büyük/hassas veri setleri için IndexedDB veya WebSQL kullanıyor mu?
- [ ] Hayır — bu depolama mekanizmaları kullanılmıyor  
- [ ] Evet — veriler saklanıyor ve DOM içinde işlenmeden önce uygun şekilde sanitize ediliyor  
- [ ] Evet — hassas veri setleri IndexedDB/WebSQL yapılarında açık metin (cleartext) olarak saklanıyor  

### Depolanan verilerin bütünlüğü, uygulama mantığını etkileyecek şekilde manipüle edilebilir mi?
- [ ] Hayır — uygulama, depolama alanından gelen verileri işlemeden önce doğrular veya imzalar  
- [ ] Evet — depolama değerlerini (örneğin kullanıcı rolleri, bayraklar) değiştirmek mümkündür ve uygulama davranışının değişmesine neden olur  

---