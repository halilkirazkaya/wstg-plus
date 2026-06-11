## WSTG-ATHN-08 — Zayıf Güvenlik Sorusu Cevaplarının Test Edilmesi

Zayıf güvenlik sorusu cevaplarının test edilmesi, genellikle parola kurtarma veya çok faktörlü kimlik doğrulama akışları sırasında kullanıcının kimliğini doğrulamak için kullanılan bilgiye dayalı kimlik doğrulama (KBA - Knowledge-Based Authentication) mekanizmalarının değerlendirilmesini içerir. Bu durum önemlidir, çünkü güvenlik soruları genellikle açık kaynak istihbaratı (OSINT) veya sosyal mühendislik (social engineering) yoluyla kolayca keşfedilebilen statik, gizli olmayan bilgilere dayanır ve bu da yetkisiz hesap ele geçirmeye (account takeover) yol açar. Bu zafiyetler tipik olarak kullanıcıların önceden ayarlanmış sorulara cevap verdiği "Parolamı Unuttum" veya "Hesap Kilidi Açma" modüllerinde meydana gelir. Bir saldırganın bakış açısından bu; birçok kullanıcının "Annenizin kızlık soyadı nedir?" veya "Hangi liseye gittiniz?" gibi yaygın sorulara tahmin edilebilir veya herkese açık olarak doğrulanabilir cevaplar vermesi nedeniyle, otomatize edilmiş kaba kuvvet saldırıları (brute-forcing) veya hedefli araştırmalar için birincil bir hedeftir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-ATHN-08 |
| **CWE** | CWE-640 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek* |

> *Güvenlik sorusu, daha fazla doğrulama olmaksızın tam bir parola sıfırlama ve hesap ele geçirme için tek gereksinimse, önem derecesi Yüksek olur.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/08-Testing_for_Weak_Security_Question_Answer  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**Araçlar:** `Burp Suite (Intruder)`, `ffuf`, `Maltego`, `Sherlock`, `Social Engineering Toolkit (SET)`

### Uygulama, kimlik doğrulama veya parola kurtarma için güvenlik soruları uyguluyor mu?
- [ ] Hayır — güvenlik soruları kimlik doğrulama veya kurtarma için **kullanılmıyor**  
- [ ] Evet — güvenlik soruları isteğe bağlı bir ikincil faktör olarak **kullanılıyor**  
- [ ] Evet — güvenlik soruları birincil/tek kurtarma mekanizması olarak **kullanılıyor** *(Yüksek Risk)*  

### Güvenlik soruları sabit bir listeden mi seçiliyor yoksa kullanıcı tarafından mı tanımlanıyor?
- [ ] Hayır — sorular tamamen kullanıcı tanımlıdır ve yüksek entropi gerektirir  
- [ ] Evet — sorular sabittir ancak OSINT ile keşfedilemeyecek seçenekler içerir  
- [ ] Evet — sorular yaygın, OSINT ile keşfedilebilir konulardan oluşan sabit bir listedir *(Zafiyet İçerir)*  

### Güvenlik sorusu cevaplama girişimlerine hız sınırlama veya hesap kilitleme uygulanıyor mu?
- [ ] Evet — kaba kuvvet saldırılarını önlemek için katı hız sınırlama (rate limiting) ve hesap kilitleme **uygulanıyor**  
- [ ] Evet — hız sınırlama **uygulanıyor** ancak IP rotasyonu veya başlık manipülasyonu ile atlatma (bypass) **mümkündür**  
- [ ] Hayır — cevaplama girişimlerinde **hız sınırlama uygulanmıyor**  

### Uygulama, cevap içeriği üzerinde karmaşıklık veya doğrulama zorunluluğu getiriyor mu?
- [ ] Evet — doğrulama yaygın kelimeleri engeller ve cevap karmaşıklığını/benzersizliğini sağlar  
- [ ] Evet — temel doğrulama mevcuttur ancak yaygın kelime listeleri veya sözlük saldırıları (dictionary attacks) ile **atlatılabilir**  
- [ ] Hayır — **doğrulama yapılmıyor**; basit veya tek karakterli cevaplara **izin veriliyor**  

### Güvenlik sorusu aşaması mantıksal hatalar yoluyla atlatılabilir mi?
- [ ] Hayır — kurtarma akışı ilerlemek için kesinlikle geçerli bir cevap gerektirir  
- [ ] Evet — kurtarma akışı, HTTP yanıt kodları veya oturum parametreleri manipüle edilerek **atlatılabilir** (bypass)  
- [ ] Evet — cevap, sayfanın kaynak kodunda veya gizli alanlarında (hidden fields) yansıtılmaktadır  

---