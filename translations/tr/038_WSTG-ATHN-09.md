## WSTG-ATHN-09 — Zayıf Parola Değiştirme veya Sıfırlama İşlevselliklerinin Test Edilmesi

Parola değiştirme ve sıfırlama mekanizmaları, hatalı yapılandırıldıklarında saldırganların kullanıcı hesaplarını ele geçirmesine olanak tanıyan kimlik doğrulama yönetiminin (authentication management) kritik bileşenleridir. Bu zafiyetler genellikle tahmin edilebilir sıfırlama belirteçleri (tokens), Brute Force (kaba kuvvet) girişimleri sırasında hesap kilitleme (account lockout) eksikliği, belirteçlerin güvensiz iletimi veya mevcut parola doğrulanmadan parola değiştirilebilmesi gibi durumları içerir. Saldırganlar, kurtarma bağlantılarını ele geçirerek veya tahmin ederek, sıfırlama e-postalarını yönlendirmek için Host Header Injection gerçekleştirerek veya parola değişiminden sonra erişimi sürdürmek için Session Fixation (oturum sabitleme) yöntemini kullanarak bu kusurlardan yararlanırlar. Bu süreçlerin güçlü bir doğrulama gerektirmesini sağlamak ve kriptografik rastgelelik kullanmak, hesap bütünlüğünü korumak ve yetkisiz erişimi önlemek için esastır.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-ATHN-09 |
| **CWE** | CWE-640 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Kritik |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/09-Testing_for_Weak_Password_Change_or_Reset_Functionalities  
* https://hacktricks.wiki/en/pentesting-web/reset-password.html  

**Araçlar:** `Burp Suite (Repeater/Intruder)`, `Python (Custom Scripts)`, `CyberChef`, `OAST (Interactsh/Burp Collaborator)`

### Standart bir değişiklik sırasında yeni bir parola belirlemek için mevcut parola gerekli mi?
- [ ] Evet — mevcut parola **gereklidir** ve doğrulanmaktadır  
- [ ] Evet — mevcut parola **gereklidir** ancak Parameter Pollution veya manipülasyon yoluyla atlatılabilir  
- [ ] Hayır — mevcut parola **istenmemektedir**, bu da Session Hijacking (oturum çalma) yoluyla hesabın ele geçirilmesine olanak tanır *(Yüksek)*  

### Parola sıfırlama belirteçleri (tokens) kriptografik olarak güvenli ve tahmin edilemez mi?
- [ ] Evet — belirteçler yüksek entropili (high-entropy), rastgele ve **tahmin edilemezdir**  
- [ ] Evet — belirteçler oluşturulmaktadır ancak düşük entropi veya tahmin edilebilir desenler (örneğin zaman damgası tabanlı) göstermektedir  
- [ ] Hayır — belirteçler **tahmin edilebilirdir** veya Base64 ile kodlanmış e-posta/ID gibi statik kullanıcı verilerine dayanmaktadır *(Kritik)*  

### Parola sıfırlama bağlantısı Host Header Injection aracılığıyla yönlendirilebilir mi?
- [ ] Hayır — uygulama, bağlantı oluşturma için `Host` başlığını dikkate almaz veya doğrular  
- [ ] Evet — uygulama, bağlantıları oluşturmak için `Host` veya `X-Forwarded-Host` başlığını kullanır ancak doğrulama **mevcuttur**  
- [ ] Evet — bağlantılar, başlık manipülasyonu yoluyla saldırgan kontrolündeki bir alan adına **yönlendirilebilir** *(Yüksek)*  

### Sıfırlama belirtecinin sınırlı bir yaşam döngüsü ve anında geçersiz kılınma özelliği var mı?
- [ ] Evet — belirteçlerin süresi hızla dolar ve kullanım veya parola değişiminden **hemen sonra** geçersiz kılınırlar  
- [ ] Evet — belirteçlerin süresi uzun bir süre sonra (örneğin 24+ saat) dolar veya **birden fazla kullanıma** izin verilir  
- [ ] Hayır — belirteçlerin süresi **asla dolmaz** veya parola başarıyla değiştirildikten sonra bile geçerli kalmaya devam eder  

### Parola sıfırlama ve değiştirme uç noktalarında (endpoints) hız sınırlaması veya kilitleme mevcut mu?
- [ ] Evet — kullanıcı sayımını (enumeration) ve Brute Force girişimlerini önlemek için sıkı Rate Limiting ve CAPTCHA **uygulanmaktadır**  
- [ ] Evet — sınırlı kontroller mevcuttur ancak IP rotasyonu veya başlık yanıltma (header spoofing) yoluyla atlatma **mümkündür**  
- [ ] Hayır — herhangi bir Rate Limiting **uygulanmamaktadır**, bu da toplu hesap sayımına veya belirteçlere yönelik Brute Force saldırılarına izin verir  

---