## WSTG-SESS-08 — Session Puzzling

Oturum Değişkeni Aşırı Yükleme (Session Variable Overloading) olarak da bilinen Session Puzzling, bir uygulamanın aynı oturum değişkenini farklı modüllerde birden fazla amaç için kullanması veya iş akışı geçişleri sırasında oturum durumlarını uygun şekilde temizleyememesi durumunda ortaya çıkar. Saldırganlar, kimlik doğrulaması yapılmamış bir profil önizlemesi veya çok adımlı bir kayıt işlemi gibi bir bağlamda oturum değişkenlerini doldurarak ve ardından uygulamanın bu önceden var olan değişkenlere yanlışlıkla güvendiği korumalı bir alana giderek bu davranıştan yararlanırlar. Bu durum; kimlik doğrulama atlatma (Authentication Bypass), yetki yükseltme (Privilege Escalation) veya uygulamanın bir oturumun gerçekte olduğundan daha yüksek yetkili bir durumda olduğuna ikna edilmesiyle hassas verilere yetkisiz erişim gibi kritik sonuçlara yol açabilir. Bu zafiyet, oturum yönetimi mantığının farklı fonksiyonel bileşenler arasında tutarsız bir şekilde uygulandığı karmaşık ve durumlu (stateful) uygulamalarda oldukça yaygındır.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-SESS-08 |
| **CWE** | CWE-621 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Şiddet** | Yüksek / Kritik |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/08-Testing_for_Session_Puzzling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite (Repeater/Comparator)`, `OWASP ZAP`, `Postman`, `Cookie Editor`

### Uygulama, ayrı modüller arasında farklı amaçlar için aynı oturum değişken isimlerini kullanıyor mu?
- [ ] Hayır — değişken isimleri benzersizdir veya kapsamlar (scopes) izole edilmiştir  
- [ ] Evet — ortak değişken isimleri mevcuttur ancak modüller arasında durum temizlenmektedir  
- [ ] Evet — ortak değişken isimleri mevcuttur ve durum **temizlenmemektedir**  

### Kimlik doğrulaması yapılmamış eylemler, kimlik doğrulaması yapılmış bağlamlarda kullanılan oturum değişkenlerini etkileyebilir mi?
- [ ] Hayır — oturum değişkenleri yalnızca başarılı bir kimlik doğrulamadan **sonra** başlatılır  
- [ ] Evet — oturum değişkenleri girişten önce ayarlanabilir ancak giriş sonrası durumu **etkilemez**  
- [ ] Evet — kimlik doğrulama öncesinde ayarlanan oturum değişkenlerine kimlik doğrulama sonrasında **güvenilmektedir** *(Kritik)*  

### Uygulama, yetki seviyesi değişikliğinde oturum tanımlayıcısını (session identifier) ve onunla ilişkili değişkenleri yeniden başlatıyor mu?
- [ ] Evet — oturum tamamen sıfırlanır ve yeni bir ID **atanır**  
- [ ] Kısmen — yeni ID atanır ancak bazı oturum değişkenleri **varlığını sürdürür**  
- [ ] Hayır — oturum ID'si ve tüm değişkenler değişmeden **kalır**  

### Yetkisiz bir hesap üzerinden oturum değişkenleri manipüle edilerek yönetici yetkileri elde edilebilir mi?
- [ ] Hayır — yetki değişkenleri kullanıcılar tarafından **değiştirilemez**  
- [ ] Evet — değişkenler değiştirilebilir ancak uygulama bunları arka uç (backend) veritabanına göre **doğrular**  
- [ ] Evet — değişkenler değiştirilebilir ve uygulama oturum durumuna dolaylı olarak **güvenir** *(Kritik)*  

### Belirli bir iş akışı (örneğin şifre sıfırlama, ödeme işlemi) tamamlandığında oturum durumu derhal temizleniyor mu?
- [ ] Evet — iş akışına ait oturum değişkenleri tamamlanma anında yok edilir  
- [ ] Hayır — iş akışı değişkenleri **kalıcıdır** ve uygulamanın diğer alanlarında yeniden kullanılabilir  

---