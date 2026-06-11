## WSTG-IDNT-02 — Kullanıcı Kayıt Süreci Testi

Kullanıcı kayıt sürecinin test edilmesi, yeni kimliklerin oluşturulduğu iş akışının yetkisiz erişim veya otomatik istismar amacıyla suistimal edilmemesini sağlamak için değerlendirilmesini kapsar. Bu süreçteki zayıflıklar genellikle kimlik doğrulama (e-posta/SMS) eksikliği, botlar aracılığıyla toplu hesap oluşturmaya karşı hassasiyet veya mevcut kullanıcı adlarını açığa çıkaran ayrıntılı hata mesajları yoluyla bilgi ifşası (Information Disclosure) şeklinde ortaya çıkar. Saldırganlar, kullanıcı listeleme (User Enumeration) yapmak, büyük ölçekli spam kampanyaları yürütmek veya hesap oluşturma aşamasında yükseltilmiş yetkiler (Elevated Privileges) elde etmek amacıyla kayıt parametrelerini manipüle ederek bu açıkları istismar ederler. Bu test, uygulamanın bütünlüğünü korumak ve saldırganların sistemi sahte veya kötü niyetli hesaplarla doldurmasını önlemek için hayati önem taşır.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-IDNT-02 |
| **CWE** | CWE-836 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/02-Test_User_Registration_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite`, `OWASP ZAP`, `Python`, `ffuf`, `Cuppa`

### Yeni bir hesap aktif hale gelmeden önce kimlik doğrulaması gerekiyor mu?
- [ ] Evet — kayıt işlemi; e-posta veya SMS yoluyla gönderilen benzersiz, zaman kısıtlı bir Token gerektirir  
- [ ] Evet — doğrulama gereklidir ancak Tokenlar **zayıf** veya **tahmin edilebilir**dir  
- [ ] Hayır — hesaplar herhangi bir doğrulama adımı olmaksızın **anında** aktif edilir  

### Kayıt süreci kullanıcı listelemeyi (User Enumeration) engelliyor mu?
- [ ] Evet — uygulama hem yeni hem de mevcut e-posta/kullanıcı adları için **aynı** yanıtları döner  
- [ ] Hayır — hata mesajları (örneğin, "Email already in use") bir saldırganın kayıtlı kullanıcıları **tespit etmesine (map)** olanak tanır  
- [ ] Hayır — sunucu yanıtlarındaki zamanlama farkları (timing differences), bir kullanıcının mevcut olup olmadığını **açığa çıkarır**  

### Toplu kaydı önlemek için otomasyon karşıtı kontroller uygulanmış mı?
- [ ] Evet — CAPTCHA veya Proof-of-Work mekanizmaları **mevcuttur** ve **etkilidir**  
- [ ] Evet — kontroller mevcuttur ancak API uç noktaları veya Header manipülasyonu yoluyla atlatılması (bypass) **mümkündür**  
- [ ] Hayır — kayıt uç noktasına herhangi bir Rate Limiting veya CAPTCHA **uygulanmamıştır**  

### Kayıt parametreleri yetki yükseltmek (Privilege Escalation) için manipüle edilebiliyor mu?
- [ ] Hayır — kullanıcı rolleri **kesinlikle** sunucu tarafındaki mantık tarafından atanır  
- [ ] Evet — daha yüksek erişim elde etmek için istekteki `role`, `admin` veya `group_id` gibi parametreler **değiştirilebilir**  

### Kayıt aşamasında parola politikası (Password Policy) zorunlu tutuluyor mu?
- [ ] Evet — güçlü parola gereksinimleri sunucu tarafında **zorunlu kılınmıştır (enforced)**  
- [ ] Hayır — zayıf parolalar (örneğin, "password123") sistem tarafından **kabul edilir**  

---