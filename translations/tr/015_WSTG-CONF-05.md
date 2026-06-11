## WSTG-CONF-05 — Altyapı ve Uygulama Yönetim Arayüzlerini Numaralandırma

Yönetim arayüzleri, bir uygulamanın ve temel altyapısının yönetim kontrol düzlemini temsil eder ve genellikle sistem yapılandırmalarına, kullanıcı verilerine ve sunucu tarafı işlemlerine yetkili erişim sağlar. Saldırganlar; dizin kaba kuvvet saldırısı (Directory Brute Force), `robots.txt` analizi veya tahmin edilebilir URL kalıplarını gözlemleyerek, güçlü kimlik doğrulamadan yoksun olabilecek veya varsayılan kimlik bilgilerine (Default Credentials) dayanan giriş noktalarını bulmak için bu arayüzleri numaralandırmaya öncelik verirler. Açıkta kalan bir yönetim panelinin keşfedilmesi; sistemin tamamen ele geçirilmesi, yetkisiz veri değişikliği veya hizmet kesintisi riskini önemli ölçüde artırır. Bu arayüzler sıklıkla standart dışı portlarda veya alt alan adlarında (Subdomains) bulunur ve bu durum onları otomatik tarayıcılar ile manuel keşif çalışmaları için birincil hedef haline getirir.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CONF-05 |
| **CWE** | CWE-425 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek* |

> *Önem derecesi; arayüzün çok faktörlü kimlik doğrulama (MFA) olmaksızın sistem genelinde yapılandırma değişikliklerine, kullanıcı yönetimine veya veri sızdırmasına izin vermesi durumunda Yüksek (High) olarak kabul edilir.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/05-Enumerate_Infrastructure_and_Application_Admin_Interfaces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `ffuf`, `dirsearch`, `Gobuster`, `Nmap`, `Wappalyzer`, `Burp Suite (Intruder)`

### Yönetim arayüzleri, dizin fuzing (Fuzzing) veya keşif çalışmaları yoluyla keşfedilebiliyor mu?
- [ ] Hayır — hiçbir yönetim arayüzü açıkta değildir veya keşfedilememektedir  
- [ ] Evet — arayüzler keşfedilebilirdir ancak erişim dahili IP aralıklarıyla kısıtlanmıştır  
- [ ] Evet — arayüzler keşfedilebilirdir ve halka açıktır  

### Keşfedilen yönetim portallarında kimlik doğrulama zorunlu mu?
- [ ] Evet — çok faktörlü kimlik doğrulama (MFA) zorunludur  
- [ ] Evet — tek faktörlü kimlik doğrulama zorunludur  
- [ ] Hayır — arayüze erişmek için kimlik doğrulama gerekmemektedir *(Kritik)*  

### Keşfedilmeyi önlemek için yönetim yolları gizli mi yoksa standart dışı mı?
- [ ] Evet — yollar standart dışı, rastgeleleştirilmiş veya tahmin edilemeyen adlandırmalar kullanmaktadır  
- [ ] Hayır — yollar yaygın adlandırma kurallarını (örneğin `/admin`, `/manager`, `/console`) kullanmaktadır ve kolayca tahmin edilebilir  

### Arayüz, hassas sistem veya uygulama işlevselliğine erişim sağlıyor mu?
- [ ] Hayır — arayüz, etkisi minimum olan "salt okunur" (Read-only) bir durum panosudur  
- [ ] Evet — arayüz uygulama yapılandırmasını veya kullanıcı verilerini değiştirebilir  
- [ ] Evet — arayüz sistem düzeyinde komutlar çalıştırabilir veya veritabanı yönetimi gerçekleştirebilir  

---