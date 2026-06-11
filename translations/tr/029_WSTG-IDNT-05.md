## WSTG-IDNT-05 — Zayıf veya Uygulanmayan Kullanıcı Adı Politikası Testi

Zayıf veya uygulanmayan kullanıcı adı politikalarının test edilmesi, hesap tanımlayıcı oluşturma kurallarına ve bunların kullanıcı adı belirleme (Enumeration) veya yanıltma (Spoofing) saldırılarına karşı duyarlılığına odaklanır. Zayıf politikalar genellikle tahmin edilebilir dizilere, yaygın sözlük kelimelerine veya dahili çalışan kimliklerini yansıtan tanımlayıcılara izin verir; bu durum Brute Force ve Credential Stuffing saldırıları için arama alanını önemli ölçüde azaltır. Bir saldırgan açısından bakıldığında, bu kalıpların tanımlanması, genellikle kayıt hata mesajlarının, parola sıfırlama davranışlarının veya herkese açık profillerdeki meta verilerin analiz edilmesiyle elde edilen geniş ölçekli hesap keşfinin ilk adımıdır. Sağlam bir politikanın sağlanması, saldırganların kullanıcı tabanını sistematik olarak eşleştirmesini engeller ve hedefli oltalama (Phishing) ile otomatik kimlik doğrulama atlatma (Authentication Bypass) girişimlerine karşı savunmayı kolaylaştırır.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-IDNT-05 |
| **CWE** | CWE-521 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Düşük / Orta |

**Kaynaklar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/05-Testing_for_Weak_or_Unenforced_Username_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite (Intruder)`, `ffuf`, `Custom Python Scripts`, `theHarvester`

### Kullanıcı adları yüksek derecede tahmin edilebilir veya ardışık kalıplara mı dayanıyor?
- [ ] Hayır — kullanıcı adları rastgele, kullanıcı tarafından tanımlanmış veya yüksek entropili dizelerdir  
- [ ] Evet — kullanıcı adları tahmin edilebilir bir formatı takip eder (örneğin, `user1001`, `user1002`) ancak kimlik doğrulama sağlamdır  
- [ ] Evet — kullanıcı adları **kesinlikle ardışıktır** veya bilinen bir kurumsal formatı takip eder, bu da kullanıcı adı belirleme (Enumeration) işlemini kolaylaştırır  

### Uygulama, kullanıcı adları için minimum uzunluk ve karmaşıklık gereksinimlerini zorunlu kılıyor mu?
- [ ] Evet — kayıt sırasında katı uzunluk ve karakter seti politikaları **uygulanmaktadır**  
- [ ] Evet — politikalar mevcuttur ancak aşırı kısa (örneğin 1-2 karakter) veya aşırı basit kullanıcı adlarına izin verilir  
- [ ] Hayır — kullanıcı adı uzunluğu veya karakter türleri ile ilgili **hiçbir politika** uygulanmamaktadır  

### Geçerli kullanıcı adları uygulama yanıtları aracılığıyla tespit edilebilir mi (Enumeration)?
- [ ] Hayır — uygulama genel hata mesajları döndürür ve tüm girişimler için tutarlı bir zamanlama (Timing) sergiler  
- [ ] Evet — uygulama farklı hata mesajları döndürür (örneğin, "Kullanıcı adı zaten alınmış") ancak Rate Limiting **uygulanmaktadır**  
- [ ] Evet — uygulama; kayıt hataları, parola sıfırlama veya zamanlama farklılıkları yoluyla geçerli kullanıcı adlarını **sızdırır**  

### Politika, yaygın veya ayrılmış yönetici kullanıcı adlarının kaydedilmesini engelliyor mu?
- [ ] Evet — uygulama yaygın adları (örneğin `admin`, `root`, `support`) ve sistem tarafından ayrılmış terimleri engeller  
- [ ] Hayır — kullanıcılar hassas veya yönetici adlarıyla hesap **kaydedebilir**  

### Kullanıcı adı politikası tüm giriş noktalarında (API, Mobil, Web) tutarlı mı?
- [ ] Evet — politikalar tüm arayüzlerde ve sürümlerde tutarlı bir şekilde **uygulanmaktadır**  
- [ ] Hayır — eski uç noktalar (Legacy Endpoints) veya belirli API sürümleri aynı kullanıcı adı kısıtlamalarını **zorunlu kılmaz**  

---