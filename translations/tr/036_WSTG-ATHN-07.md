## WSTG-ATHN-07 — Zayıf Parola Politikası Testi

Zayıf parola politikalarının test edilmesi, bir uygulama tarafından hesap oluşturma ve kimlik bilgisi güncellemeleri sırasında zorunlu kılınan karmaşıklık, uzunluk ve entropi gereksinimlerinin değerlendirilmesini içerir. Yetersiz politikalar; kullanıcı hesaplarını otomatik sözlük saldırılarına (dictionary attacks), Brute Force girişimlerine ve kimlik bilgisi doldurma (credential stuffing) saldırılarına karşı savunmasız bırakarak gizliliğini doğrudan tehlikeye atar. Bu zafiyetler genellikle kayıt uç noktalarında (endpoints), parola sıfırlama iş akışlarında ve idari kullanıcı yönetimi panellerinde bulunur. Bir saldırganın perspektifinden bakıldığında, esnek bir politika, yaygın wordlist'ler veya kalıp tabanlı kırma yöntemleri kullanılarak kimlik bilgilerinin başarılı bir şekilde ele geçirilmesi için gereken hesaplama maliyetini ve süresini önemli ölçüde azaltır.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-ATHN-07 |
| **CWE** | CWE-521 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Düşük* |

> *Uygulama, otomatik tahminleri önlemek için hesap kilitleme (account lockout) veya Rate Limiting mekanizmalarından yoksunsa önem derecesi Yüksek (High) olur.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/07-Testing_for_Weak_Authentication_Methods  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**Araçlar:** `Burp Suite (Intruder)`, `ZAP (Fuzzer)`, `Hydra`, `Hashcat`, `Cewl`

### Uygulama tarafından minimum parola uzunluğu zorunlu tutuluyor mu?
- [ ] Evet — minimum uzunluk 12+ karakterdir *(En güvenli)*  
- [ ] Evet — minimum uzunluk 8 ile 11 karakter arasındadır  
- [ ] Evet — minimum uzunluk 8 karakterden **azdır**  
- [ ] Hayır — herhangi bir minimum uzunluk zorunluluğu yoktur  

### Karmaşıklık gereksinimleri (büyük harf, küçük harf, rakam, sembol) zorunlu tutuluyor mu?
- [ ] Evet — birden fazla karakter sınıfı zorunludur ve atlatma (bypass) **mümkün değildir**  
- [ ] Evet — karakter sınıfları önerilir ancak zorunlu **değildir**  
- [ ] Hayır — hiçbir karmaşıklık gereksinimi uygulanmamaktadır  

### Uygulama, yaygın/zayıf parolaları veya kullanıcıya özgü verileri içeren parolaları engelliyor mu?
- [ ] Evet — bilinen zayıf parolalar (örn. "password123") ve kullanıcı adı tabanlı parolalar **kullanılamaz**  
- [ ] Evet — yaygın parolalar engellenir, ancak kullanıcıya özgü veriler (örn. kullanıcı adı, e-posta) **kullanılabilir**  
- [ ] Hayır — uzunluk/karmaşıklık gereksinimlerini karşılayan her türlü karakter dizisi kabul edilir  

### Karmaşıklık veya uzunluk gereksinimleri istemci tarafı (client-side) modifikasyonu ile atlatılabilir mi?
- [ ] Hayır — politikalar **sunucu tarafında (server-side)** sıkı bir şekilde uygulanmaktadır  
- [ ] Evet — politikalar JavaScript aracılığıyla uygulanmaktadır ve isteğin araya girilerek yakalanması (intercept) ile **atlatılabilir**  

### Parola politikası, uygulama detaylarını sızdırmadan kullanıcıya net bir şekilde iletiliyor mu?
- [ ] Evet — politika giriş sırasında görünürdür ve genel (generic) hata mesajları sağlanır  
- [ ] Evet — politika görünürdür ancak hata mesajları belirli regex veya mantık açıklarını ortaya çıkarır  
- [ ] Hayır — politika görünür **değildir**, bu da deneme yanılma yoluyla keşfetmeyi zorunlu kılar  

---