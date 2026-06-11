## WSTG-ATHN-03 — Zayıf Hesap Kilitleme Mekanizmasının Test Edilmesi (Testing for Weak Lock Out Mechanism)

Hesap kilitleme mekanizması (Account Lockout Mechanism), belirli sayıda başarısız kimlik doğrulama denemesinden sonra hesabı devre dışı bırakarak brute-force (kaba kuvvet) ve credential stuffing (kimlik bilgisi doldurma) saldırılarını azaltmak için tasarlanmış kritik bir derinlemesine savunma (defense-in-depth) kontrolüdür. Sağlam bir kilitleme politikası olmadan saldırganlar, birden fazla hesap genelinde sistematik olarak şifre tahmin etmek (password spraying) veya başarılı olana kadar yüksek değerli tek bir hesabı hedeflemek için otomatik araçlar kullanabilirler. Bu zayıflık genellikle giriş portallarında, şifre sıfırlama formlarında ve Rate Limiting (hız sınırlaması) veya hesap düzeyinde koruması bulunmayan API uç noktalarında (endpoints) ortaya çıkar. Bir saldırganın perspektifinden, zayıf veya eksik bir kilitleme mekanizması sonsuz şifre denemesine izin verirken; aşırı agresif bir mekanizma ise meşru kullanıcıların hesaplarını kasıtlı olarak kilitleyerek onlara karşı bir dağıtık Denial of Service (DoS - Hizmet Reddi) saldırısı gerçekleştirmek için kötüye kullanılabilir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-ATHN-03 |
| **CWE** | CWE-307 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek* |

> *Uygulama hassas PII (Kişisel Veri), finansal veri işliyorsa veya yönetici hesapları herhangi bir ikincil kontrol olmaksızın Brute Force saldırılarına karşı savunmasızsa Önem Derecesi Yüksek olur.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/03-Testing_for_Weak_Lock_Out_Mechanism  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Araçlar:** `Burp Suite Intruder`, `THC-Hydra`, `ffuf`, `Patator`, `wfuzz`

### Belirlenmiş sayıda başarısız denemeden sonra bir hesap kilitleme mekanizması uygulanıyor mu?
- [ ] Evet — hesap tanımlanmış, makul bir eşikten sonra (örneğin 5-10 deneme) kilitlenir  
- [ ] Evet — hesap kilitlenir ancak yalnızca aşırı yüksek sayıda denemeden sonra  
- [ ] Hayır — hesap **kilitlenemez** ve süresiz Brute Force denemesine izin verir  

### Kilitleme mekanizması yaygın atlatma teknikleri kullanılarak baypas edilebiliyor mu?
- [ ] Hayır — kilitleme sunucu tarafında (server-side) uygulanır ve istemci tarafındaki değişikliklerden **etkilenmez**  
- [ ] Evet — IP adreslerini döndürerek veya bir proxy havuzu kullanarak kilitlemeyi baypas etmek **mümkündür**  
- [ ] Evet — `X-Forwarded-For` veya `User-Agent` gibi HTTP başlıklarını manipüle ederek kilitlemeyi baypas etmek **mümkündür**  
- [ ] Evet — çerezleri (cookies) temizleyerek veya yeni oturumlar başlatarak kilitlemeyi baypas etmek **mümkündür**  

### Kilitleme mekanizması hesap kurtarma veya süre dolumu için bir yol sağlıyor mu?
- [ ] Evet — hesap belirli bir süre sonra otomatik olarak veya e-posta doğrulaması yoluyla açılır  
- [ ] Evet — hesabın açılması için yönetici müdahalesi gerekir  
- [ ] Hayır — kilitleme kalıcıdır ve net bir kurtarma yolu yoktur, bu da DoS riskini artırır  

### Uygulama, kilitleme sırasında geçerli ve geçersiz kullanıcı adları arasında ayrım yapıyor mu?
- [ ] Hayır — uygulama yanıtı hem mevcut olan hem de mevcut olmayan kullanıcılar için aynıdır  
- [ ] Evet — uygulama bir hesabın kilitli olup olmadığını ifşa eder, bu da **username enumeration** (kullanıcı adı listeleme) saldırısına olanak tanır  

### Kilitleme mekanizması bir Denial of Service (DoS) saldırısına karşı hassas mı?
- [ ] Hayır — CAPTCHA veya IP tabanlı Rate Limiting gibi kontroller toplu hesap kilitlenmesini engeller  
- [ ] Evet — bir saldırgan, yanlış şifreler sağlayarak bilinen tüm kullanıcıları sistematik olarak **kilitleyebilir**  

---