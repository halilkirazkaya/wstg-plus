## WSTG-INFO-03 — Bilgi Sızıntısı için Web Sunucusu Meta Dosyalarının İncelenmesi

`robots.txt`, `sitemap.xml` ve `security.txt` gibi web sunucusu meta dosyaları (metafiles), crawler'ları yönlendirmek ve idari meta veriler sağlamak amacıyla tasarlanmıştır; ancak bu dosyalar genellikle yanlışlıkla hassas dizin yapılarını veya gizli uç noktaları (endpoints) ifşa ederler. Saldırganlar, uygulamanın saldırı yüzeyini (attack surface) listelemek için bu dosyaları analiz eder ve sıklıkla bağlantısı bulunmayan yönetim panellerini, yedekleme dizinlerini veya geliştirme ortamlarını işaret eden "Disallow" direktiflerini tespit ederler. Arama motoru talimatlarının ötesinde, `.well-known/` dizinindeki dosyalar veya `humans.txt` gibi dosyalar; teknoloji yığınlarını (technology stacks), geliştirici bilgilerini veya kurumsal güvenlik iletişim bilgilerini sızdırabilir. Bu dosyaların sistematik olarak incelenmesi, bir test uzmanının agresif kaba kuvvet (brute-force) keşfi gerçekleştirmeden yapısal ve teknik içgörüler elde etmesini sağlar.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INFO-03 |
| **CWE** | CWE-200 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Düşük / Bilgi Amaçlı* |

> *Meta dosyaları, kimlik doğrulaması gerektirmeyen idari arayüzleri veya hassas yapılandırma yedeklerini ifşa ediyorsa Önem Derecesi Orta (Medium) seviyesine yükselir.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/03-Review_Webserver_Metafiles_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `curl`, `wget`, `Burp Suite`, `FFUF`, `GoBuster`

### `robots.txt` dosyası mevcut mu ve hassas dizinleri ifşa ediyor mu?
- [ ] Hayır — `robots.txt` mevcut **değil**  
- [ ] Evet — `robots.txt` mevcut ancak yalnızca standart genel yolları içeriyor  
- [ ] Evet — `robots.txt` hassas dizin yollarını veya yönetim arayüzlerini ifşa ediyor  
- [ ] Evet — `robots.txt` yorum satırlarında kimlik bilgilerini veya belirli yazılım sürümlerini ifşa ediyor  

### Bir `sitemap.xml` dosyası mevcut mu ve gizli uygulama yapısını ifşa ediyor mu?
- [ ] Hayır — `sitemap.xml` mevcut **değil**  
- [ ] Evet — `sitemap.xml` mevcut ancak yalnızca halka açık URL'leri listeliyor  
- [ ] Evet — `sitemap.xml`, erişilebilen bağlantısı olmayan uç noktaları veya yalnızca dahili kaynakları listeliyor  

### Güvenlik ve kurumsal meta dosyaları teknik detayları ifşa ediyor mu?
- [ ] Hayır — kök dizinde veya `.well-known/` içinde meta veri dosyası bulunamadı  
- [ ] Evet — `security.txt` veya `humans.txt` mevcut ancak standart bilgiler içeriyor  
- [ ] Evet — meta dosyaları; sosyal mühendislik veya ileri düzey saldırılara yardımcı olabilecek dahili ana bilgisayar adlarını (hostnames), geliştirici kimliklerini veya teknoloji yığını detaylarını ifşa ediyor  

### Eski veya çerçeveye özgü meta dosyaları mevcut mu?
- [ ] Hayır — çerçeveye (framework) özgü dosya (örneğin `info.php`, `composer.json`, `package.json`) bulunamadı  
- [ ] Evet — `README.md`, `CHANGELOG.txt` veya `.DS_Store` gibi dosyalar mevcut ancak temizlenmiş (sanitized)  
- [ ] Evet — çerçeve veya sürüme özgü dosyalar mevcut ve kesin teknoloji parmak izi tespiti (fingerprinting) yapılmasına **olanak tanıyor**  

---