## WSTG-INPV-06 — LDAP Injection Testi

LDAP Injection (LDAP Enjeksiyonu), bir uygulamanın kullanıcı tarafından sağlanan verileri, uygun temizleme (sanitization) veya kaçış karakteri (escaping) kullanmadan LDAP (Lightweight Directory Access Protocol - Hafif Dizin Erişim Protokolü) filtrelerine dahil etmesiyle, saldırganın sorgu mantığını manipüle etmesine olanak tanıdığında meydana gelir. Yıldız işareti (*), parantezler ve mantıksal operatörler gibi özel karakterler enjekte edilerek, saldırganlar arama filtresini kimlik doğrulama mekanizmalarını atlatacak veya kullanıcı adları, grup üyelikleri ve kurumsal öznitelikler dahil olmak üzere hassas dizin bilgilerini sızdıracak şekilde değiştirebilirler. Bu güvenlik zafiyeti tipik olarak kurumsal dizin aramaları, çalışan portalları veya Active Directory ya da OpenLDAP ile arayüz oluşturan tekli oturum açma (Single Sign-On - SSO) sistemleri gibi özelliklerde kendini gösterir. Bir saldırganın bakış açısıyla, başarılı bir sömürü (Exploit) süreci, genellikle doğrudan sorgu çıktısının uygulama tarafından engellendiği durumlarda, dizin yapısını veya öznitelik değerlerini bit bit numaralandırmak için blind (kör) tekniklerin kullanılmasını içerir.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INPV-06 |
| **CWE** | CWE-90 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek |

**Kaynaklar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/06-Testing_for_LDAP_Injection  
* https://hacktricks.wiki/en/pentesting-web/ldap-injection.html  

**Araçlar:** `Burp Suite (Intruder/Repeater)`, `ldapsearch`, `JNDIExploit`, `Wfuzz`, `nmap`

### Uygulama, kimlik doğrulama veya dizin aramaları için LDAP kullanıyor mu?
- [ ] Hayır — Uygulama mimarisinde LDAP **kullanılmıyor**  
- [ ] Evet — LDAP kullanılıyor ve tüm girdiler sıkı bir şekilde temizleniyor (sanitized) veya parametreleştiriliyor  
- [ ] Evet — LDAP kullanılıyor ve kullanıcı girdisi, uygun kaçış karakteri kullanılmadan filtrelere doğrudan ekleniyor (**concatenated**)  

### Kullanıcı girdisinde LDAP meta-karakterlerinden uygun şekilde kaçınılıyor mu veya bunlar filtreleniyor mu?
- [ ] Evet — `(`, `)`, `&`, `|`, `*` ve `\` gibi karakterlere **izin verilmiyor** veya bu karakterlerden doğru şekilde kaçınılıyor  
- [ ] Hayır — LDAP filtresinin yapısını değiştirmek için meta-karakterler enjekte **edilebiliyor**  

### Kimlik doğrulama mekanizması enjeksiyon yoluyla atlatılabilir mi?
- [ ] Hayır — Oturum açma mantığı sorgu manipülasyonuna karşı **hassas değil**  
- [ ] Evet — Kullanıcı adı veya parola alanlarında mantıksal OR (`|`) veya joker karakter (`*`) enjeksiyonu kullanılarak kimlik doğrulama **atlatılabilir**  

### Dizin servisinden blind (kör) veri sızıntısı mümkün mü?
- [ ] Hayır — Arama sonuçları sınırlıdır ve herhangi bir zamanlama (timing) veya mantıksal (boolean) fark gözlemlenmemektedir  
- [ ] Evet — Mantıksal tabanlı kör enjeksiyon (boolean-based blind injection) teknikleri ile dizin öznitelikleri bit bit **numaralandırılabilir** (enumerate)  
- [ ] Evet — Sorgu manipülasyonu nedeniyle tam dizin kayıtları uygulama yanıtına **yansıtılmaktadır**  

### İkincil uygulama bileşenleri (örn. posta göndericiler, SSO) LDAP tabanlı öznitelik enjeksiyonuna karşı savunmasız mı?
- [ ] Hayır — LDAP öznitelikleri tüm bileşenlerde güvenli bir şekilde işlenmektedir  
- [ ] Evet — Bir saldırgan, yetki yükseltmek veya iletişimleri yönlendirmek için enjeksiyon yoluyla kendi özniteliklerini (örn. e-posta, grup üyeliği) **değiştirebilir**  

---