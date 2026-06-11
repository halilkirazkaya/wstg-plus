## WSTG-SESS-09 — Oturum Ele Geçirme (Session Hijacking) Testi

Oturum ele geçirme (Session Hijacking), bir saldırganın kullanıcının aktif oturumuna yetkisiz erişim sağlamak amacıyla geçerli bir oturum tanımlayıcısını (SID) ele geçirmesi, tahmin etmesi veya sabitlemesi durumunda meydana gelir. Bu zafiyet genellikle yetersiz taşıma katmanı güvenliği, çerez (cookie) güvenlik bayraklarının eksikliği veya bir saldırganın kimlik doğrulamayı tamamen atlamasına izin veren tahmin edilebilir SID oluşturma algoritmaları aracılığıyla kendini gösterir. Bir saldırganın perspektifinden, başarılı bir istismar (exploit), kurbanın kimlik bilgilerine ihtiyaç duymadan kurbanın kimliğine tam olarak bürünülmesini sağlar; bu durum genellikle ağ dinleme (network sniffing), Cross-Site Scripting (XSS) veya oturum sabitleme (session fixation) saldırıları yoluyla gerçekleştirilir.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-SESS-09 |
| **CWE** | CWE-287 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Kritik |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/09-Testing_for_Session_Hijacking  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Araçlar:** `Burp Suite`, `OWASP ZAP`, `EditThisCookie`, `Wireshark`, `Bettercap`

### Oturum tanımlayıcıları iletim sırasında korunuyor mu?
- [ ] Evet — `Secure` bayrağı **etkinleştirilmiş** ve HSTS sıkı bir şekilde uygulanıyor  
- [ ] Evet — `Secure` bayrağı **etkinleştirilmiş** ancak HSTS **devre dışı**  
- [ ] Hayır — `Secure` bayrağı **uygulanmamış**, bu da SID'nin şifrelenmemiş kanallar üzerinden ele geçirilmesine (interception) olanak tanıyor *(Yüksek)*  

### Oturum tanımlayıcısı istemci tarafı betik (script) erişimine karşı korunuyor mu?
- [ ] Evet — Oturumla ilgili tüm çerezlere `HttpOnly` bayrağı **uygulanmış**  
- [ ] Hayır — `HttpOnly` bayrağı **uygulanmamış**, bu da XSS yoluyla SID sızdırılmasına (exfiltration) olanak tanıyor *(Kritik)*  

### Uygulama, istemci özniteliklerine oturum bağlama (session binding) uyguluyor mu?
- [ ] Evet — Oturum, istemci özniteliklerine (IP/User-Agent) bağlı ve yeniden kullanım **mümkün değil**  
- [ ] Evet — Oturum bağlama mevcut ancak başlık sahteciliği (header spoofing) yoluyla atlatılması **mümkün**  
- [ ] Hayır — Oturum bağlama mevcut değil; SID herhangi bir kaynaktan kullanıldığında **geçerlidir**  

### Oturum tanımlayıcısı, tahmini engellemek için yeterince rastgele mi?
- [ ] Evet — SID, kriptografik olarak güvenli sahte rastgele sayı üreteci (CSPRNG) kullanıyor  
- [ ] Hayır — SID uzunluğu yetersiz veya **tahmin edilebilir** bir dizi/örüntü izliyor  

### Saldırı penceresini sınırlamak için eş zamanlı oturumlar yönetiliyor mu?
- [ ] Hayır — Uygulama, kullanıcı başına kesinlikle tek bir aktif oturumu zorunlu kılıyor  
- [ ] Evet — Birden fazla eş zamanlı oturum **etkinleştirilmiş** ancak izleniyor  
- [ ] Evet — Farklı cihazlar/konumlar arasında sınırsız eş zamanlı oturum **mümkün**  

---