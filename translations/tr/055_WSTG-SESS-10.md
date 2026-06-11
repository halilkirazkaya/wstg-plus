## WSTG-SESS-10 — JSON Web Token'ların (JWT) Test Edilmesi

JSON Web Token'lar (JWT), durumluksuz (stateless) oturum yönetimi ve kimlik yayılımı için yaygın olarak kullanılır; ancak güvenlikleri tamamen imzalama algoritmalarının doğru uygulanmasına ve katı talep (claim) doğrulamasına bağlıdır. Saldırganlar sıklıkla "none" algoritması kusurlarını istismar ederek, zayıf HS256 gizli anahtarlarına Brute Force (kaba kuvvet) uygulayarak veya asimetrik bir genel anahtarın (public key) simetrik bir HMAC gizli anahtarı olarak kullanıldığı algoritma değiştirme (algorithm-switching) saldırıları gerçekleştirerek kimlik doğrulamayı atlatmaya çalışırlar. Bu zafiyetler tipik olarak oturum çerezlerinde (session cookies) veya Authorization başlıklarında (headers) kendini gösterir; bu da bir saldırganın, token'ın kriptografik bütünlüğünü bozmadan `role` veya `user_id` gibi talepleri değiştirerek yetki yükseltmesine (privilege escalation) veya herhangi bir kullanıcıyı taklit etmesine (impersonate) olanak tanır.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-SESS-10 |
| **CWE** | CWE-347 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Kritik |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/10-Testing_JSON_Web_Tokens  
* https://hacktricks.wiki/en/pentesting-web/hacking-jwt-json-web-tokens.html  
* https://portswigger.net/web-security/jwt  

**Araçlar:** `jwt_tool`, `Burp Suite (JWT Editor Extension)`, `hashcat`, `john`, `jose`

### "none" algoritması sunucu tarafından kabul ediliyor mu?
- [ ] Hayır — sunucu, başlıkta `alg: none` kullanan token'ları **reddediyor**  
- [ ] Evet — sunucu, `alg: none` ve boş bir imza içeren token'ları **kabul ediyor** *(Kritik)*  

### JWT imzası nasıl doğrulanıyor ve brute-force saldırılarına karşı nasıl korunuyor?
- [ ] Evet — güçlü asimetrik anahtarlar (RS256/ES256) veya yüksek entropili HMAC gizli anahtarları kullanılıyor  
- [ ] Evet — HS256 kullanılıyor ancak gizli anahtar, düşük entropi veya varsayılan değerler nedeniyle Brute Force ile **ele geçirilebilir**  
- [ ] Hayır — imza doğrulaması **uygulanmıyor** veya algoritma değiştirme (RS256'dan HS256'ya) yoluyla **atlatılabiliyor**  

### Standart talepler (exp, nbf, iat) arka uç tarafından sıkı bir şekilde denetleniyor mu?
- [ ] Evet — `exp` (son kullanma süresi) mevcut ve **atlatılamıyor**  
- [ ] Evet — `exp` mevcut ancak sunucu doğrulama sırasında bunu **zorunlu tutmuyor**  
- [ ] Hayır — son kullanma talepleri **eksik** veya görmezden geliniyor, bu da süresiz oturum tekrarına (session replay) izin veriyor  

### Uygulama, başlık tabanlı anahtar enjeksiyonunu (kid, jku, x5u) engelliyor mu?
- [ ] Evet — başlıklar temizleniyor ve yalnızca güvenilir dahili anahtarlara/kaynaklara **izin veriliyor**  
- [ ] Hayır — `kid` (Key ID) başlığı, bilinen bir dosyayı işaret etmek için dizin atlatma (directory traversal) veya SQL Injection saldırılarına karşı savunmasız  
- [ ] Hayır — `jku` veya `x5u` başlıkları, kötü amaçlı anahtarları yüklemek için saldırgan kontrolündeki URL'lere **yönlendirilebilir**  

---