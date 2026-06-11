## WSTG-BUSL-05 — Bir Fonksiyonun Kullanım Sınırı Sayısının Test Edilmesi

Bu test, bir uygulamanın tek bir kullanıcı, oturum veya IP adresi tarafından belirli bir eylemin veya fonksiyonun yürütülme sıklığı ve toplam sayısı üzerindeki iş mantığı (Business Logic) kısıtlamalarını uygulayıp uygulamadığını değerlendirir. Bu sınırların uygulanmaması; saldırganların SMS OTP (Tek Kullanımlık Şifre) gönderimi, şifre sıfırlama e-postalarının tetiklenmesi, indirim kodlarının uygulanması veya yüklü veri dışa aktarımı (Data Export) gibi yüksek değerli fonksiyonları kötüye kullanmasına olanak tanıyarak finansal kayba, kaynak tüketimine (Resource Exhaustion) veya itibar kaybına yol açar. Bu zafiyetler genellikle işlemsel uç noktalarda (Transactional Endpoints) veya iletişim özelliklerinde bulunur ve eylemi hedeflenen iş eşiklerinin çok ötesinde tekrarlayan otomatik betikler (Automated Scripts) aracılığıyla istismar edilir. Bir saldırganın bakış açısından bu durum; SMS Pumping, Mail Bombing veya açık bir hız sınırlandırma (Rate Limiting) ya da sayı tabanlı kısıtlama (Throttling) mekanizması bulunmayan iş akışlarına yönelik Brute Force (Kaba Kuvvet) saldırıları için birincil hedeftir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-BUSL-05 |
| **CWE** | CWE-799 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek* |

> *Eğer fonksiyon finansal işlemleri, SMS maliyetlerini içeriyorsa veya sistem genelindeki kullanılabilirliği etkiliyorsa Önem Derecesi "Yüksek" olur.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/05-Test_Number_of_Times_a_Function_Can_Be_Used_Limits  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite (Intruder/Repeater)`, `Turbo Intruder`, `Python (requests)`, `OWASP ZAP`

### Uygulama, hassas iş fonksiyonları üzerinde limitler tanımlıyor ve uyguluyor mu?
- [ ] Evet — limitler sıkı bir şekilde uygulanmaktadır ve atlatılması (Bypass) **mümkün değildir**  
- [ ] Evet — limitler uygulanıyor ancak kötüye kullanımı önlemek için çok yüksek  
- [ ] Hayır — fonksiyon yürütme üzerinde hiçbir limit uygulanmıyor  

### Hız sınırları (Rate Limits) ve kullanım sayaçları sunucu tarafında (Server-Side) mı uygulanıyor?
- [ ] Evet — uygulama kesinlikle sunucu tarafındadır ve **manipüle edilemez**  
- [ ] Hayır — uygulama istemci tarafı mantığına (örneğin JavaScript veya gizli alanlar) dayanıyor  
- [ ] Hayır — hiçbir seviyede uygulama mevcut değil  

### Kullanım sınırları, başlık (Header) veya oturum manipülasyonu yoluyla atlatılabilir mi?
- [ ] Hayır — `X-Forwarded-For`, `Origin` veya oturum rotasyonu (Session Rotation) yoluyla atlatma **mümkün değildir**  
- [ ] Evet — limitler, IP başlıklarını sahtelerken (Spoofing) veya çerezleri temizleyerek **atlatılabilir**  
- [ ] Evet — limitler, birden fazla hesap veya oturum oluşturularak **atlatılabilir**  

### Limitin aşılması uygun bir savunma yanıtını tetikliyor mu?
- [ ] Evet — uygulama `429 Too Many Requests` yanıtı döndürür ve olayı günlüğe kaydeder (Loglar) *(En güvenli)*  
- [ ] Evet — uygulama bir hata döndürür ancak potansiyel kötüye kullanımı **günlüğe kaydetmez**  
- [ ] Hayır — uygulama istekleri işlemeye devam eder ancak çıktıyı görmezden gelir  
- [ ] Hayır — uygulama yanıt vermez veya yük altında çöker  

### Fonksiyonun kötüye kullanılmasının iş üzerindeki etkisi önemli mi?
- [ ] Hayır — fonksiyonun kötüye kullanılmasının ihmal edilebilir bir maliyeti veya kaynak etkisi vardır  
- [ ] Evet — kötüye kullanım, küçük çaplı kaynak tüketimine veya kullanıcı rahatsızlığına **yol açabilir**  
- [ ] Evet — kötüye kullanım, önemli finansal maliyetlere (örneğin SMS ücretleri) veya hizmet reddine (DoS) **yol açabilir** *(Kritik)*  

---