## WSTG-IDNT-04 — Hesap Numaralandırma ve Tahmin Edilebilir Kullanıcı Hesapları Testi

Hesap numaralandırma (Account Enumeration), bir uygulamanın HTTP yanıtlarındaki varyasyonlar, hata mesajları veya ölçülebilir zamanlama farklılıkları aracılığıyla bir kullanıcı hesabının varlığını veya yokluğunu ifşa etmesi durumunda gerçekleşir. Saldırganlar, geçerli hesapları belirlemek için kullanıcı adı listelerini sistematik olarak göndererek bu davranışı istismar ederler; bu hesaplar daha sonra kimlik bilgisi doldurma (Credential Stuffing), kaba kuvvet (Brute Force) saldırıları veya sosyal mühendislik için hedef alınır. Bu zafiyet genellikle oturum açma portallarında, parola sıfırlama özelliklerinde, kayıt formlarında ve kullanıcıya özel tanımlayıcıları işleyen API uç noktalarında (endpoints) ortaya çıkar. Bir saldırganın perspektifinden, başarılı bir numaralandırma, kör bir kaba kuvvet girişimini bilinen, geçerli kimliklerin hedeflendiği bir istismara dönüştürerek saldırı yüzeyini önemli ölçüde daraltır.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-IDNT-04 |
| **CWE** | CWE-204 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Düşük / Orta |

**Kaynaklar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/04-Testing_for_Account_Enumeration_and_Guessable_User_Account  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite (Intruder/Comparer)`, `ffuf`, `Wfuzz`, `GoBuster`, `Hashcat`

### Kimlik doğrulama hata mesajları tüm senaryolarda tutarlı mı?
- [ ] Evet — hem geçersiz kullanıcı adları hem de geçersiz parolalar için genel (generic) hata mesajları kullanılıyor  
- [ ] Evet — mesajlar benzer, ancak noktalama veya büyük/küçük harf kullanımında hafif varyasyonlar **mevcut**  
- [ ] Hayır — hata mesajları açıkça "Kullanıcı bulunamadı" veya "Hatalı parola" ifadesini belirtiyor *(Zafiyet İçerebilir)*  

### Parola sıfırlama veya "parolamı unuttum" akışı hesap varlığını sızdırıyor mu?
- [ ] Hayır — uygulama, e-posta/kullanıcı adının var olup olmadığına bakılmaksızın genel bir mesaj döndürüyor  
- [ ] Evet — kontroller mevcut, ancak yanıt uzunluğu veya durum kodları aracılığıyla atlatma (bypass) **mümkündür**  
- [ ] Evet — uygulama, bir sıfırlama e-postasının gönderildiğini veya hesabın **mevcut olmadığını** açıkça onaylıyor  

### Kayıt işlemi kullanıcı numaralandırmaya karşı korumalı mı?
- [ ] Hayır — kayıt işlemi bilgi sızdırmıyor veya toplu kontrolleri önlemek için CAPTCHA/hız sınırlama (Rate Limiting) kullanıyor  
- [ ] Evet — kontroller mevcut, ancak zamanlama veya API yanıt farklılıkları aracılığıyla atlatma (bypass) **mümkündür**  
- [ ] Evet — uygulama, kullanıcı adının veya e-postasının **zaten alınmış olması** durumunda kullanıcıyı anında bilgilendiriyor  

### Geçerli ve geçersiz kullanıcı adları karşılaştırıldığında zamanlama farklılıkları ölçülebiliyor mu?
- [ ] Hayır — sabit zamanlı karşılaştırmalar (constant-time comparisons) nedeniyle hesap geçerliliğinden bağımsız olarak yanıt süreleri tutarlıdır  
- [ ] Evet — zamanlama farklılıkları mevcut ancak ağ üzerinden güvenilir bir şekilde ölçülemeyecek kadar küçük  
- [ ] Evet — ölçülebilir zamanlama tutarsızlıkları **mevcut** (örneğin, parola karma işleminin -password hashing- yalnızca geçerli kullanıcılar için çalıştırılması nedeniyle)  

### Uygulama, hesaplar için öngörülebilir veya tahmin edilebilir adlandırma kuralları kullanıyor mu?
- [ ] Hayır — hesap tanımlayıcıları rastgeledir (UUID) veya kullanıcı tarafından tanımlanmış ve karmaşıktır  
- [ ] Evet — hesap tanımlayıcıları bir kalıbı izliyor (örneğin, `emp001`, `emp002`) ve numaralandırma (Enumeration) **mümkündür**  
- [ ] Evet — tanımlayıcılar ardışık tam sayılardır, bu da tam veritabanı numaralandırmasını **mümkün** kılar  

---