## WSTG-CLNT-11 — Web Mesajlaşma Testi (Test Web Messaging)

Web Mesajlaşma veya `postMessage`, bir üst sayfa ile açılan bir pencere (spawned popup) veya gömülü bir iframe gibi pencere nesneleri (window objects) arasında kökenler arası iletişime (cross-origin communication) olanak tanır. Bu mekanizma, modern web işlevselliği için kritiktir; ancak alıcı uygulama gönderenin kimliğini doğrulamada başarısız olursa veya mesaj yükünü (payload) hatalı işlerse önemli riskler doğurur. Saldırganlar, hedef uygulamayı gömen kötü niyetli bir site barındırarak ve istenmeyen eylemleri tetiklemek, hassas verileri sızdırmak (exfiltrate) veya DOM tabanlı Siteler Arası Betik Çalıştırma (Cross-Site Scripting - XSS) saldırılarını gerçekleştirmek için özel yapılandırılmış mesajlar (crafted messages) göndererek bu güvenlik açıklarını istismar ederler. Başarılı bir istismar, mesaj dinleyicileri (message listeners) `origin` özelliğini sıkı bir şekilde doğrulamadığında veya `message.data` içeriğini doğrudan tehlikeli yürütme havuzlarına (execution sinks) aktardığında gerçekleşir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CLNT-11 |
| **CWE** | CWE-345 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek |

**Kaynaklar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/11-Testing_Web_Messaging  
* https://hacktricks.wiki/en/pentesting-web/postmessage-vulnerabilities/index.html  

**Araçlar:** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `PostMessage-Tracker`, `PMHook`

### Uygulama, dokümanlar arası iletişim için `postMessage` API'sini kullanıyor mu?
- [ ] Hayır — İstemci tarafı kodunda `postMessage` dinleyicileri mevcut **değildir**  
- [ ] Evet — `postMessage`, dahili bileşen iletişimi için kullanılmaktadır  
- [ ] Evet — `postMessage`, üçüncü taraf alan adları veya widget'larla iletişim kurmak için kullanılmaktadır  

### Gelen mesajların kaynağı (origin) sıkı bir şekilde doğrulanıyor mu?
- [ ] Evet — Kaynak, `===` veya tam eşleşme karşılaştırması kullanılarak sıkı bir beyaz listeye (whitelist) göre kontrol edilmektedir *(En güvenli yöntem)*  
- [ ] Evet — Kaynak, bir regex (düzenli ifade) kullanılarak doğrulanmaktadır ancak mantık atlatılmaya (bypass) müsaittir (örneğin, `^https://trusted.com`)  
- [ ] Hayır — `origin` özelliği kontrol edilmemekte ve herhangi bir alan adının mesaj göndermesine izin verilmektedir  
- [ ] Hayır — Gönderenin `targetOrigin` kısmında bir joker karakter `*` kullanılmaktadır ve bu durum verilerin kötü niyetli dinleyicilere sızmasına neden olabilir  

### Mesaj yükü (payload), uygulama tarafından işlenmeden önce temizleniyor (sanitized) mu?
- [ ] Evet — Veriler düz metin (plain text) olarak ele alınmaktadır ve hiçbir tehlikeli havuz (sink) kullanılmamaktadır  
- [ ] Evet — Veriler kullanılmadan önce ayrıştırılmakta (örneğin, `JSON.parse`) ve doğrulanmaktadır; atlatma (bypass) **mümkün değildir**  
- [ ] Hayır — Mesaj verileri doğrudan `innerHTML`, `eval()` veya `setTimeout()` gibi tehlikeli havuzlara aktarılmaktadır  
- [ ] Hayır — Mesaj verileri, doğrulama yapılmadan pencereyi yönlendirmek veya `location.href` değerini değiştirmek için kullanılmaktadır  

### Bir saldırgan, kötü niyetli bir mesaj yoluyla yetkisiz eylemleri tetikleyebilir mi veya veri sızdırabilir mi?
- [ ] Hayır — Sahte bir mesajla bile hassas eylemlere veya verilere erişilememektedir  
- [ ] Evet — Özel yapılandırılmış bir mesaj, hassas durum değişikliklerini tetikleyebilir (örneğin, parola değiştirme, profil güncelleme)  
- [ ] Evet — Özel yapılandırılmış bir mesaj, DOM tabanlı XSS ile sonuçlanabilir veya CSRF token'larının/oturum verilerinin sızdırılmasına yol açabilir  

---