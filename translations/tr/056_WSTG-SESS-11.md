## WSTG-SESS-11 — Eşzamanlı Oturumların (Concurrent Sessions) Test Edilmesi

Eşzamanlı oturum testi (Concurrent session testing), bir uygulamanın tek bir kullanıcı hesabının farklı tarayıcılar, cihazlar veya IP adresleri üzerinden aynı anda birden fazla aktif oturum sürdürmesine izin verip vermediğini değerlendirir. Eşzamanlı oturumların sınırlandırılmaması, saldırganların ele geçirilmiş oturum token'larını (session tokens) veya tehlikeye atılmış kimlik bilgilerini, asıl kullanıcıyı uyarmadan veya mevcut oturumları sonlandırmadan kullanma fırsatını artırır. Bu davranış genellikle birden fazla kaynaktan kimlik doğrulaması yapılarak ve oturum kararlılığı izlenerek değerlendirilir; bu durum genellikle oturum yönetimi (session management) mantığındaki zayıflıkları veya güvenlik odaklı iş kurallarının eksikliğini ortaya çıkarır. Bir saldırganın bakış açısıyla, eşzamanlılık kontrolünün eksikliği, ilk sızmadan sonra gizli bir kalıcılık (persistence) sağlar ve paralel otomatik saldırıları kolaylaştırır.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-SESS-11 |
| **CWE** | CWE-613 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Şiddet** | Düşük / Orta |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/11-Testing_for_Concurrent_Sessions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite (Repeater/Containers)`, `Firefox Multi-Account Containers`, `Postman`, `cURL`

### Eşzamanlı oturumlar uygulama politikası tarafından sınırlandırılmış mı?
- [ ] Evet — kullanıcı başına yalnızca bir oturuma **izin verilir**; yeni girişler eskileri geçersiz kılar *(En güvenli)*  
- [ ] Evet — belirli bir sayıda eşzamanlı oturum **zorunlu tutulur** (enforced) ve bu sayı aşılamaz  
- [ ] Hayır — sınırsız sayıda eşzamanlı oturum **mümkündür**  

### Oturum sınırına ulaşıldığında uygulama nasıl tepki verir?
- [ ] En eski aktif oturum **otomatik olarak sonlandırılır** (session fixation/hijacking azaltma)  
- [ ] Yeni oturum yetkilendirilmeden önce kullanıcıdan mevcut oturumları sonlandırması **istenir**  
- [ ] Başka bir cihazdan manuel çıkış yapılana kadar yeni giriş denemesi **engellenir**  
- [ ] Hiçbir işlem yapılmaz — birden fazla oturum aynı anda **etkin** kalmaya devam eder  

### Uygulama, kullanıcı için bir oturum yönetimi arayüzü sağlıyor mu?
- [ ] Evet — kullanıcılar aktif oturumları (IP, cihaz, zaman) **görüntüleyebilir** ve bunları uzaktan sonlandırabilir  
- [ ] Evet — kullanıcılar aktif oturumları **görüntüleyebilir** ancak bunları uzaktan sonlandıramaz  
- [ ] Hayır — kullanıcılar, hesaplarıyla ilişkili diğer aktif oturumları **göremez** veya yönetemez  

### Eşzamanlı giriş faaliyeti tespit edildiğinde kullanıcı bilgilendiriliyor mu?
- [ ] Evet — yeni cihazlardan veya konumlardan yapılan girişler için bir uyarı (e-posta, SMS veya uygulama içi) **tetiklenir**  
- [ ] Hayır — eşzamanlı oturumlar oluşturulduğunda herhangi bir bildirim **sağlanmaz**  

---