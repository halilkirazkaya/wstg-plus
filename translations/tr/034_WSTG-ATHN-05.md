## WSTG-ATHN-05 — Güvenlik Açıklı Beni Hatırla Fonksiyonunun Test Edilmesi

"Beni Hatırla" (Remember Me) veya kalıcı oturum açma (persistent login) fonksiyonu, bir token veya kimlik bilgisini istemci tarafında saklayarak kullanıcının kimlik doğrulaması yapılmış durumunu tarayıcı yeniden başlatılsa dahi korumak üzere tasarlanmıştır. Eğer bu özellik; açık metin (plaintext) parolaların, zayıf özetlerin (hashes) veya düşük entropili tokenların çerezlerde (cookies) saklanması gibi hatalı şekillerde yapılandırılmışsa; kullanıcıyı Cross-Site Scripting (XSS), fiziksel erişim veya ağ trafiğine müdahale edilmesi yoluyla hesap ele geçirme (account takeover) riskine maruz bırakır. Sızma testi uzmanları (pentesters), kalıcı erişim kolaylığının; Çok Faktörlü Kimlik Doğrulama (MFA) veya oturum sonlandırma gibi kritik güvenlik kontrollerini devre dışı bırakmadığından emin olmak için bu kalıcı tokenların entropisini, depolama mekanizmasına uygulanan güvenlik bayraklarını (security flags) ve oturum yaşam döngüsünü değerlendirir. İstismar süreci genellikle, orijinal kimlik bilgilerine ihtiyaç duymadan hesaplara yetkisiz erişim sağlamak için bu uzun ömürlü tokenların ele geçirilmesini içerir.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-ATHN-05 |
| **CWE** | CWE-522 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek* |

> *Kalıcı çerezde açık metin kimlik bilgileri veya tuzlanmamış (unsalted) özetler saklanıyorsa veya token Çok Faktörlü Kimlik Doğrulamayı (MFA) atlatıyorsa önem derecesi Yüksek olur.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/05-Testing_for_Vulnerable_Remember_Password  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  
* https://portswigger.net/web-security/authentication  

**Araçlar:** `Burp Suite (Cookie Editor/Sequencer)`, `Browser Developer Tools`, `EditThisCookie`

### Uygulama bir "Beni Hatırla" veya "Oturumu Açık Tut" özelliğini uyguluyor mu?
- [ ] Hayır — uygulama içinde bu özellik **bulunmamaktadır**  
- [ ] Evet — özellik mevcut ve kriptografik olarak güçlü, tahmin edilemez tokenlar kullanıyor  
- [ ] Evet — özellik mevcut ancak tahmin edilebilir veya düşük entropili tokenlar kullanıyor *(Orta)*  
- [ ] Evet — özellik mevcut ve açık metin kimlik bilgilerini veya base64 ile kodlanmış parolaları saklıyor *(Yüksek)*  

### Güvenlik bayrakları kalıcı çereze doğru şekilde uygulanmış mı?
- [ ] Evet — `HttpOnly`, `Secure` ve `SameSite` bayrakları **uygulanmıştır**  
- [ ] Evet — bazı bayraklar uygulanmış ancak `HttpOnly` **eksik**  
- [ ] Evet — bazı bayraklar uygulanmış ancak HTTPS üzerinden `Secure` **eksik**  
- [ ] Hayır — kalıcı çereze hiçbir güvenlik bayrağı **uygulanmamıştır**  

### "Beni Hatırla" tokenı, manuel çıkış işleminden sonra geçerli kalmaya devam ediyor mu?
- [ ] Hayır — token, çıkış yapıldığı anda sunucu tarafında geçersiz kılınmaktadır  
- [ ] Evet — token geçerli kalıyor ancak hassas işlemler için parola gerektiriyor  
- [ ] Evet — token geçerli kalıyor ve yeniden kimlik doğrulaması **gerekmeden** tam hesap erişimine izin veriyor *(Orta)*  

### Parola değişikliği yapıldığında kalıcı oturum sonlandırılıyor mu?
- [ ] Evet — kullanıcı parolasını değiştirdiğinde tüm kalıcı tokenlar iptal edilmektedir  
- [ ] Hayır — parola sıfırlama/değiştirme işlemi **gerçekleştirildikten sonra** "Beni Hatırla" tokenı geçerli kalmaya devam ediyor *(Yüksek)*  

### Kalıcı token, sonraki ziyaretlerde Çok Faktörlü Kimlik Doğrulamayı (MFA) atlatıyor mu?
- [ ] Hayır — bir "Beni Hatırla" tokenı olsa dahi MFA gerekmektedir  
- [ ] Evet — MFA atlatılıyor ancak token belirli bir cihaz parmak izine (device fingerprint) bağlıdır  
- [ ] Evet — herhangi bir cihazdan sadece kalıcı çerez kullanılarak MFA **tamamen atlatılıyor** *(Yüksek)*  

---