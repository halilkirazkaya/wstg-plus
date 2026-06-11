## WSTG-INFO-01 — Bilgi Sızıntısı İçin Arama Motoru Keşfi ve Keşif Çalışması Yürütülmesi

Arama motoru keşfi (Search engine discovery), hedef kuruluşun yanlışlıkla ifşa ettiği bilgileri tanımlamak için halka açık arama motorlarından, önbelleğe alınmış sayfalardan ve indeksleme hizmetlerinden yararlanmayı içerir. Saldırganlar, halka açık olmaması gereken hassas dosyaları, dizinleri, giriş portallarını, hata mesajlarını ve meta verileri keşfetmek için gelişmiş arama operatörlerini (`Google Dorks`) ve üçüncü taraf indeksleme hizmetlerini kullanırlar. Bu keşif (reconnaissance) aşaması kritiktir; çünkü hedefle doğrudan etkileşim gerektirmez, bu da onu neredeyse tespit edilemez kılarken açıkta kalan yönetim panelleri, yapılandırma dosyaları, veritabanı dökümleri (database dumps) ve dahili belgeler gibi yüksek değerli giriş noktalarını potansiyel olarak ortaya çıkarabilir.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INFO-01 |
| **CWE** | CWE-200 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/01-Conduct_Search_Engine_Discovery_Reconnaissance_for_Information_Leakage  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/external-recon-methodology/index.html  
* https://portswigger.net/web-security/information-disclosure  

**Araçlar:** `Google Dorks`, `theHarvester`, `Shodan`, `Censys`, `Wayback Machine`, `Recon-ng`, `SearchDiggity`

### Hassas dosyalar veya dizinler arama motorları tarafından indekslenmiş mi?
- [ ] Hayır — arama motoru sonuçlarında hassas içerik bulunmadı  
- [ ] Evet — yapılandırma dosyaları, yedekler veya dahili belgeler indekslenmiş *(Kritik)*  

### Google Dorking çalışması açıkta kalan yönetim veya giriş arayüzlerini ortaya çıkarıyor mu?
- [ ] Hayır — yönetici panelleri ve giriş sayfaları indekslenmemiş  
- [ ] Evet — yönetici panelleri indekslenmiş ancak güçlü çok faktörlü kimlik doğrulama gerektiriyor  
- [ ] Evet — yönetici panelleri indekslenmiş ve yeterli kontroller olmaksızın halka açık durumda  

### Sayfaların önbelleğe alınmış sürümleri, o zamandan beri kaldırılmış olan hassas verileri ifşa ediyor mu?
- [ ] Hayır — önbelleğe alınmış anlık görüntülerde hassas veri bulunmadı  
- [ ] Evet — önbelleğe alınmış sayfalar kimlik bilgileri, dahili IP adresleri veya hassas bilgiler içeriyor  

### `robots.txt` dosyası istenmeden hassas yolları ifşa ediyor mu?
- [ ] Hayır — `robots.txt` hassas dizin yapılarını ortaya çıkarmıyor  
- [ ] Evet — `robots.txt` saldırgan keşif çalışmasına yardımcı olan hassas yolları listeliyor  
- [ ] `robots.txt` dosyası mevcut değil  

### Üçüncü taraf hizmetler (`Shodan`, `Censys`, `Wayback Machine`) geçmişteki veya mevcut maruziyeti ortaya çıkarıyor mu?
- [ ] Hayır — üçüncü taraf indeksleme hizmetlerinden önemli bir bulgu elde edilmedi  
- [ ] Evet — geçmişe dönük anlık görüntüler veya servis taramaları hassas meta verileri veya servis banner bilgilerini (service banners) ortaya çıkarıyor

---

## WSTG-INFO-02 — Web Sunucusu Parmak İzi Belirleme (Fingerprint Web Server)

Web sunucusu parmak izi belirleme (Fingerprinting), hedef bir web sunucusunun belirli yazılım türünü, versiyonunu ve temel işletim sistemini tanımlama sürecidir. Saldırganlar, Apache, Nginx, IIS veya diğer sunucu teknolojilerinin belirli sürümlerine özgü yamalanmamış CVE'ler veya yapılandırma zayıflıkları gibi bilinen güvenlik açıklarını belirlemek için bu keşif (Reconnaissance) işlemini gerçekleştirirler. Bu işlem genellikle HTTP yanıt başlıklarının (örneğin; `Server`, `X-Powered-By`) analiz edilmesi, benzersiz protokol davranışlarının incelenmesi veya varsayılan hata sayfaları ve dosyalar aracılığıyla sızan bilgilerin gözlemlenmesiyle gerçekleştirilir. Başarılı bir parmak izi belirleme, saldırganın istismar (Exploit) stratejisini uyarlamasına, genel taramalardan bilinen mimari kusurlara yönelik yüksek olasılıklı saldırılara geçmesine olanak tanır.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INFO-02 |
| **CWE** | CWE-200 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Bilgi Düzeyi / Düşük* |

> *Parmak izi belirleme sonucunda bilinen kritik güvenlik açıklarına sahip, güncel olmayan veya kullanım ömrü dolmuş (EOL) bir sunucu sürümü ortaya çıkarsa önem derecesi "Yüksek" olur.

**Kaynaklar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/02-Fingerprint_Web_Server  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**Araçlar:** `curl`, `Nmap`, `WhatWeb`, `Wappalyzer`, `Nikto`, `Netcat`

### HTTP yanıt başlıklarında tanımlayıcı dizeler mevcut mu?
- [ ] Hayır — `Server` ve `X-Powered-By` başlıkları **mevcut değil** veya genel değerler içeriyor  
- [ ] Evet — başlıklar sunucu yazılımını açığa çıkarıyor ancak spesifik sürüm numaralarını **içermiyor**  
- [ ] Evet — başlıklar belirli sunucu yazılımını ve **tam** sürüm numaralarını açığa çıkarıyor  

### Varsayılan hata sayfaları veya sistem tarafından oluşturulan yanıtlar sunucu ayrıntılarını sızdırıyor mu?
- [ ] Hayır — özel hata sayfaları kullanılıyor ve sunucu bilgilerini **ifşa etmiyor**  
- [ ] Evet — varsayılan hata sayfaları sunucu yazılımı adını ve/veya sürümünü sızdırıyor  
- [ ] Evet — hata sayfaları sunucu ayrıntılarını, sürümünü ve **temel işletim sistemini** sızdırıyor  

### Sunucu sürümü varsayılan dosyalar veya dizin yapıları aracılığıyla ifşa ediliyor mu?
- [ ] Hayır — varsayılan kurulum dosyaları, kılavuzlar ve test betikleri **kaldırılmış**  
- [ ] Evet — varsayılan dosyalar (örneğin; `info.php`, `manual/`, `test.html`) **mevcut** ve sürüm verilerini ifşa ediyor  

### Sunucu türü benzersiz davranış analizi yoluyla doğru bir şekilde tahmin edilebiliyor mu?
- [ ] Hayır — sunucu yanıtları normalize edilmiş ve davranış tabanlı parmak izi belirleme (Fingerprinting) **mümkün değil**  
- [ ] Evet — benzersiz davranışlar (örneğin; başlık sıralaması, belirli hata kodları veya TCP/IP yığını tuhaflıkları) sunucu tanımlamasına **izin veriyor**  

### Tanımlanan sunucu sürümü şu anda destekleniyor ve yamalanmış mı?
- [ ] Evet — tanımlanan sürüm güncel ve satıcı tarafından **destekleniyor**  
- [ ] Hayır — tanımlanan sürüm **güncel değil** veya bilinen güvenlik açıklarına sahip **kullanım ömrü dolmuş (EOL)** bir sürüm

---

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

## WSTG-INFO-04 — Web Sunucusundaki Uygulamaların Numaralandırılması

Uygulama numaralandırma (Application Enumeration), tek bir web sunucusu veya IP adresi üzerinde barındırılan tüm web uygulamalarını ve servislerini tanımlama sürecidir. Modern web sunucuları genellikle birden fazla sanal ana makine (Virtual Host), eski hazırlık ortamları (Staging Environments) veya standart dışı portlarda ve yollarda yönetim arayüzleri barındırdığından, bu gizli varlıkların tanımlanamaması saldırı yüzeyinin (Attack Surface) önemli bir kısmının test edilmeden kalmasına neden olur. Saldırganlar, birincil üretim uygulamasından genellikle daha az güvenli olan bu unutulmuş veya gizlenmiş giriş noktalarını keşfetmek için DNS bölge transferleri (DNS zone transfers), sanal ana makine kaba kuvvet saldırıları (Virtual Host Brute-Forcing) ve ters IP aramaları (Reverse IP Lookups) gibi teknikleri kullanırlar.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INFO-04 |
| **CWE** | CWE-200 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Bilgi Düzeyi / Düşük |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/04-Attack_Surface_Identification  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/external-recon-methodology/index.html  

**Araçlar:** `nmap`, `ffuf`, `gobuster`, `theHarvester`, `Dig`, `DNSrecon`, `Reverse IP Lookups`, `Shodan`

### Hedef altyapı ile ilişkili birden fazla IP adresi veya DNS takma adı (Alias) mevcut mu?
- [ ] Hayır — yalnızca tek bir IP ve A-kaydı (A-record) mevcut  
- [ ] Evet — birden fazla IP adresi veya takma ad tespit edildi, kapsam genişletildi  

### Web sunucusu aynı IP üzerinde birden fazla Sanal Ana Makine (vhosts) barındırıyor mu?
- [ ] Hayır — sunucu tarafından yalnızca birincil alan adı (domain) sunulmaktadır  
- [ ] Evet — ek sanal ana makineler tespit edildi ancak erişim **mümkün değil** (ör. 403 Forbidden)  
- [ ] Evet — gizli, geliştirme veya hazırlık (staging) sanal ana makineleri tespit edildi ve erişilebilir durumda  

### Uygulamalar standart dışı portlarda mı barındırılıyor?
- [ ] Hayır — sadece standart portlar (80/443) açık  
- [ ] Evet — standart dışı portlar açık ancak web uygulaması **barındırmıyor**  
- [ ] Evet — standart dışı portlarda ek web uygulamaları tespit edildi (ör. 8080, 8443, 9000)  

### Farklı uygulamalar belirli URI yollarına (URI paths) mı atanmış?
- [ ] Hayır — tek bir uygulama tüm kök dizine (root directory) hizmet vermektedir  
- [ ] Evet — dizin numaralandırma (Directory Enumeration) aracılığıyla birden fazla uygulama keşfedildi (ör. `/api`, `/portal`, `/backup`)  

### Üçüncü taraf keşif (Reconnaissance) servisleri geçmişe ait veya ikincil uygulamaları ortaya çıkarıyor mu?
- [ ] Hayır — Shodan, Censys veya Wayback Machine üzerinden anlamlı bir bulgu elde edilmedi  
- [ ] Evet — geçmişe dönük anlık görüntüler (snapshots) veya servis taramaları, şu anda aktif olan ancak bağlantısı bulunmayan uygulamaları ortaya çıkarıyor

---

## WSTG-INFO-05 — Bilgi Sızıntısı için Web Sayfası İçeriğinin İncelenmesi

Web sayfası içeriğinin incelenmesi; kullanıcılara kasıtsız olarak ifşa edilen hassas bilgileri tespit etmek amacıyla HTML, JavaScript ve CSS dosyaları dahil olmak üzere uygulama yanıtlarının manuel ve otomatik olarak denetlenmesini kapsar. Saldırganlar; geliştirici yorumlarını, gizli form alanlarını, dahili sunucu yollarını, sabit kodlanmış (hardcoded) API anahtarlarını ve sonraki sömürü aşamalarına yardımcı olabilecek hata ayıklama (debugging) bilgilerini bulmak için istemci tarafı kaynak kodunu titizlikle inceler. Bu sızıntı genellikle, geliştiricilerin uygulamayı canlı ortama (production) almadan önce meta verileri veya hata ayıklama kodlarını temizlemeyi unuttuğu durumlarda meydana gelir ve potansiyel olarak mantıksal hataları veya arka uç altyapı detaylarını ortaya çıkarır. Saldırganın bakış açısından bu durum, güvenlik uyarılarını tetiklemeden veya sunucunun arka uç mantığıyla etkileşime girmeden uygulamanın dahili yapısını haritalandırmak ve gözden kaçan giriş noktalarını keşfetmek için düşük çaba gerektiren bir yöntem sağlar.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INFO-05 |
| **CWE** | CWE-200 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Düşük / Orta* |

> *Sabit kodlanmış hassas kimlik bilgileri, özel anahtarlar (private keys) veya dahili ortam değişkenleri keşfedilirse Önem Derecesi "Yüksek" olur.

**Kaynaklar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/05-Review_Web_Page_Content_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**Araçlar:** `Burp Suite (Target/Proxy)`, `Browser Developer Tools`, `SecretFinder`, `grep`, `truffleHog`, `LinkFinder`

### Kaynak kodda hassas bilgiler içeren geliştirici yorumları bulunuyor mu?
- [ ] Hayır — yorum bulunamadı veya yalnızca genel, hassas olmayan yorumlar bulundu  
- [ ] Evet — yorumlar mevcut ancak hassas bir mantık veya veri içermiyor  
- [ ] Evet — yorumlar dahili yolları, SQL sorgularını veya idari talimatları ifşa ediyor  
- [ ] Evet — yorumlar kimlik bilgilerini veya hassas geliştirici notlarını ifşa ediyor *(Yüksek)*  

### Gizli HTML girdi (input) alanları hassas veri veya durum (state) bilgisi içeriyor mu?
- [ ] Hayır — gizli alan bulunamadı veya bunlar yalnızca hassas olmayan jetonlar (tokens) içeriyor  
- [ ] Evet — gizli alanlar durum bilgisi için kullanılıyor ancak şifrelenmiş (encrypted) veya imzalanmış (signed)  
- [ ] Evet — gizli alanlar düz metin (plaintext) parolalar, kullanıcı rolleri veya dahili kimlikler (IDs) içeriyor  

### JavaScript dosyalarında sabit kodlanmış sırlar, API anahtarları veya dahili IP adresleri mevcut mu?
- [ ] Hayır — JS dosyalarında hassas dizgiler (strings) veya sırlar bulunamadı  
- [ ] Evet — JS dosyaları uygun kısıtlamalara sahip genel (public) API anahtarları içeriyor  
- [ ] Evet — JS dosyaları korumasız API anahtarları, dahili IP'ler veya özel uç noktalar (endpoints) içeriyor  
- [ ] Evet — JS dosyaları sabit kodlanmış idari kimlik bilgileri veya özel anahtarlar (private keys) içeriyor *(Kritik)*  

### Uygulama, kaynak kodda uygulama iskeleti (framework) sürümlerini veya dahili dosya yollarını ifşa ediyor mu?
- [ ] Hayır — framework bilgileri ve dosya yolları küçültülmüş (minified) veya karartılmış (obfuscated)  
- [ ] Evet — framework isimleri ve sürümleri görünüyor ancak yamalanmış  
- [ ] Evet — dahili dosya yolları veya mutlak sunucu yolları kaynak kodda ifşa edilmiş  

### Kaynak kod, küçültme (minification) veya karartma (obfuscation) eksikliği nedeniyle sızıntıya eğilimli mi?
- [ ] Hayır — canlı (production) kod tamamen küçültülmüş ve yorumlardan arındırılmış  
- [ ] Evet — kod küçültülmüş ancak yorumlar kaynakta kalmış  
- [ ] Evet — kaynak haritaları (`.map` dosyaları) herkese açık olarak erişilebilir durumda ve kaynak kodun tamamen yeniden oluşturulmasına (reconstruction) olanak tanıyor

---

## WSTG-INFO-06 — Uygulama Giriş Noktalarının Belirlenmesi

Uygulama giriş noktalarının belirlenmesi, bir kullanıcının veya harici sistemin uygulama ile etkileşime girebileceği tüm olası yolların numaralandırılarak (enumeration) tüm saldırı yüzeyinin (attack surface) haritalanmasını içerir. Bu süreç; tüm URL'lerin, API uç noktalarının (endpoints), parametrelerin (GET, POST, çerez tabanlı), HTTP başlıklarının (headers) ve WebSockets veya gRPC gibi özelleşmiş protokollerin keşfedilmesini kapsar. Bir sızma testi uzmanı (penetration tester) için bu aşama hayati öneme sahiptir; çünkü zafiyetler genellikle geliştirme sırasında gözden kaçan belgelenmemiş "gölge" (shadow) API'lerde, eski (legacy) uç noktalarda veya karmaşık parametre yapılarında bulunur. Kapsamlı bir numaralandırma, uygulamanın hiçbir bileşeninin test edilmemiş bir "kara kutu" (black box) olarak kalmamasını sağlar ve böylece saldırganların sisteme giden izlenmeyen yollar bulmasını engeller.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INFO-06 |
| **CWE** | CWE-1059 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Bilgi Düzeyi (Informational) |

**Kaynaklar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/06-Identify_Application_Entry_Points  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite (Target/Proxy)`, `OWASP ZAP`, `ffuf`, `Arjun`, `LinkFinder`, `GoSpider`, `KiteRunner`

### Uygulama genelindeki tüm GET ve POST parametreleri numaralandırıldı mı?
- [ ] Evet — otomatik ve manuel tarama (crawling) yoluyla kapsamlı numaralandırma **tamamlanmıştır**  
- [ ] Evet — standart tarama yoluyla yalnızca yaygın parametreler belirlenmiştir  
- [ ] Hayır — parametreler büyük ölçüde **belirlenmemiştir** veya test edilmemiştir  

### Belgelenmemiş veya gizli parametreler (örneğin; debug, admin) fuzzing kullanılarak araştırıldı mı?
- [ ] Hayır — bu özel kapsam için gizli parametre keşfi **gerekli değildir**  
- [ ] Evet — parametre fuzzing işlemi (örneğin; `Arjun` kullanılarak) **gerçekleştirilmiştir** ve herhangi bir bulguya rastlanmamıştır  
- [ ] Evet — gizli parametreler **keşfedilmiştir** ve daha ileri testler için haritalanmıştır  
- [ ] Hayır — gizli parametre keşfi **gerçekleştirilmemiştir**  

### Her bir uç nokta için desteklenen tüm HTTP yöntemleri (PUT, DELETE, PATCH vb.) belirlendi mi?
- [ ] Evet — yöntem keşfi keşfedilen tüm uç noktalara **uygulanmıştır**  
- [ ] Evet — yöntem keşfi yalnızca yüksek öncelikli veya hassas uç noktalara **uygulanmıştır**  
- [ ] Hayır — yalnızca standart GET ve POST yöntemleri **belirlenmiştir**  

### WebSockets, gRPC gibi standart dışı giriş noktaları veya özel başlıklar haritalandı mı?
- [ ] Hayır — uygulama standart dışı protokoller veya özel başlıklar (custom headers) **kullanmamaktadır**  
- [ ] Evet — protokole özgü tüm giriş noktaları ve özel başlıklar **belirlenmiştir**  
- [ ] Hayır — standart dışı giriş noktaları **mevcut** ancak haritalanmamıştır  

### API yüzeyi (v1/ veya /v2/ gibi sürümlendirilmiş uç noktalar dahil) tamamen haritalandı mı?
- [ ] Hayır — uygulama bir API **sunmamaktadır**  
- [ ] Evet — tüm API sürümleri ve uç noktaları **belirlenmiştir** ve belgelenmiştir  
- [ ] Evet — yalnızca mevcut üretim (production) API sürümü **belirlenmiştir**  
- [ ] Hayır — API uç noktaları **haritalanmamış** veya yalnızca kısmen keşfedilmiştir

---

## WSTG-INFO-07 — Uygulama Boyunca Yürütme Yollarının Haritalanması

Yürütme yollarının haritalanması (mapping execution paths), bir web uygulaması içindeki tüm erişilebilir uç noktaların (endpoints), fonksiyonel akışların ve karar verme dallarının sistematik olarak tanımlanmasını içerir. Bu süreç; eski kodlar (legacy code), hata ayıklama arayüzleri (debug interfaces) veya belgelenmemiş API rotaları gibi genellikle modern güvenlik kontrollerinden yoksun olan "karanlık" işlevlerin güvenlik incelemesinden gizli kalmamasını sağlamak için hayati önem taşır. Saldırganlar, uygulamanın yapısını görselleştirmek için otomatik örümcek tarama (spidering) ve manuel keşif yöntemlerinin bir kombinasyonunu kullanır; bu süreçte genellikle yetkisiz yönetici erişimi veya iş mantığı hataları (business logic flaws) gibi zafiyetleri keşfederler. Test uzmanları, girdileri belirli sunucu tarafı yanıtlarıyla ilişkilendirerek, kapsamlı bir test sağlamak amacıyla hedeflenmiş derinlemesine inceleme gerektiren hassas işleme mantıklarını tam olarak belirleyebilirler.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INFO-07 |
| **CWE** | CWE-200 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Şiddet** | Bilgilendirme |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/07-Map_Execution_Paths_Through_Application  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite Professional`, `OWASP ZAP`, `Katana`, `FFUF`, `LinkFinder`, `GoBuster`, `ParamMiner`

### Uygulama, otomatik ve manuel emekleme (crawling) kullanılarak tam olarak dizine eklendi mi?
- [ ] Evet — tüm görünür ve bağlantılı kaynakların kapsamlı haritası **tamamlandı**  
- [ ] Evet — otomatik emekleme (crawling) **tamamlandı** ancak manuel keşif bekleniyor  
- [ ] Hayır — uygulama dizine **eklenmedi**  

### JS analizi veya dizin kaba kuvvet saldırısı (directory brute-forcing) yoluyla gizli veya belgelenmemiş uç noktalar (endpoints) tespit edildi mi?
- [ ] Hayır — yan kanal analizi (side-channel analysis) yoluyla hiçbir belgelenmemiş yol keşfedilmedi  
- [ ] Evet — gizli yollar keşfedildi ancak yetkilendirme olmadan bunlara **erişilemiyor**  
- [ ] Evet — belgelenmemiş uç noktalar **erişilebilir** durumdadır ve hassas işlevsellik sağlamaktadır *(Kritik / Yüksek)*  

### Yürütme yolları, standart dışı parametreler veya başlıklar (headers) aracılığıyla değiştirilebiliyor mu?
- [ ] Hayır — `debug`, `test` veya `admin` gibi parametreler yürütme akışını **etkilemiyor**  
- [ ] Evet — parametreler çıktıyı değiştiriyor ancak güvenlik kontrollerini **atlatmıyor**  
- [ ] Evet — yol değiştiren parametreler **uygulanıyor** ve hedeflenen mantığın atlatılmasına izin veriyor  

### Çok adımlı iş mantığı (örneğin; çok faktörlü kimlik doğrulama, ödeme) akışı tam olarak haritalandı mı?
- [ ] Evet — çok adımlı süreçlerdeki tüm durumlar ve geçişler **tanımlandı**  
- [ ] Hayır — karmaşık durum makinesi (state machine) geçişleri tam olarak haritalanamıyor  

### API dokümantasyon dosyaları (Swagger/OpenAPI) veya istemci tarafı haritaları (Source Maps) ifşa edilmiş mi?
- [ ] Hayır — hiçbir mimari harita veya dokümantasyon dosyası herkese açık şekilde ifşa edilmedi  
- [ ] Evet — dokümantasyon mevcut ancak kimlik doğrulaması ile **korunuyor**  
- [ ] Evet — Swagger UI, OpenAPI JSON veya JS Source Maps **etkinleştirilmiş** ve herkese açık olarak erişilebilir durumda

---

## WSTG-INFO-08 — Web Uygulama Çerçevesi Parmak İzi Belirleme (Fingerprint Web Application Framework)

Bir web uygulama çerçevesinin (framework) parmak izinin belirlenmesi (fingerprinting), uygulamayı oluşturmak ve sunmak için kullanılan spesifik teknoloji yığınını ve yazılım sürümlerini tanımlamayı içerir. Saldırganlar; hedefin React, Angular, Django veya Spring gibi platformlar üzerinde çalışıp çalışmadığını belirlemek için HTTP yanıt başlıklarını (headers), çerçeveye özgü çerezleri (cookies), öngörülebilir dosya yapılarını ve benzersiz JavaScript bileşenlerini analiz ederler. Bu keşif (reconnaissance) aşaması, test uzmanının saldırı yüzeyini (attack surface) bilinen zafiyetlere (CVE'ler) ve söz konusu çerçeveye özgü yaygın yapılandırma zayıflıklarına göre daraltmasına olanak tanıdığı için hayati önem taşır. Saldırgan, temel mimariyi anlayarak genel testlerden yüksek düzeyde hedeflenmiş sömürme (exploitation) stratejilerine geçiş yapabilir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INFO-08 |
| **CWE** | CWE-200 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Bilgi Amaçlı (Informational) |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/08-Fingerprint_Web_Application_Framework  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Wappalyzer`, `BuiltWith`, `WhatWeb`, `nmap`, `Burp Suite`, `curl`

### HTTP yanıt başlıkları çerçeve adını veya sürümünü ifşa ediyor mu?
- [ ] Hayır — `X-Powered-By` veya `Server` gibi başlıklar mevcut **değil** veya sanitize edilmiş (arındırılmış)  
- [ ] Evet — çerçeve adı mevcut ancak sürüm **gizli**  
- [ ] Evet — hem çerçeve adı hem de sürüm **ifşa ediliyor**  

### Çerçeveye özgü çerezler veya dizin yapıları tanımlanabiliyor mu?
- [ ] Hayır — yaygın çerçeve bileşenleri (örneğin `.js` yolları, çerez adları) karartılmış (obfuscated) veya yeniden adlandırılmış  
- [ ] Evet — çerçeveye özgü çerezler (örneğin `JSESSIONID`, `PHPSESSID`, `csrftoken`) tanımlanabiliyor  
- [ ] Evet — dizin yapıları (örneğin `/wp-content/`, `_next/static/`) görünüyor ve çerçeveyi doğruluyor  

### Hata mesajları veya kaynak kod yorumları çerçeve ayrıntılarını açığa çıkarıyor mu?
- [ ] Hayır — hata yönetimi genel (generic) yapılmış ve yorumlar temizlenmiş  
- [ ] Evet — çerçeveye özgü yığın izleri (stack traces) **etkinleştirilmiş** ve yanıtlarda görünüyor  
- [ ] Evet — HTML kaynak kodu, çerçeveyi tanımlayan yorumlar veya meta veri etiketleri içeriyor  

### Tanımlanan çerçeve sürümü ile ilişkili bilinen zafiyetler var mı?
- [ ] Hayır — tanımlanan çerçeve güncel ve bilinen herhangi bir genel zafiyeti **yok**  
- [ ] Evet — çerçeve sürümü güncel değil ve spesifik CVE'ler tanımlanabiliyor

---

## WSTG-INFO-09 — Web Uygulaması Parmak İzi Belirleme (Fingerprinting)

Bir web uygulamasının parmak izini belirlemek (Fingerprinting), belirli framework, İçerik Yönetim Sistemi (CMS) veya temel teknoloji yığını ile bunlarla ilişkili sürümlerin tespit edilmesi sürecidir. Bu keşif (Reconnaissance) aşaması, tespit edilen bileşenlerin bilinen zafiyetler (CVE) ve halka açık exploit'ler (Exploit) ile eşleştirilmesine olanak tanıyarak saldırı yüzeyini önemli ölçüde daralttığı için hayati önem taşır. Bilgiler tipik olarak HTTP yanıt başlıklarından (Response Headers), benzersiz dosya yollarından, çerez (Cookie) adlarından, HTML kaynak kodu meta verilerinden ve statik varlıkların kriptografik özetlerinden (Hash) elde edilir. Teknoloji yığını doğru bir şekilde belirlenerek, bir saldırgan genel otomatik taramalardan sürüm odaklı zayıflıkların hedeflendiği istismar aşamasına geçebilir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INFO-09 |
| **CWE** | CWE-200 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Düşük / Bilgisel |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/09-Fingerprint_Web_Application  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Wappalyzer`, `WhatWeb`, `BuiltWith`, `CMSmap`, `nmap`, `Nikto`, `Burp Suite`

### HTTP yanıt başlıkları teknoloji yığınını veya sürüm numaralarını ifşa ediyor mu?
- [ ] Hayır — hassas başlıklar kaldırılmış veya genel değerler içeriyor  
- [ ] Evet — `X-Powered-By`, `Server` veya `X-AspNet-Version` gibi varsayılan başlıklar mevcut  
- [ ] Evet — başlıklar web sunucusunun veya uygulama framework'ünün belirli, güncel olmayan sürümlerini ifşa ediyor  

### Uygulama framework'ü veya CMS, benzersiz dosya yolları veya adlandırma kuralları aracılığıyla tanımlanabiliyor mu?
- [ ] Hayır — dosya yolları gizlenmiş (Obfuscated) ve temel teknolojiyi açığa çıkarmıyor  
- [ ] Evet — yaygın dizin yapıları (örneğin `/wp-content/`, `/_next/`, `/node_modules/`) görünür durumda  
- [ ] Evet — `robots.txt`, `humans.txt` veya `sitemap.xml` gibi benzersiz dosyalar teknolojiye özgü meta veriler içeriyor  

### HTML kaynak kodu veya statik varlıklar içinde belirli sürüm numaraları ifşa ediliyor mu?
- [ ] Hayır — yorum satırlarında veya varlık parametrelerinde (Asset parameters) sürüm bilgisi bulunamadı  
- [ ] Evet — HTML yorumları veya meta etiketleri (örneğin `<meta name="generator" content="...">`) mevcut  
- [ ] Evet — sürüm dizeleri CSS/JS dosya adlarına eklenmiş (örneğin `jquery.js?v=1.12.4`) ve devre dışı bırakılamıyor  

### Varsayılan hata sayfaları veya yönetim arayüzleri yazılım ortamını açığa çıkarıyor mu?
- [ ] Hayır — uygulama, tüm durum kodları için özel ve genel hata sayfaları kullanıyor  
- [ ] Evet — web sunucusu veya framework için varsayılan hata sayfaları etkin ve sürüm verisi sızdırıyor  
- [ ] Evet — yönetim paneli giriş portalları (örneğin `/wp-admin`, `/phpmyadmin`) erişilebilir ve tanımlanabilir durumda  

### Çerezler oturum yönetimi framework'ünü ifşa ediyor mu?
- [ ] Hayır — oturum çerezleri (Session cookies) genel veya özel adlar kullanıyor  
- [ ] Evet — teknolojiye özgü çerez adları (örneğin `PHPSESSID`, `JSESSIONID`, `ASPSESSIONID`) kullanılıyor

---

## WSTG-INFO-10 — Uygulama Mimarisini Haritalama

Uygulama mimarisini haritalama; web uygulamasını destekleyen temel teknolojilerin, altyapı bileşenlerinin ve veri akış yollarının belirlenmesini içerir. Test uzmanları; HTTP yanıt başlıklarını (response headers), hata mesajlarını, çerez (cookie) formatlarını ve dosya uzantılarını analiz ederek web sunucuları, uygulama sunucuları, veritabanları ve kullanılan üçüncü taraf entegrasyonlarının bir şemasını yeniden oluşturabilirler. Bu bilgi, platforma özgü exploit'lerin seçilmesine ve yük dengeleyiciler (load balancers) ile WAF'lar gibi yapılandırılmış ara cihazlardaki potansiyel darboğazların veya hatalı yapılandırmaların belirlenmesine olanak tanıdığı için sonraki saldırıların özelleştirilmesinde temel teşkil eder. Bir saldırgan, genel taramadan spesifik yazılım yığınının (software stack) ve birbiriyle bağlantılı bileşenlerinin hedeflenmiş sömürüsüne geçmek için bu yapısal haritayı kullanır.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INFO-10 |
| **CWE** | CWE-200 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Bilgi Düzeyi / Düşük* |

> *Dahili ağ topolojisi veya özel IP adresleri ifşa edilirse Önem Derecesi Orta (Medium) olur.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/10-Map_Application_Architecture  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Wappalyzer`, `BuiltWith`, `nmap`, `WhatWeb`, `Burp Suite`, `Netcat`, `curl`

### Web sunucusu ve uygulama sunucusu teknolojileri doğru bir şekilde tanımlanabiliyor mu?
- [ ] Hayır — etkili sıkılaştırma (hardening) veya karartma (obfuscation) nedeniyle tanımlama **mümkün değil**  
- [ ] Evet — teknoloji türü biliniyor ancak spesifik sürümler **belirlenemiyor**  
- [ ] Evet — teknoloji ve spesifik sürümler, başlıklar veya dosya imzaları (file signatures) aracılığıyla tamamen **ifşa ediliyor** *(Bilgi Düzeyi)*  

### İletişim yolunda ara cihazlar (WAF'lar, Yük Dengeleyiciler, Proxy'ler) tespit edilebiliyor mu?
- [ ] Hayır — hiçbir ara cihaz tespit edilmedi veya cihazlar tamamen şeffaf  
- [ ] Evet — zamanlama veya davranış yoluyla varlıklarından şüpheleniliyor ancak kimlikleri **doğrulanmadı**  
- [ ] Evet — spesifik ürünler (örneğin Cloudflare, Nginx, F5), benzersiz başlıklar veya davranışlar aracılığıyla **tanımlanıyor**  

### Uygulama, dahili dizin yapısı veya ağ topolojisi hakkında bilgi sızdırıyor mu?
- [ ] Hayır — dahili yollar ve IP'ler **ifşa edilmiyor**  
- [ ] Evet — dahili yollar hata mesajlarında veya yığın izlerinde (stack traces) sızdırılıyor ancak dahili IP'ler **sızdırılmıyor**  
- [ ] Evet — hem dahili dizin yapıları hem de özel IP adresleri **ifşa ediliyor**  

### Arka uç veritabanı türü ve sürümü uygulama davranışından çıkarılabiliyor mu?
- [ ] Hayır — veritabanı ayrıntıları **belirlenemiyor**  
- [ ] Evet — veritabanı türü (örneğin PostgreSQL, MSSQL), hata mesajları veya davranış yoluyla **tahmin ediliyor**  
- [ ] Evet — veritabanı türü ve sürümü, yanıt gövdelerinde veya başlıklarında açıkça **ifşa ediliyor**  

### Uygulama, tanımlanabilir bulut hizmet sağlayıcılarında veya belirli üçüncü taraf CDN'lerde mi barındırılıyor?
- [ ] Hayır — barındırma altyapısı **tanımlanamıyor**  
- [ ] Evet — bulut sağlayıcısı (AWS, Azure, GCP) tanımlanıyor ancak spesifik hizmetler **belirlenemiyor**  
- [ ] Evet — spesifik bulut hizmetleri (örneğin S3 bucket'ları, Lambda, Azure Functions) **haritalanmış ve erişilebilir durumda**

---

## WSTG-CONF-01 — Ağ Altyapı Yapılandırmasının Test Edilmesi

Ağ altyapı yapılandırma testi, web uygulamasını destekleyen temel sunucu ve ağ bileşenlerindeki zayıflıkların tanımlanmasını içerir. Saldırganlar; yer edinmek, yatayda hareket etmek veya keşif yapmak için yanlış yapılandırılmış servisleri, güncelliğini yitirmiş protokolleri ve gereksiz açık portları hedef alırlar. Yaygın bulgular arasında güvensiz TLS sürümleri, ayrıntılı servis başlıkları (verbose service banners) ve dahili ağlarla sınırlandırılması gereken açık yönetim arayüzleri yer alır. Bu altyapı düzeyi kusurların istismar edilmesi (Exploit) yoluyla, bir saldırgan iletim halindeki verilerin gizliliğini tehlikeye atabilir veya ana işletim sistemine yetkisiz erişim sağlayabilir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CONF-01 |
| **CWE** | CWE-16 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Düşük / Orta |

**Kaynaklar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/01-Test_Network_Infrastructure_Configuration  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/pentesting-methodology.html  

**Araçlar:** `nmap`, `sslyze`, `testssl.sh`, `Nikto`, `OpenVAS`, `Nessus`

### Uygulama sunucusunda gereksiz açık portlar veya dışarıya açık servisler bulunuyor mu?
- [ ] Hayır — yalnızca gerekli portlara (örneğin, 443) **erişilebilir**  
- [ ] Evet — ek servisler dışarıya açıktır ancak **güvenli** bir yapılandırmaya sahiptir  
- [ ] Evet — temel olmayan servisler (örneğin FTP, Telnet, SMB) halka açık şekilde **maruz bırakılmıştır**  

### Servis başlıkları (banners) veya header bilgileri hassas sürüm bilgisi sızdırıyor mu?
- [ ] Hayır — başlıklar kaldırılmış veya geneldir  
- [ ] Evet — başlıklar sunucu yazılımını (örneğin Apache/nginx) gösteriyor ancak sürüm numaralarını **göstermiyor**  
- [ ] Evet — ayrıntılı başlıklar belirli yazılım sürümlerini ifşa ederek **istismarı (exploitation)** kolaylaştırıyor  

### TLS yapılandırması modern güvenlik en iyi uygulamalarını takip ediyor mu?
- [ ] Evet — güçlü şifreleme paketleri (cipher suites) ile yalnızca TLS 1.2/1.3 **etkindir**  
- [ ] Evet — eski protokoller (TLS 1.0/1.1) **etkindir**  
- [ ] Hayır — zayıf şifrelemeler veya güvensiz protokoller (SSLv2/v3) **etkindir**  

### Yönetim veya kontrol arayüzleri yetkili ağlarla sınırlandırılmış mı?
- [ ] Evet — arayüzlere (örneğin SSH, RDP, kontrol panelleri) halka açık internetten **erişilemez**  
- [ ] Hayır — arayüzlere halka açık şekilde **erişilebilir** ancak çok faktörlü kimlik doğrulama (MFA) gerektirir  
- [ ] Hayır — arayüzlere tek faktörlü kimlik doğrulama veya varsayılan kimlik bilgileri (default credentials) ile halka açık şekilde **erişilebilir**

---

## WSTG-CONF-02 — Uygulama Platformu Yapılandırmasının Test Edilmesi

Uygulama platformu yapılandırma testi, yaygın Exploit girişimlerine karşı sıkılaştırıldığından emin olmak için temel web sunucusu, uygulama sunucusu ve framework ayarlarının denetlenmesini içerir. Saldırganlar, yetkisiz erişim elde etmek veya kod çalıştırmak için aktif olarak varsayılan kimlik bilgilerini, yamalanmamış platform zafiyetlerini ve açıkta kalan yönetim arayüzlerini ararlar. Yanlış yapılandırmalar genellikle hassas ortam değişkenlerinin, dahili sistem yollarının ifşa edilmesine veya saldırı yüzeyini genişleten gereksiz örnek uygulamaların varlığına neden olur. Profesyonel bir perspektiften bakıldığında, kötü yapılandırılmış bir platform, hedef ortama ilk erişimi (initial access) sağlamak için yüksek olasılıklı bir giriş noktası görevi görür.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CONF-02 |
| **CWE** | CWE-16 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek* |

> *Önem derecesi, yönetim arayüzlerine varsayılan kimlik bilgileriyle erişilebiliyorsa veya örnek betikler Remote Code Execution imkanı tanıyorsa Yüksek olarak kabul edilir.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/02-Test_Application_Platform_Configuration  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Nmap`, `Nikto`, `WhatWeb`, `Wappalyzer`, `Burp Suite`, `Curl`

### Platform üzerinde varsayılan kimlik bilgileri veya hesaplar aktif mi?
- [ ] Hayır — tüm varsayılan hesaplar **devre dışı bırakılmış** veya parolaları değiştirilmiştir  
- [ ] Evet — varsayılan hesaplar mevcuttur ancak dış ağdan **erişilebilir değildir**  
- [ ] Evet — varsayılan hesaplar **aktiftir** ve internet üzerinden erişilebilirdir *(Kritik)*  

### Platform, header'lar veya hata sayfaları aracılığıyla hassas sürüm bilgilerini ifşa ediyor mu?
- [ ] Hayır — sürüm dizinleri **gizlenmiştir** veya geneldir  
- [ ] Evet — detaylı sürüm bilgileri HTTP header'larında (örneğin `Server`, `X-Powered-By`) **ifşa edilmektedir**  
- [ ] Evet — ayrıntılı hata mesajlarında tam stack trace'ler ve dahili sistem yolları **açığa çıkmaktadır**  

### Yönetim veya idari arayüzler uygun şekilde kısıtlanmış mı?
- [ ] Hayır — yönetim arayüzleri **açıkta değildir**  
- [ ] Evet — arayüzler mevcuttur ancak IP allowlisting veya Multi-Factor Authentication ile **kısıtlanmıştır**  
- [ ] Evet — arayüzler (örneğin `/admin`, `/manager`, `/console`) tek faktörlü kimlik doğrulama ile **kamuya açıktır**  

### Production sunucusunda örnek dosyalar, betikler veya dokümantasyon bulunuyor mu?
- [ ] Hayır — gerekli olmayan tüm dosyalar **kaldırılmıştır**  
- [ ] Evet — örnek dosyalar veya dokümantasyon **mevcuttur** ancak hassas bilgi sızdırmamaktadır  
- [ ] Evet — örnek betikler (örneğin `info.php`, `examples/`) **mevcuttur** ve doğrudan bir saldırı vektörü sağlamaktadır  

### Sunucu, güvenli olmayan HTTP metotlarını destekleyecek şekilde mi yapılandırılmış?
- [ ] Hayır — sadece `GET` ve `POST` metotları **etkindir**  
- [ ] Evet — `PUT`, `DELETE` veya `TRACE` gibi potansiyel olarak riskli metotlar **etkindir** ancak kısıtlanmıştır  
- [ ] Evet — riskli metotlar **etkindir** ve yetkisiz dosya değişikliğine veya kimlik bilgisi hırsızlığına izin vermektedir

---

## WSTG-CONF-03 — Hassas Bilgiler İçin Dosya Uzantısı İşleme Testi

Dosya uzantısı işleme testi, web sunucusunun veya uygulama sunucusunun kısıtlanması veya yürütülmesi gereken dosyaları sunarak hassas bilgileri ifşa edip etmediğinin belirlenmesini içerir. Saldırganlar sıklıkla, web kök dizininde bırakılmış olabilecek ve belirli işleyici eşlemelerinin (handler mappings) eksikliği nedeniyle düz metin olarak sunulan yedek dosyaları, yapılandırma dosyaları ve kaynak kodu parçacıklarını (örneğin; `.bak`, `.old`, `.env`, `.inc`) araştırırlar. Bu zafiyet önemlidir çünkü veri tabanı kimlik bilgilerinin, API anahtarlarının ve dahili iş mantığının ifşa edilmesine yol açarak altyapının daha derinlemesine istismar edilmesini (Exploit) kolaylaştırabilir. Bu tür ifşalar genellikle halka açık dizinlerde, yükleme klasörlerinde veya sunucu üzerindeki hatalı yapılandırılmış MIME-type ayarları aracılığıyla gerçekleşir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CONF-03 |
| **CWE** | CWE-552 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek* |

> *Kimlik bilgilerini içeren yapılandırma dosyalarına veya tam kaynak koduna erişilebiliyorsa önem derecesi Yüksek olur.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/03-Test_File_Extensions_Handling_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `ffuf`, `gobuster`, `dirsearch`, `Burp Suite (Intruder)`, `Wfuzz`

### Hassas yedekleme veya geçici dosya uzantılarına (örneğin; `.bak`, `.old`, `.swp`, `~`) erişilebiliyor mu?
- [ ] Hayır — sunucu, yaygın yedekleme uzantıları için 403 Forbidden veya 404 Not Found döndürüyor  
- [ ] Evet — sunucu yedekleme dosyalarının indirilmesine izin veriyor ancak bunlar hassas veri **içermiyor**  
- [ ] Evet — sunucu, hassas veriler veya kaynak kodu içeren yedekleme dosyalarının indirilmesine izin veriyor *(Yüksek)*  

### Sunucu, ortam veya sistem yapılandırma dosyalarını (örneğin; `.env`, `.config`, `.yml`, `.ini`) ifşa ediyor mu?
- [ ] Hayır — kısıtlanmış yapılandırma dosyalarına **erişilemiyor** ve 403/404 döndürülüyor  
- [ ] Evet — hassas yapılandırma dosyalarına **erişilebiliyor** ve dahili sırlar veya kimlik bilgileri ifşa ediliyor  

### Alternatif uzantılar eklenerek (örneğin; `.php.txt`, `.jsp.old`, `.aspx.bak`) kaynak kodu elde edilebiliyor mu?
- [ ] Hayır — uzantı manipülasyonuna bakılmaksızın uygulama kodu düz metin olarak **işlenmiyor**  
- [ ] Evet — eklenen uzantı için sunucuda bir işleyici (handler) bulunmadığından kaynak kodu **ifşa ediliyor**  

### Yönetim veya meta veri dizinlerinin (örneğin; `.git/`, `.svn/`, `.DS_Store`) halka açık erişimi engellenmiş mi?
- [ ] Hayır — bu dizinler/dosyalar web kök dizininde **mevcut değil**  
- [ ] Evet — sunucu tarafı yeniden yazma kuralları (rewrite rules) veya izinler nedeniyle erişim **mümkün değil**  
- [ ] Evet — meta veri dizinlerine **erişilebiliyor** ve tam kaynak kodunun yeniden yapılandırılmasına olanak tanıyor  

### Sunucu, birden fazla uzantıya sahip dosya isteklerini (örneğin; `file.php.jpg`) nasıl ele alıyor?
- [ ] Hayır — sunucu, dahili uzantının güvenliğine doğru şekilde öncelik veriyor veya isteği engelliyor  
- [ ] Evet — sunucu dosyayı ilk uzantı olarak işliyor ancak yürütme için atlatma (Bypass) **mümkün değil**  
- [ ] Evet — sunucu son uzantıyı görmezden geliyor, kod yürütülmesine veya kaynak ifşasına izin verilmesi **mümkün**

---

## WSTG-CONF-04 — Hassas Bilgiler İçin Eski Yedek ve Referans Verilmemiş Dosyaların İncelenmesi

Eski yedeklerin ve referans verilmemiş dosyaların incelenmesi; bir web sunucusu üzerinde genel erişime açık olması amaçlanmayan unutulmuş, geçici veya gizli dosyaların tespit edilmesini kapsar. Bu dosyalar genellikle kaynak kodu yedeklerini (`.zip`, `.bak`), metin düzenleyici swap dosyalarını (`.swp`, `~`) veya hassas bilgi sızdırabilecek kaynak kontrol meta verilerini (`.git`, `.svn`) içerir. Saldırganlar; veritabanı kimlik bilgilerini, kod içine gömülmüş (hardcoded) API anahtarlarını veya daha ileri seviye istismarları (exploitation) kolaylaştıran mantıksal kurguları ele geçirmek amacıyla, yaygın isimlendirme kurallarına karşı otomatik kelime listeleri ve keşif araçları kullanarak Brute Force saldırıları düzenler. Bu zafiyet tipik olarak manuel sunucu bakımlarından veya canlı (production) ortamdaki web kök dizinini (web root) temizlemeyi başaramayan kusurlu CI/CD süreçlerinden kaynaklanır.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CONF-04 |
| **CWE** | CWE-530 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek* |

> *Konfigürasyon dosyaları, kaynak kod arşivleri veya kimlik bilgileri tespit edilirse önem derecesi Yüksek (High) olarak kabul edilir.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/04-Review_Old_Backup_and_Unreferenced_Files_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `ffuf`, `dirsearch`, `gobuster`, `GitTools`, `Burp Suite (Engagement Tools)`, `Wfuzz`

### Web sunucusunda dizin listeleme (directory listing) özelliği etkin mi?
- [ ] Hayır — dizin listeleme **devre dışıdır** ve 403 Forbidden veya özel bir hata sayfası döndürülmektedir  
- [ ] Evet — dizin listeleme hassas olmayan dizinlerde **etkindir**  
- [ ] Evet — dizin listeleme kaynak kodu veya hassas dosyalar içeren dizinlerde **etkindir** *(Kritik)*  

### Yaygın uzantılara sahip (.bak, .old, .save gibi) yedek dosyaları keşfedilebiliyor mu?
- [ ] Hayır — yaygın yedekleme uzantıları **bulunamadı** veya sunucu yapılandırması tarafından engellenmiş  
- [ ] Evet — yedek dosyaları mevcut ancak hassas bilgi **içermiyor**  
- [ ] Evet — yapılandırma veya kaynak koduna ait yedek dosyaları erişilebilir **durumdadır** *(Yüksek)*  

### Kaynak kontrol dizinleri (.git, .svn gibi) veya meta veriler açıkta mı?
- [ ] Hayır — kaynak kontrol meta verileri **mevcut değil** veya doğru şekilde engellenmiş  
- [ ] Evet — meta veriler mevcut ancak tüm depo (repository) yeniden **oluşturulamıyor**  
- [ ] Evet — tüm kaynak kodu deposu, açıkta kalan meta veriler aracılığıyla **sızdırılabilir**  

### Web kök dizininde (web root) uygulamanın sıkıştırılmış arşivleri (.zip, .tar.gz, .rar gibi) bulunuyor mu?
- [ ] Hayır — Brute Force veya numaralandırma (enumeration) yoluyla hassas arşiv dosyası tespit edilmedi  
- [ ] Evet — arşivler bulundu ancak parola korumalılar veya yalnızca halka açık varlıkları içeriyorlar  
- [ ] Evet — uygulama kaynağını veya verilerini içeren şifrelenmemiş arşivler halka açık şekilde **erişilebilirdir**  

### Uygulama, metin düzenleyiciler veya IDE'ler tarafından oluşturulan geçici dosyaları sızdırıyor mu?
- [ ] Hayır — `.swp`, `~` veya `.DS_Store` gibi geçici dosyalar **mevcut değil**  
- [ ] Evet — hassas olmayan geçici dosyalar **mevcut**  
- [ ] Evet — geçici dosyalar kaynak kodu parçacıklarını veya dahili yolları (internal paths) ifşa ediyor

---

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

## WSTG-CONF-06 — HTTP Metotlarını Test Etme

HTTP metotlarının test edilmesi, yalnızca gerekli işlevselliğin dışarıya açıldığından emin olmak için web sunucusu ve uygulama tarafından hangi fiillerin (verbs) desteklendiğinin belirlenmesini içerir. Standart `GET` ve `POST` metotlarının ötesinde, sunucular yanlışlıkla `PUT`, `DELETE` veya `TRACE` gibi tehlikeli metotları etkinleştirebilir; bu durum saldırganların kötü amaçlı dosyalar yüklemesine, mevcut içeriği silmesine veya Cross-Site Tracing (XST) aracılığıyla oturum çerezlerini sızdırmasına olanak tanıyabilir. Sızma testi uzmanları (Pentesters), `Allow` veya `Public` gibi yanıt başlıklarını inceler ve yetkisiz işlevlere erişmek için metot geçersiz kılma (method overriding) tekniklerini veya fiil kurcalama (verb tampering) yöntemlerini kullanarak kısıtlamaları atlatmaya çalışır. Bu test, saldırı yüzeyini sertleştirmek ve sunucunun ele geçirilmesine veya yetkisiz veri manipülasyonuna yol açabilecek hatalı yapılandırmaları önlemek için kritik öneme sahiptir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CONF-06 |
| **CWE** | CWE-650 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Düşük / Orta* |

> *`PUT` veya `DELETE` yetkisiz dosya manipülasyonuna izin veriyorsa veya `TRACE` oturum belirtecinin (session token) sızdırılmasını kolaylaştırıyorsa Önem Derecesi Yüksek (High) olur.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/06-Test_HTTP_Methods  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `curl`, `nmap`, `Burp Suite (Repeater)`, `ZAP`, `Metasploit`

### `PUT` veya `DELETE` gibi tehlikeli HTTP metotları uygulama genelinde devre dışı bırakılmış mı?
- [ ] Evet — yalnızca güvenli metotlar (GET, POST, HEAD) **etkindir**  
- [ ] Hayır — `PUT` veya `DELETE` **etkindir** ancak geçerli kimlik doğrulaması gerektirir  
- [ ] Hayır — `PUT` veya `DELETE` **etkindir** ve kimlik doğrulaması olmadan erişilebilirdir *(Kritik)*  

### Cross-Site Tracing (XST) saldırısını önlemek için `TRACE` metodu devre dışı bırakılmış mı?
- [ ] Evet — `TRACE` metodu **devre dışıdır** veya 405 Method Not Allowed döndürür  
- [ ] Hayır — `TRACE` metodu **etkindir** ancak hassas başlıkları yansıtmaz  
- [ ] Hayır — `TRACE` metodu **etkindir** ve `Cookie` veya `Authorization` başlıklarını yansıtır *(Orta)*  

### HTTP Metot Geçersiz Kılma (örneğin `X-HTTP-Method-Override`) sunucu tarafından destekleniyor mu?
- [ ] Hayır — sunucu metot geçersiz kılma başlıklarını **işlemez**  
- [ ] Evet — sunucu geçersiz kılma başlıklarını işler ancak erişim kontrolleri **uygulanır**  
- [ ] Evet — sunucu geçersiz kılma başlıklarını işler ve erişim kontrolü atlatma (bypass) **mümkündür**  

### Sunucu, rastgele veya hatalı biçimlendirilmiş HTTP metotlarına güvenli bir şekilde yanıt veriyor mu?
- [ ] Evet — sunucu 405 Method Not Allowed veya 501 Not Implemented döndürür  
- [ ] Hayır — sunucu `JEFF` veya `TEST` gibi rastgele metotlar için 200 OK veya 500 Error döndürür  
- [ ] Hayır — rastgele metotların kullanılması, kimlik doğrulama veya yetkilendirme filtrelerinin atlatılmasına olanak tanır  

### `OPTIONS` metodu, bilgi ifşasını en aza indirecek şekilde kısıtlanmış veya yapılandırılmış mı?
- [ ] Evet — `OPTIONS` devre dışıdır veya minimal bir `Allow` başlığı döndürür  
- [ ] Hayır — `OPTIONS`, DAV veya hata ayıklama (debugging) fiilleri de dahil olmak üzere etkinleştirilmiş çok çeşitli metotları açığa çıkarır

---

## WSTG-CONF-07 — HTTP Strict Transport Security Testi

HTTP Strict Transport Security (HSTS), bir web sunucusunun tarayıcılara kendisine yalnızca HTTPS üzerinden erişilmesi gerektiğini bildirmesini sağlayan; protokol düşürme saldırılarını (protocol downgrade attacks) ve çerez ele geçirmeyi (cookie hijacking) engelleyen bir güvenlik mekanizmasıdır. Şifrelenmiş bir bağlantıyı zorunlu kılarak HSTS, bir saldırganın TLS'e geçişi önlemek için başlangıçtaki güvensiz bir HTTP isteğine müdahale ettiği SSLStrip gibi Aradaki Adam (Man-in-the-Middle - MitM) saldırılarını azaltır. Saldırgan açısından bakıldığında, bu başlığın eksikliği veya kısa bir `max-age` süresi, ilk açık metin (cleartext) el sıkışması (handshake) sırasında hassas oturum belirteçlerini (session tokens) veya kimlik bilgilerini ele geçirmek için bir fırsat penceresi sunar. Bu test, tüm hassas uygulama uç noktalarında `Strict-Transport-Security` başlığının varlığını, süresini ve kapsamını doğrulamaya odaklanır.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CONF-07 |
| **CWE** | CWE-319 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Düşük / Orta* |

> *Uygulama; PII (Kişisel Veri), oturum belirteçleri veya kimlik bilgilerini işliyorsa, HSTS eksikliği MitM saldırılarının başarı oranını artırdığından önem derecesi Orta (Medium) olur.

**Kaynaklar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/07-Test_HTTP_Strict_Transport_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `curl`, `Burp Suite`, `nmap`, `SSLLabs`, `hsts-preload-checker`

### `Strict-Transport-Security` başlığı HTTPS yanıtlarında mevcut mu?
- [ ] Evet — başlık tüm hassas uç noktalarda **mevcut**  
- [ ] Evet — başlık **mevcut** ancak yalnızca giriş sayfasında (landing page)  
- [ ] Hayır — başlık uygulamada tamamen **eksik**  

### `max-age` direktifi yeterli bir süreye ayarlanmış mı?
- [ ] Evet — `max-age` bir yıl veya daha fazlasına ayarlanmış (örneğin, `31536000`)  
- [ ] Evet — `max-age` ayarlanmış ancak **çok kısa** (örneğin, 6 aydan az)  
- [ ] Hayır — `max-age` direktifi **eksik** veya `0` olarak ayarlanmış *(Kritik)*  

### HSTS politikası tüm alt alan adlarını (subdomains) kapsıyor mu?
- [ ] Evet — `includeSubDomains` direktifi **uygulanmış**  
- [ ] Hayır — `includeSubDomains` direktifi **eksik**, alt alan adları protokol düşürmeye karşı savunmasız bırakılmış  
- [ ] Hayır — alt alan adı bulunmadığından bu özellik geçerli değil  

### Uygulama HSTS Ön Yükleme (Preloading) için kayıtlı mı?
- [ ] Evet — `preload` direktifi **mevcut** ve alan adı HSTS ön yükleme listesinde  
- [ ] Evet — `preload` direktifi **mevcut** ancak alan adı henüz ön yükleme listesinde **değil**  
- [ ] Hayır — `preload` direktifi **eksik**, ilk ziyarette MitM saldırısı için bir açık kapı bırakıyor  

### HSTS başlığı hatalı bir şekilde açık metin HTTP üzerinden mi gönderiliyor?
- [ ] Hayır — başlık **yalnızca** güvenli HTTPS bağlantıları üzerinden gönderiliyor  
- [ ] Evet — başlık HTTP üzerinden **gönderiliyor**; bu durum tarayıcılar tarafından yok sayılır ve yapılandırma ayrıntılarını sızdırabilir

---

## WSTG-CONF-08 — RIA Çapraz Etki Alanı Politikasının Test Edilmesi (Test RIA Cross Domain Policy)

Zengin İnternet Uygulaması (Rich Internet Application - RIA) çapraz etki alanı politikaları, Adobe Flash ve Microsoft Silverlight gibi web istemcilerinin çapraz kökenli istekleri (cross-origin requests) nasıl yönettiğini tanımlayan `crossdomain.xml` ve `clientaccesspolicy.xml` gibi dosyaları içerir. Bu dosyalar güvenlik açısından kritiktir; çünkü Aynı Köken Politikasını (Same-Origin Policy - SOP) açıkça geçersiz kılarak hangi harici etki alanlarının (domains) uygulama ile etkileşime girmesine ve yanıtlarını okumasına izin verileceğini belirler. `domain` özniteliğinde joker karakterler (wildcards) kullanılması gibi aşırı izin verici yapılandırmalar, saldırganların üçüncü taraf bir sitede zararlı RIA nesneleri barındırmasına olanak tanır. Bu nesneler hassas verileri sızdırabilir (exfiltrate) veya kimliği doğrulanmış kullanıcılar adına işlemler gerçekleştirebilir. Sızma testi uzmanları (pentesters), genellikle üçüncü taraf kökenlere verilen güven düzeyini değerlendirmek ve olası çapraz kökenli veri sızıntılarını tespit etmek için bu dosyaları web kök dizininde (web root) ararlar.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CONF-08 |
| **CWE** | CWE-942 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek* |

> *Eğer uygulama, geniş izinli bir politika aracılığıyla sızdırılabilecek hassas kullanıcı verilerini veya oturum tanımlayıcılarını (session identifiers) işliyorsa, Önem Derecesi Yüksek olur.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/08-Test_RIA_Cross_Domain_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `curl`, `Burp Suite`, `FFUF`, `dirsearch`, `Wget`

### Çapraz etki alanı politikası dosyaları (`crossdomain.xml` veya `clientaccesspolicy.xml`) web kök dizininde mevcut mu?
- [ ] Hayır — dosyalar kök dizinde **bulunamadı**  
- [ ] Evet — `crossdomain.xml` (Flash) **mevcut**  
- [ ] Evet — `clientaccesspolicy.xml` (Silverlight) **mevcut**  
- [ ] Evet — her iki RIA politika dosyası da **mevcut**  

### `allow-access-from` etki alanı özniteliği güvenilir kökenlerle sınırlandırılmış mı?
- [ ] Hayır — dosyalar mevcut ancak herhangi bir `allow-access-from` girişi içermiyor  
- [ ] Evet — belirli ve güvenilir etki alanları açıkça izinli listesine (whitelist) eklenmiş ve atlatma **mümkün değil**  
- [ ] Evet — etki alanları izinli listesine eklenmiş ancak güvenilmeyen üçüncü tarafları veya alt etki alanlarını içeriyor  
- [ ] Evet — bir joker karakter `*` **kullanılmış**, bu da **herhangi bir** kökenden erişime izin veriyor *(Kritik)*  

### Politika, HTTPS kaynakları için HTTP üzerinden güvensiz iletişime izin veriyor mu?
- [ ] Hayır — `secure="true"` ayarlanmış (veya varsayılan olarak bırakılmış), bu da HTTP kökenlerinin HTTPS verilerine erişmesini engelliyor  
- [ ] Evet — `secure="false"` açıkça ayarlanmış, bu da MITM (Man-in-the-Middle) saldırganlarının güvenli verileri sızdırmasına izin veriyor  

### HTTP başlıkları çapraz etki alanı yapılandırmasında aşırı izin verici mi?
- [ ] Hayır — `allow-http-request-headers-from` kısıtlanmış veya **etkinleştirilmemiş**  
- [ ] Evet — belirli başlıklara (headers) güvenilir etki alanları için izin verilmiş  
- [ ] Evet — başlıklar için joker karakter `*` kullanılmış, bu da saldırganların herhangi bir etki alanından özel başlıklar göndermesine izin veriyor  

### `site-control` (master-policy) yapılandırması kısıtlayıcı mı?
- [ ] Hayır — `permitted-cross-domain-policies` değeri `none` veya `master-only` olarak ayarlanmış  
- [ ] Evet — politika, sunucudaki diğer dosyaların kendi kurallarını tanımlamasına izin veriyor (`all`)

---

## WSTG-CONF-09 — Dosya İzinlerinin Test Edilmesi

Dosya izinlerinin test edilmesi, hassas kaynakların yetkisiz kullanıcılara veya süreçlere aşırı derecede ifşa edilmediğinden emin olmak amacıyla web sunucusundaki dosya ve dizinlere atanan erişim kontrol seviyelerinin denetlenmesini içerir. Yanlış yapılandırılmış izinler; saldırganların veritabanı kimlik bilgilerini içeren hassas yapılandırma dosyalarını okumasına, tescilli kaynak kodlarını görüntülemesine veya Remote Code Execution (Uzaktan Kod Çalıştırma) gerçekleştirmek için sunucu tarafı betikleri değiştirmesine olanak tanıyabilir. Bu zafiyet tipik olarak; yapılandırma yolları, günlük (log) klasörleri veya yükleme depoları gibi kritik uygulama dizinlerinde varsayılan "world-readable" (herkes tarafından okunabilir) veya "world-writable" (herkes tarafından yazılabilir) bayraklarının bırakıldığı dağıtım aşamasında ortaya çıkar. Bir saldırganın perspektifinden, hatalı şekilde güvenliği sağlanmış bir dosyanın keşfedilmesi, genellikle Privilege Escalation (Yetki Yükseltme) veya tam sistem ele geçirme için birincil tetikleyici görevi görür.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CONF-09 |
| **CWE** | CWE-732 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek* |

> *Hassas yapılandırma dosyaları (örneğin `.env`, `web.config`) okunabilir durumdaysa veya web kök dizinine (web root) yazma erişimi mümkünse Önem Derecesi "Yüksek" olarak değerlendirilir.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/09-Test_File_Permission  
* https://hacktricks.wiki/en/linux-hardening/privilege-escalation/index.html  

**Araçlar:** `ls`, `find`, `LinPeas`, `WinPeas`, `ffuf`, `dirsearch`, `Nmap`

### Hassas uygulama yapılandırma dosyaları yetkisiz erişime karşı korunuyor mu?
- [ ] Evet — yapılandırma dosyalarına (örneğin `.env`, `config.php`) web istekleri aracılığıyla **erişilemiyor**  
- [ ] Evet — dosyalar mevcut ancak erişim yalnızca yetkili yerel kullanıcılarla **sınırlandırılmış**  
- [ ] Hayır — hassas yapılandırma dosyalarına **erişilebiliyor** ve kimlik bilgileri veya sırlar ifşa ediliyor  

### Bir saldırgan web kök dizini (web root) içindeki dosya veya dizinleri değiştirebilir mi?
- [ ] Hayır — tüm web kök dizinleri web sunucusu kullanıcısı için **salt okunurdur (read-only)**  
- [ ] Evet — yazılabilir dizinler mevcut ancak betik yürütme (script execution) **devre dışı bırakılmış**  
- [ ] Evet — "world-writable" dizinler mevcut ve betiklerin yüklenmesine ve yürütülmesine **izin veriyor** *(Kritik)*  

### Gevşek izinler nedeniyle sürüm kontrol meta verileri veya yedek dosyaları ifşa ediliyor mu?
- [ ] Hayır — `.git`, `.svn` ve yedek dosyaları (örneğin `.bak`, `~`) **mevcut değil** veya korunuyor  
- [ ] Evet — meta veri dizinleri mevcut ancak dizin listeleme (directory listing) **devre dışı**  
- [ ] Evet — hassas meta veriler veya yedekler **tamamen erişilebilir** durumda, bu da kaynak kodun elde edilmesine olanak tanıyor  

### Web sunucusu kullanıcısı en düşük yetki prensibi (principle of least privilege) ile mi çalışıyor?
- [ ] Evet — web sunucusu süreci **özel bir düşük yetkili** kullanıcı olarak çalışıyor  
- [ ] Hayır — web sunucusu süreci **gereksiz yetkilerle** (örneğin `root` veya `Administrator`) çalışıyor  

### Günlük (log) dosyaları yetkisiz okuma veya değiştirmeye karşı güvenli mi?
- [ ] Evet — günlük dosyaları idari kullanıcılarla **sınırlandırılmış**  
- [ ] Evet — günlük dosyaları okunabilir ancak oturum belirteçleri (session tokens) veya Kişisel Veriler (PII) **içermiyor**  
- [ ] Hayır — günlük dosyaları **herkese açık şekilde okunabiliyor** ve hassas işlem verilerini veya kullanıcı bilgilerini ifşa ediyor

---

## WSTG-CONF-10 — Alt Alan Adı Ele Geçirme (Subdomain Takeover)

Alt alan adı ele geçirme (Subdomain Takeover), bir DNS kaydının (tipik olarak CNAME, ancak bazen A veya MX kayıtları) üçüncü taraf bir bulut sağlayıcısı veya barındırma hizmetindeki kullanımdan kaldırılmış veya mevcut olmayan bir kaynağa işaret etmesiyle meydana gelir. Saldırganlar, AWS, GitHub veya Azure gibi sağlayıcılardan gelen ve bir kaynağın artık sahiplenilmediğini belirten belirli hata mesajlarını arayarak bu "boşta kalan" (dangling) DNS kayıtlarını tespit ederler. Saldırgan, terk edilmiş kaynak adını sağlayıcının platformunda kendi adına kaydederek alt alan adı üzerinde tam kontrol sahibi olur; bu da kötü amaçlı içerik barındırmasına, ana alan adı kapsamında tanımlanmış oturum çerezlerini sızdırmasına (exfiltrate) ve İçerik Güvenlik Politikası (CSP) veya CORS korumalarını atlatmasına olanak tanır. Bu güvenlik açığı, yüksek etkili oltalama (phishing) ve veri hırsızlığını kolaylaştırmak için kuruluşun meşru alan adı yapısıyla ilişkili güvenden yararlandığı için özellikle tehlikelidir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CONF-10 |
| **CWE** | CWE-1329 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Kritik* |

> *Alt alan adı, ana alan adından oturum çerezlerini çalmak veya SSO/OAuth yönlendirmelerini atlatmak için kullanılabiliyorsa Önem Derecesi Kritik hale gelir.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/10-Test_for_Subdomain_Takeover  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://github.com/EdOverflow/can-i-take-over-xyz  

**Araçlar:** `Subjack`, `SubOver`, `dnscan`, `Amass`, `Nuclei`, `dig`, `nslookup`

### Uygulama, alt alan adları aracılığıyla üçüncü taraf barındırma veya bulut hizmetlerinden yararlanıyor mu?
- [ ] Hayır — tüm alt alan adları kuruluş tarafından kontrol edilen IP alanına işaret ediyor  
- [ ] Evet — alt alan adları üçüncü taraf sağlayıcılara (S3, GitHub Pages, Heroku, vb.) işaret ediyor  

### Mevcut olmayan kaynaklara işaret eden herhangi bir "boşta kalan" (dangling) DNS kaydı var mı?
- [ ] Hayır — tespit edilen tüm DNS kayıtları aktif ve kontrol edilen kaynaklara çözümleniyor  
- [ ] Evet — sağlayıcı tarafında **artık mevcut olmayan** kaynaklar için CNAME veya A kayıtları mevcut  
- [ ] Evet — DNS kayıtları NXDOMAIN veya sağlayıcıya özgü hata sayfalarına çözümleniyor *(örneğin, "NoSuchBucket")*  

### Tespit edilen boşta kalan kaynak başarıyla sahiplenilebilir mi?
- [ ] Hayır — sağlayıcının terk edilmiş adların sahiplenilmesine karşı korumaları var veya hesap doğrulaması gerekiyor  
- [ ] Evet — kaynak adı, herhangi bir kullanıcı tarafından üçüncü taraf platformunda **kaydedilebilir**  

### Ana alan adı, güvenliği ihlal edilebilecek "Domain" kapsamlı çerezler kullanıyor mu?
- [ ] Hayır — çerezler kesinlikle belirli alt alan adları ile sınırlandırılmıştır veya `Host-Only` bayraklarını kullanır  
- [ ] Evet — hassas çerezler (oturum kimlikleri, JWT'ler) üst alan adı kapsamında tanımlanmıştır ve ele geçirilmiş bir alt alan adı aracılığıyla **sızdırılabilir**  

### Alt alan adı, güvenlik başlıklarını veya kimlik doğrulama akışlarını atlatmak için kullanılabilir mi?
- [ ] Hayır — tespit edilen alt alan adlarına yönelik herhangi bir güvenlik bağımlılığı bulunmuyor  
- [ ] Evet — alt alan adı, CSP `script-src` veya `connect-src` direktiflerinde beyaz listeye eklenmiş  
- [ ] Evet — alt alan adı, OAuth veya SSO yapılandırmaları için geçerli bir yönlendirme URI'sidir (redirect URI)

---

## WSTG-CONF-11 — Bulut Depolama Testi

Bulut depolama hatalı yapılandırmalarına (misconfigurations) yönelik testler; Amazon S3, Azure Blobs veya Google Cloud Storage gibi genel erişime açık nesne depolama servislerinin (object storage services) tanımlanmasını ve denetlenmesini içerir. Bu kaynaklar genellikle aşırı izinli Erişim Kontrol Listeleri (ACLs) veya Kimlik ve Erişim Yönetimi (IAM) politikaları nedeniyle sızdırılan; yedeklemeler, kaynak kodlar, Kişisel Veriler (PII) veya yapılandırma dosyaları gibi hassas veriler içerir. Saldırganlar, veri sızdırma (data exfiltration) veya yetkisiz dosya değişikliği için giriş noktaları bulmak amacıyla JavaScript dosyaları, HTML kaynak kodları ve Brute Force teknikleri aracılığıyla keşif yaparak bucket isimlerini numaralandırır (enumerate). İstismar (Exploit) süreci, tam veri ihlalleri veya güvenilir bir alan adından zararlı dosyaların servis edilmesi dahil olmak üzere önemli etkilere yol açabilir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CONF-11 |
| **CWE** | CWE-922 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Kritik* |

> *PII, kimlik bilgileri veya üretim yedekleri erişilebilirse ya da kimlik doğrulaması gerektirmeyen yazma erişimi etkinse önem derecesi Kritik olur.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/11-Test_Cloud_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `aws-cli`, `gsutil`, `azure-cli`, `S3Scanner`, `CloudEnum`, `FFUF`, `Burp Suite`

### Bulut depolama uç noktaları (buckets/containers) tanımlanmış ve numaralandırılmış mı?
- [ ] Hayır — herhangi bir bulut depolama uç noktası kullanılmıyor veya keşfedilemiyor  
- [ ] Evet — depolama uç noktaları kod analizi veya trafik izleme yoluyla tanımlandı  
- [ ] Evet — depolama uç noktaları adlandırma kuralı (naming convention) brute-force yöntemiyle keşfedildi  

### Kimlik doğrulaması yapılmamış kullanıcılar bulut depolama içeriğini listeleyebiliyor mu?
- [ ] Hayır — bucket listeleme **devre dışı** ve 403 Forbidden veya 404 Not Found döndürüyor  
- [ ] Evet — listeleme **etkin** ancak hassas dosya bulunmuyor  
- [ ] Evet — listeleme **etkin** ve hassas dosya isimleri/metadataları görünüyor  

### Tanımlanan bucket'lardan hassas dosyaların indirilmesi mümkün mü?
- [ ] Hayır — dosyalar IAM politikaları ile korunuyor veya geçerli bir kimlik doğrulaması gerekiyor  
- [ ] Evet — dosyalara erişilebiliyor ancak kolayca tahmin edilemeyen imzalı bir URL (signed URL) gerekiyor  
- [ ] Evet — hassas dosyalar (yedeklemeler, `.env`, PII) indirme için **genel erişime açık** *(Kritik)*  

### Kimlik doğrulaması yapılmamış dosya yükleme veya değişikliğine izin veriliyor mu?
- [ ] Hayır — kimlik doğrulaması yapılmamış yüklemeler veya değişiklikler **mümkün değil**  
- [ ] Evet — kimlik doğrulamalı yükleme gerekiyor ancak zayıf politika koşulları nedeniyle **atlatma (bypass) mümkün**  
- [ ] Evet — dosyaların kimlik doğrulaması olmadan yazılması, üzerine yazılması veya silinmesi **mümkün** *(Kritik)*  

### Depolama uç noktasındaki Cross-Origin Resource Sharing (CORS) politikaları kısıtlayıcı mı?
- [ ] Evet — CORS **devre dışı** veya belirli güvenilir kaynaklarla (origins) kısıtlanmış  
- [ ] Hayır — CORS, bir joker karakter `*` kaynağı ile **etkinleştirilmiş**, bu da yetkisiz web tabanlı erişime izin veriyor

---

## WSTG-CONF-12 — Content Security Policy (CSP)

İçerik Güvenliği Politikası (Content Security Policy - CSP), Cross-Site Scripting (XSS), clickjacking ve veri enjeksiyonu saldırıları gibi riskleri azaltmak amacıyla HTTP yanıt başlıkları aracılığıyla uygulanan kritik bir derinlemesine savunma (defense-in-depth) mekanizmasıdır. Hangi dinamik kaynakların yüklenmesine izin verileceğini tanımlayarak, iyi yapılandırılmış bir CSP, bir saldırganın yetkisiz script çalıştırma veya hassas verileri harici domainlere sızdırma (exfiltrate) yeteneğini kısıtlar. Pentesterlar, politikayı aşırı izin verici direktifler, `'unsafe-inline'` gibi güvenli olmayan anahtar kelimelerin kullanımı ve atlatmaları (bypass) kolaylaştırabilecek güvensiz CDN'lere güvenme açısından değerlendirir. Zayıf veya eksik bir CSP doğrudan bir zafiyet oluşturmaz, ancak ikincil bir koruma katmanı sağlamayarak başarılı enjeksiyon saldırılarının etkisini önemli ölçüde artırır.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CONF-12 |
| **CWE** | CWE-693 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Düşük / Orta |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/12-Test_for_Content_Security_Policy  
* https://hacktricks.wiki/en/pentesting-web/content-security-policy-csp-bypass/index.html  

**Araçlar:** `Google CSP Evaluator`, `Burp Suite (CSP Pro)`, `Mozilla Observatory`, `ZAP`, `CSP Mitigator`

### Uygulama yanıtlarında bir Content Security Policy (CSP) başlığı mevcut mu?
- [ ] Evet — `Content-Security-Policy` başlığı **mevcut** ve **zorunlu kılınmış (enforced)**  
- [ ] Evet — Test amaçlı `Content-Security-Policy-Report-Only` **mevcut**  
- [ ] Hayır — Yanıtlarda herhangi bir CSP başlığı **mevcut değil**  

### `script-src` veya `default-src` direktifleri uygun şekilde kısıtlanmış mı?
- [ ] Evet — Direktifler katı beyaz listeye alma (whitelisting), nonce'lar veya hash'ler kullanıyor ve atlatma (bypass) **mümkün değil**  
- [ ] Evet — Direktifler **uygun şekilde yapılandırılmış** ancak beyaz liste, bilinen ve atlatılabilir CDN'leri içeriyor  
- [ ] Hayır — Direktifler joker karakterler (`*`) veya `data:` şemaları kullanıyor, bu da atlatmayı (bypass) **mümkün kılıyor**  

### Politika, güvenli olmayan inline scriptlerin veya eval fonksiyonunun çalıştırılmasına izin veriyor mu?
- [ ] Hayır — `'unsafe-inline'` ve `'unsafe-eval'` **mevcut değil**  
- [ ] Evet — `'unsafe-inline'` **mevcut** ancak bir `nonce` veya `hash` ile korunuyor  
- [ ] Evet — `'unsafe-inline'` veya `'unsafe-eval'` ek koruma olmaksızın **etkinleştirilmiş** *(Kritik)*  

### Uygulama, `frame-ancestors` direktifi aracılığıyla clickjacking'e karşı korunuyor mu?
- [ ] Evet — `frame-ancestors` değeri `'none'` veya `'self'` olarak ayarlanmış  
- [ ] Evet — `frame-ancestors` **etkinleştirilmiş** ancak belirli güvenilir üçüncü taraf domainlere izin veriyor  
- [ ] Hayır — `frame-ancestors` **eksik**; eski `X-Frame-Options` başlığına güveniliyor veya hiç koruma yok  

### CSP ihlallerini raporlamak için bir mekanizma var mı?
- [ ] Evet — `report-uri` veya `report-to` **yapılandırılmış** ve aktif  
- [ ] Hayır — İhlal raporlaması **devre dışı** veya **yapılandırılmamış**

---

## WSTG-CONF-13 — Yol Karışıklığı (Path Confusion)

Yol Karışıklığı (Path Confusion), ters vekil sunucular (reverse proxies), yük dengeleyiciler (load balancers) ve arka uç uygulama sunucuları (backend application servers) gibi farklı web bileşenlerinin URL yollarını ayrıştırma ve yorumlama biçimlerindeki tutarsızlıklardan kaynaklanır. Saldırganlar, güvenlik filtrelerini aldatmak ve kısıtlanmış uç noktalara (endpoints) veya statik kaynaklara erişmek için noktalı virgül, kodlanmış eğik çizgiler veya nokta segmentleri gibi belirli karakterleri enjekte ederek bu tutarsızlıkları istismar ederler. Ön uç (front-end) sistemin, arka ucun (back-end) farklı yorumladığı bir yola güvenlik kuralları uygulaması nedeniyle; bu güvenlik açığı kimlik doğrulama atlatma (authentication bypass), yetkisiz veri erişimi ve web önbellek zehirlenmesi (web cache poisoning) dahil olmak üzere kritik güvenlik ihlallerine yol açabilir. Başarılı bir istismar genellikle istek yönlendirme (request routing) ve normalleştirme (normalization) mantığının teknoloji yığını genelinde farklılaştığı mimari sınırlarda meydana gelir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CONF-13 |
| **CWE** | CWE-444 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek* |

> *Yol karışıklığı, hassas yönetim uç noktaları için kimlik doğrulama veya erişim kontrolünün atlatılmasıyla sonuçlanırsa önem derecesi "Yüksek" olur.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/13-Test_for_Path_Confusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite`, `Dirsearch`, `FFUF`, `ParamMiner`, `Arjun`

### Farklı mimari katmanlar yol ayırıcılarını (örneğin; `;`, `#`, `?`) tutarlı bir şekilde yorumluyor mu?
- [ ] Evet — tüm katmanlar yol ayırıcılarını aynı şekilde normalleştirir ve yorumlar  
- [ ] Hayır — tutarsızlıklar mevcuttur ancak kısıtlanmış kaynaklara erişmek için **kullanılamaz**  
- [ ] Hayır — farklılıklar, bir saldırganın ön uç filtrelerini atlatmasına ve arka uç mantığına ulaşmasına **izin vermektedir**  

### Erişim kontrolleri, yol aşımı (path traversal) dizileri veya kodlanmış karakterler kullanılarak atlatılabilir mi?
- [ ] Hayır — güvenlik kontrollerinden önce normalleştirme tutarlı bir şekilde uygulanmaktadır  
- [ ] Evet — nokta segmenti dizileri (örneğin; `/admin/..;/`) aracılığıyla atlatma **mümkündür**  
- [ ] Evet — URL kodlamalı karakterler (örneğin; `%2f`, `%2e%2e%2f`) aracılığıyla atlatma **mümkündür**  

### Uygulama, yol karışıklığı yoluyla Web Önbellek Zehirlenmesine (Web Cache Poisoning) karşı savunmasız mı?
- [ ] Hayır — önbelleğe alma mantığı, yol belirsizliğinden veya fazladan yol bilgisinden etkilenmez  
- [ ] Evet — eşleştirme tutarsızlıkları nedeniyle kötü niyetli içerik meşru yollar için önbelleğe **alınabilir**  
- [ ] Evet — hassas bilgiler, `RCD` (Göreceli Yol Üzerine Yazma - Relative Path Overwrite) teknikleri aracılığıyla halka açık dizinlerde önbelleğe **alınabilir**  

### Sunucu "ek yol bilgilerini" (PathInfo) güvenli bir şekilde işliyor mu?
- [ ] Hayır — özellik **devre dışı bırakılmıştır** veya mevcut değildir  
- [ ] Evet — `PathInfo` işlenmektedir ve güvenlik filtrelerine müdahale **etmemektedir**  
- [ ] Hayır — `PathInfo`, bir saldırganın uygulamanın yönlendirme mantığını karıştıran veriler eklemesine **izin vermektedir**

---

## WSTG-CONF-14 — Diğer HTTP Güvenlik Başlığı Yanlış Yapılandırmalarının Test Edilmesi

HTTP güvenlik başlığı yanlış yapılandırmalarının test edilmesi, yaygın web tabanlı saldırı vektörlerine karşı temel koruma sağlayan savunma başlıklarının varlığının ve doğru uygulandığının doğrulanmasını içerir. Bu başlıklar temel kod kusurlarını düzeltmese de, eksiklikleri; Ortadaki Adam (Man-in-the-Middle - MITM) saldırıları, MIME-sniffing (MIME türü koklama) istismarı ve yönlendiriciler (referrers) aracılığıyla istenmeyen kaynaklar arası veri sızıntısı riskini önemli ölçüde artırır. Bir saldırganın perspektifinden bakıldığında, eksik güvenlik başlıkları, kötü sıkılaştırılmış bir ortamın yüksek görünürlüklü göstergeleridir ve genellikle oturum çalma (session hijacking) veya bilgi sızdırma (information exfiltration) gibi daha karmaşık istismar zincirlerini kolaylaştırır. Bu yapılandırmalar genellikle sunucu düzeyinde değerlendirilir ve API uç noktaları ile statik kaynaklar dahil olmak üzere tüm yanıt türlerinde tutarlı bir şekilde uygulanmalıdır.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CONF-14 |
| **CWE** | CWE-16 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Düşük / Orta |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/14-Test_Other_HTTP_Security_Header_Misconfigurations  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `curl`, `Burp Suite (Repeater/Scanner)`, `OWASP ZAP`, `Hardenize`, `securityheaders.com`

### Güvenlik Başlığı Varlık Denetimi
| Başlık | Mevcut | Eksik | Önerilen Yapılandırma |
|--------|:-------:|:-------:|---------------------------|
| `Strict-Transport-Security` |  |  | `max-age=63072000; includeSubDomains; preload` |
| `X-Content-Type-Options` |  |  | `nosniff` |
| `Referrer-Policy` |  |  | `no-referrer` veya `strict-origin-when-cross-origin` |
| `Permissions-Policy` |  |  | (Özelliğe özgü, örn. `geolocation=()`) |
| `Cross-Origin-Opener-Policy` |  |  | `same-origin` |

### Güvenlik başlıkları uygulama kapsamında nasıl zorunlu kılınmaktadır?
- [ ] Evet — başlıklar merkezi bir yük dengeleyici (load balancer) veya ters vekil sunucu (reverse proxy) aracılığıyla global olarak uygulanır ve **geçersiz kılınamaz**  
- [ ] Evet — başlıklar ana doküman yanıtlarına uygulanır ancak API yanıtlarında veya statik varlıklarda **eksiktir**  
- [ ] Hayır — başlıklar tutarsız uygulanmıştır veya tüm uygulama alan adında **eksiktir**  

### Strict-Transport-Security (HSTS) başlığı doğru yapılandırılmış mı?
- [ ] Evet — `max-age` uzun bir süreye (örn. 2 yıl) ayarlanmış ve `includeSubDomains` **etkinleştirilmiştir**  
- [ ] Evet — `max-age` ayarlanmış ancak `includeSubDomains` **devre dışı bırakılmıştır**, bu durum alt alan adlarını savunmasız bırakır  
- [ ] Hayır — HSTS başlığı **mevcut değil** veya `max-age` 0 olarak ayarlanmış; bu durum protokol düşürme (protocol downgrade) saldırılarına izin verir  

### Referrer-Policy hassas URL parametrelerinin sızmasını engelliyor mu?
- [ ] Evet — politika `no-referrer` veya `same-origin` olarak ayarlanmış *(En güvenli)*  
- [ ] Evet — politika `strict-origin-when-cross-origin` olarak ayarlanmış  
- [ ] Hayır — politika **ayarlanmamış** veya `unsafe-url` olarak ayarlanmış; bu durum referer başlıklarında token'ların veya hassas Kişisel Olarak Tanımlanabilir Bilgilerin (PII) sızmasına neden olabilir  

### X-Content-Type-Options başlığı MIME-sniffing'i engelliyor mu?
- [ ] Evet — `nosniff` direktifi **mevcuttur** ve doğru şekilde uygulanmıştır  
- [ ] Hayır — başlık **eksiktir**; tarayıcının yürütülebilir olmayan dosyaları (örn. resimler) betik (script) olarak çalıştırmasına potansiyel olarak izin verir

---

## WSTG-IDNT-01 — Rol Tanımlarının Test Edilmesi

Rol tanımlarının test edilmesi, uygulama içinde tanımlanan çeşitli kullanıcı rollerinin ve yetki seviyelerinin belirlenmesini ve belgelenmesini kapsar; bu, rollerin mantıksal olarak ayrılmasını ve en az ayrıcalık (least privilege) ilkesine bağlı kalınmasını sağlamak amacıyla yapılır. Bir saldırgan, yetki sınırları arasındaki belirsizlikleri veya belgelenmemiş yönetici işlevlerini arayarak yüksek ayrıcalıklı rolleri standart kullanıcı rollerine karşı haritalamak (map) için uygulamayı analiz eder. Bu rollerin tanımlanması, yetki yükseltme (privilege escalation) yollarını keşfetmenin ilk adımıdır; çünkü rol tanımlarındaki tutarsızlıklar genellikle hassas verilere veya yönetim özelliklerine yetkisiz erişime yol açar. Bu aşama tipik olarak ilk keşif ve iş mantığı haritalama (business logic mapping) sırasında gerçekleşir; kullanıcı profillerine, yönetici panellerine ve API (Application Programming Interface) yetkilendirme yapılarına odaklanır.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-IDNT-01 |
| **CWE** | CWE-285 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Düşük / Orta |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/01-Test_Role_Definitions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite (Compare Site Maps)`, `Postman`, `AuthMatrix`, `Authz`, `Browser Developer Tools`

### Roller, uygulamada veya uygulama belgelerinde açıkça tanımlanmış ve belgelenmiş mi?
- [ ] Evet — roller tamamen belgelenmiştir ve **en az ayrıcalık** (least privilege) ilkesine uygundur  
- [ ] Evet — roller belgelenmiştir ancak **çakışan izinler** içermektedir  
- [ ] Hayır — resmi bir rol belgelendirmesi veya tanımı mevcut değildir  

### Yönetici işlevleri ile standart kullanıcı işlevleri arasında net bir ayrım var mı?
- [ ] Evet — yönetici işlevleri, standart kullanıcı görünümlerinden **tamamen izole edilmiştir**  
- [ ] Evet — ayrım mevcuttur ancak yönetici uç noktaları (endpoints) **tahmin edilebilir veya saptanabilirdir**  
- [ ] Hayır — yönetici ve standart işlevler aynı arayüzler içinde **karışıktır**  

### Uygulama, Rol Tabanlı Erişim Kontrolü (Role-Based Access Control - RBAC) için merkezi bir mekanizma kullanıyor mu?
- [ ] Evet — merkezi RBAC veya ABAC uygulanmıştır ve tutarlı bir şekilde zorunlu kılınmaktadır  
- [ ] Evet — merkezi olmayan kontroller mevcuttur ancak modüller arasında **tutarsız bir şekilde uygulanmaktadır**  
- [ ] Hayır — erişim kontrol mantığı, bağımsız sayfalara veya bileşenlere **statik olarak kodlanmıştır** (hardcoded)  

### Sistemde belgelenmemiş veya gizli roller mevcut mu?
- [ ] Hayır — keşfedilen tüm roller belgelenmiş işlevlerle eşleşmektedir  
- [ ] Evet — gizli veya "gölge" (shadow) roller (örneğin `super_admin`, `support`, `test`) **mevcuttur**  

### Düşük ayrıcalıklı bir kullanıcı, diğer kullanıcıların izinlerini veya rollerini listeleyebilir (enumerate) mi?
- [ ] Hayır — kullanıcılar diğer varlıkların rollerini/izinlerini görüntüleyemez veya **listeleyemez**  
- [ ] Evet — kullanıcılar profil sayfaları, meta veriler veya API yanıtları aracılığıyla rolleri **listeleyebilir** *(Orta)*

---

## WSTG-IDNT-02 — Kullanıcı Kayıt Süreci Testi

Kullanıcı kayıt sürecinin test edilmesi, yeni kimliklerin oluşturulduğu iş akışının yetkisiz erişim veya otomatik istismar amacıyla suistimal edilmemesini sağlamak için değerlendirilmesini kapsar. Bu süreçteki zayıflıklar genellikle kimlik doğrulama (e-posta/SMS) eksikliği, botlar aracılığıyla toplu hesap oluşturmaya karşı hassasiyet veya mevcut kullanıcı adlarını açığa çıkaran ayrıntılı hata mesajları yoluyla bilgi ifşası (Information Disclosure) şeklinde ortaya çıkar. Saldırganlar, kullanıcı listeleme (User Enumeration) yapmak, büyük ölçekli spam kampanyaları yürütmek veya hesap oluşturma aşamasında yükseltilmiş yetkiler (Elevated Privileges) elde etmek amacıyla kayıt parametrelerini manipüle ederek bu açıkları istismar ederler. Bu test, uygulamanın bütünlüğünü korumak ve saldırganların sistemi sahte veya kötü niyetli hesaplarla doldurmasını önlemek için hayati önem taşır.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-IDNT-02 |
| **CWE** | CWE-836 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/02-Test_User_Registration_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite`, `OWASP ZAP`, `Python`, `ffuf`, `Cuppa`

### Yeni bir hesap aktif hale gelmeden önce kimlik doğrulaması gerekiyor mu?
- [ ] Evet — kayıt işlemi; e-posta veya SMS yoluyla gönderilen benzersiz, zaman kısıtlı bir Token gerektirir  
- [ ] Evet — doğrulama gereklidir ancak Tokenlar **zayıf** veya **tahmin edilebilir**dir  
- [ ] Hayır — hesaplar herhangi bir doğrulama adımı olmaksızın **anında** aktif edilir  

### Kayıt süreci kullanıcı listelemeyi (User Enumeration) engelliyor mu?
- [ ] Evet — uygulama hem yeni hem de mevcut e-posta/kullanıcı adları için **aynı** yanıtları döner  
- [ ] Hayır — hata mesajları (örneğin, "Email already in use") bir saldırganın kayıtlı kullanıcıları **tespit etmesine (map)** olanak tanır  
- [ ] Hayır — sunucu yanıtlarındaki zamanlama farkları (timing differences), bir kullanıcının mevcut olup olmadığını **açığa çıkarır**  

### Toplu kaydı önlemek için otomasyon karşıtı kontroller uygulanmış mı?
- [ ] Evet — CAPTCHA veya Proof-of-Work mekanizmaları **mevcuttur** ve **etkilidir**  
- [ ] Evet — kontroller mevcuttur ancak API uç noktaları veya Header manipülasyonu yoluyla atlatılması (bypass) **mümkündür**  
- [ ] Hayır — kayıt uç noktasına herhangi bir Rate Limiting veya CAPTCHA **uygulanmamıştır**  

### Kayıt parametreleri yetki yükseltmek (Privilege Escalation) için manipüle edilebiliyor mu?
- [ ] Hayır — kullanıcı rolleri **kesinlikle** sunucu tarafındaki mantık tarafından atanır  
- [ ] Evet — daha yüksek erişim elde etmek için istekteki `role`, `admin` veya `group_id` gibi parametreler **değiştirilebilir**  

### Kayıt aşamasında parola politikası (Password Policy) zorunlu tutuluyor mu?
- [ ] Evet — güçlü parola gereksinimleri sunucu tarafında **zorunlu kılınmıştır (enforced)**  
- [ ] Hayır — zayıf parolalar (örneğin, "password123") sistem tarafından **kabul edilir**

---

## WSTG-IDNT-03 — Hesap Hazırlama Sürecinin Test Edilmesi (Account Provisioning Process)

Hesap hazırlama (Account Provisioning) süreci; yeni kimliklerin nasıl oluşturulduğunu, nasıl doğrulandığını ve bir uygulama ortamında yetkilerin nasıl atandığını yönetir. Saldırganlar; spam veya hizmet dışı bırakma (denial-of-service) saldırıları için otomatik kitlesel kayıtlar gerçekleştirmek, sahte hesaplar oluşturmak amacıyla kimlik doğrulama adımlarını atlatmak veya kayıt aşamasında kendilerine yüksek yetkiler tanımlamak için parametreleri manipüle etmek amacıyla bu iş akışını hedeflerler. Zafiyetler genellikle; iş mantığı (business logic) hatalarının yetkisiz hesap oluşturulmasına veya yetki yükseltmeye (privilege escalation) izin verdiği kendi kendine kayıt formlarında, yalnızca davetle girilen sistemlerde veya yönetici hazırlama konsollarında ortaya çıkar. Bir saldırganın bakış açısından, hazırlama akışının ele geçirilmesi; daha derin sömürü, veri sızdırma (data exfiltration) veya sosyal mühendislik saldırıları için bir basamak olarak kullanılabilecek meşru bir dayanak noktası sağlar.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-IDNT-03 |
| **CWE** | CWE-285 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek* |

> *Bir saldırganın yönetici hesapları oluşturabilmesi veya genel kayıt kısıtlamalarını atlatabilmesi durumunda Önem Derecesi Kritik (Critical) seviyesine yükselir.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/03-Test_Account_Provisioning_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite`, `FFUF`, `Postman`, `Python`, `MailHog`

### Kendi kendine kayıt işlemi sıkı bir şekilde kontrol ediliyor mu veya yetkili kullanıcılarla sınırlandırılmış mı?
- [ ] Evet — kayıt işlemi **devre dışı** bırakılmış veya manuel yönetici onayı gerektiriyor  
- [ ] Evet — kayıt işlemi açık ancak **yetkili** e-posta alan adları veya davetiye belirteçleri (tokens) ile sınırlandırılmış  
- [ ] Hayır — kayıt işlemi herhangi bir alan adı veya belirteç kısıtlaması olmaksızın tüm kullanıcılara **açık**  

### Hesap aktivasyonundan önce güçlü bir kimlik doğrulama mekanizması (örneğin, e-posta/SMS) gerekiyor mu?
- [ ] Evet — doğrulama **zorunlu** ve **atılatılamaz**  
- [ ] Evet — doğrulama **zorunlu** ancak parametre kurcalama (parameter tampering) veya tahmin edilebilir belirteçler (predictable tokens) aracılığıyla **atlatılabilir**  
- [ ] Hayır — hesaplar, herhangi bir doğrulama olmaksızın gönderim anında **hemen aktif** hale geliyor  

### Kullanıcı, kayıt isteği sırasında rol veya izin parametrelerini manipüle edebiliyor mu?
- [ ] Hayır — roller **sunucu tarafında atanır** ve istemci tarafında herhangi bir parametre bulunmaz  
- [ ] Evet — rol parametreleri mevcut ancak sunucu taraflı doğrulama nedeniyle manipülasyon **mümkün değil**  
- [ ] Evet — rol/izin parametreleri (örneğin, `role=admin`, `is_staff=true`) yüksek yetkili erişim elde etmek için **değiştirilebilir** *(Kritik)*  

### Kitlesel hesap oluşturmayı önlemek için hız sınırlaması (Rate Limiting) veya otomasyon karşıtı kontroller uygulanmış mı?
- [ ] Evet — hız sınırları ve CAPTCHA'lar **etkin** ve başarılı  
- [ ] Evet — kontroller mevcut ancak başlık sahteciliği (header spoofing) veya CAPTCHA çözme servisleri ile atlatılması **mümkün**  
- [ ] Hayır — herhangi bir hız sınırlaması veya otomasyon karşıtı kontrol **uygulanmamış**  

### Hazırlama süreci mevcut hesaplar hakkında bilgi sızdırıyor mu (Kullanıcı Tespiti - User Enumeration)?
- [ ] Hayır — yanıt mesajları mevcut olan ve olmayan kullanıcılar için **aynıdır**  
- [ ] Evet — farklı yanıtlar veya zamanlama farklılıkları mevcut kullanıcıların **tespit edilmesine** (enumeration) olanak tanır

---

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

## WSTG-ATHN-01 — Kimlik Bilgilerinin Şifrelenmiş Bir Kanal Üzerinden Aktarılmasının Test Edilmesi

Kimlik bilgilerinin şifrelenmiş bir kanal üzerinden aktarılmasının test edilmesi; kullanıcı adları, parolalar ve oturum belirteçleri (session tokens) gibi hassas kimlik doğrulama verilerinin iletim sırasında ele geçirilmeye karşı korunmasını sağlar. Ağ üzerinde konumlanmış saldırganlar —örneğin halka açık Wi-Fi ağlarındaki Aradaki Adam (Man-in-the-Middle - MitM) saldırıları veya ele geçirilmiş dahili ağlar aracılığıyla— TLS/SSL düzgün bir şekilde zorunlu kılınmamışsa, paket koklama (packet sniffing) araçlarını kullanarak açık metin halindeki kimlik bilgilerini ele geçirebilirler. Bu zafiyet genellikle uygulamanın HTTPS'i zorunlu kılmadığı veya HTTP'den hatalı yönlendirme yaptığı oturum açma uç noktalarında, parola sıfırlama formlarında ve API kimlik doğrulama başlıklarında (headers) görülür. Başarılı bir istismar (exploitation), saldırgana kullanıcı hesaplarına tam erişim sağlayarak kullanıcı verilerinin tamamen tehlikeye girmesine ve uygulama içinde potansiyel yanal harekete (lateral movement) yol açar.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-ATHN-01 |
| **CWE** | CWE-319 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Şiddet** | Yüksek |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/01-Testing_for_Credentials_Transported_over_an_Encrypted_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Araçlar:** `Wireshark`, `tcpdump`, `Burp Suite`, `OWASP ZAP`, `testssl.sh`, `sslyze`, `nmap`

### Tüm kimlik doğrulama süreci için HTTPS zorunlu kılınıyor mu?
- [ ] Evet — HSTS **etkindir** ve tüm trafik kesin olarak HTTPS üzerinden zorlanır  
- [ ] Evet — HTTPS'e yönlendirme gerçekleşiyor, ancak HSTS **etkin değil**  
- [ ] Hayır — kimlik bilgileri şifrelenmemiş HTTP üzerinden **gönderilebiliyor**  

### Oturum açma sayfaları ve formlar şifrelenmiş bir kanal üzerinden mi sunuluyor?
- [ ] Evet — oturum açma formunu içeren sayfa HTTPS aracılığıyla iletiliyor  
- [ ] Hayır — oturum açma sayfası HTTP üzerinden sunuluyor; bu da form eylemi gaspına (form-action hijacking) veya kimlik bilgisi koklamaya (credential sniffing) olanak tanıyor  

### Tüm hassas oturum çerezlerine `Secure` bayrağı uygulanmış mı?
- [ ] Evet — tüm kimlik doğrulama ve oturum çerezlerine `Secure` bayrağı **uygulanmış**  
- [ ] Evet — `Secure` bayrağı sadece bazı çerezlere **uygulanmış**  
- [ ] Hayır — `Secure` bayrağı **uygulanmamış**; bu da çerezlerin şifrelenmemiş istekler aracılığıyla sızdırılmasına (exfiltrated) izin veriyor  

### Sunucu zayıf TLS sürümlerini veya güvensiz şifreleme paketlerini (cipher suites) destekliyor mu?
- [ ] Hayır — sadece modern TLS (1.2+) ve güçlü şifrelemeler **etkin**  
- [ ] Evet — eski sürümler (TLS 1.0/1.1) veya zayıf şifrelemeler (RC4, DES, CBC) **destekleniyor**  

### Uygulama, kimlik bilgilerinin üçüncü taraf alan adlarına (domains) şifrelenmemiş kanallar üzerinden iletilmesini engelliyor mu?
- [ ] Evet — harici alan adlarına hassas veri gönderilmiyor veya tüm harici çağrılar HTTPS üzerinden zorlanıyor  
- [ ] Hayır — kimlik doğrulama başlıkları veya kimlik bilgileri, HTTP üzerinden üçüncü taraf uç noktalarına (örneğin; analitik veya SSO) gönderiliyor

---

## WSTG-ATHN-02 — Varsayılan Kimlik Bilgilerinin Test Edilmesi

Varsayılan kimlik bilgilerinin test edilmesi; fabrika ayarlarında bırakılmış veya tedarikçi tarafından sağlanan kullanıcı adlarını ve parolalarını hala kullanan yönetici arayüzlerinin, servislerin veya uygulama bileşenlerinin tespit edilmesini içerir. Bu zafiyet, minimum çabayla tam sistem ele geçirme, yönetici erişimi veya uzaktan kod yürütme (Remote Code Execution) yolları sunduğu için saldırganlar için yüksek öncelikli bir hedeftir. Genellikle hazır yazılımlarda, CMS platformlarında, veritabanı yönetim araçlarında ve IoT cihazları veya ağ donanımları gibi entegre donanımlarda görülür. İstismar (Exploitation) işlemi genellikle, tespit edilen yazılım sürümlerinin halka açık varsayılan parola veritabanlarıyla karşılaştırılmasıyla veya ilk keşif ve istismar aşamalarında otomatik kimlik bilgisi doldurma (Credential Stuffing) araçlarının kullanılmasıyla gerçekleştirilir.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-ATHN-02 |
| **CWE** | CWE-1392 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Kritik / Yüksek |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/02-Testing_for_Default_Credentials  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Araçlar:** `Hydra`, `Medusa`, `Burp Suite (Intruder)`, `Nmap`, `Metasploit`, `DefaultPassword.com`

### Yönetici veya yönetim arayüzleri internete veya güvenilmeyen ağ segmentlerine açık mı?
- [ ] Hayır — hiçbir yönetim arayüzüne erişilemez  
- [ ] Evet — arayüzler mevcut ancak ağ düzeyindeki kontrollerle (örneğin VPN, IP İzin Listesi) sınırlandırılmış  
- [ ] Evet — arayüzler halka **açıktır** ve kimlik doğrulaması gerektirir  

### Tanımlanan tüm bileşenler için tedarikçi tarafından sağlanan varsayılan kimlik bilgileri değiştirildi mi?
- [ ] Evet — tüm bileşenlerdeki tüm varsayılan kimlik bilgileri **değiştirilmiştir** *(En güvenli)*  
- [ ] Evet — ana uygulama için kimlik bilgileri **değiştirilmiştir**, ancak ikincil bileşenler (örneğin CMS, DB) varsayılan olarak kalmıştır  
- [ ] Hayır — bir veya daha fazla tanımlanabilir arayüzde varsayılan kimlik bilgileri **aktiftir** *(Kritik)*  

### Uygulama, yeni veya yönetici hesapları için ilk girişte parola değişimini zorunlu kılıyor mu?
- [ ] Evet — herhangi bir yönetimsel işlem yapılmadan önce parola değişikliği **zorunludur**  
- [ ] Hayır — uygulama, varsayılan kimlik bilgilerinin süresiz olarak kullanılmasına **izin verir**  

### Otomatik varsayılan kimlik bilgisi testlerini önlemek için kilitleme mekanizmaları veya hız sınırlama (Rate Limiting) kontrolleri aktif mi?
- [ ] Evet — agresif hız sınırlama veya hesap kilitleme **uygulanmaktadır**  
- [ ] Evet — kontroller mevcuttur ancak başlık manipülasyonu veya IP rotasyonu ile atlatılması (Bypass) **mümkündür**  
- [ ] Hayır — hiçbir hız sınırlama veya kilitleme mekanizması **etkin değildir**  

### Üçüncü taraf eklentiler, ara katman yazılımları veya yönetim araçları (örneğin phpMyAdmin, Tomcat Manager) benzersiz kimlik bilgileri kullanıyor mu?
- [ ] Hayır — ortamda üçüncü taraf bileşen bulunmamaktadır  
- [ ] Evet — tüm üçüncü taraf bileşenler benzersiz, varsayılan olmayan kimlik bilgileri kullanmaktadır  
- [ ] Hayır — en az bir üçüncü taraf bileşene bilinen varsayılan kimlik bilgileriyle **erişilebilmektedir**

---

## WSTG-ATHN-03 — Zayıf Hesap Kilitleme Mekanizmasının Test Edilmesi (Testing for Weak Lock Out Mechanism)

Hesap kilitleme mekanizması (Account Lockout Mechanism), belirli sayıda başarısız kimlik doğrulama denemesinden sonra hesabı devre dışı bırakarak brute-force (kaba kuvvet) ve credential stuffing (kimlik bilgisi doldurma) saldırılarını azaltmak için tasarlanmış kritik bir derinlemesine savunma (defense-in-depth) kontrolüdür. Sağlam bir kilitleme politikası olmadan saldırganlar, birden fazla hesap genelinde sistematik olarak şifre tahmin etmek (password spraying) veya başarılı olana kadar yüksek değerli tek bir hesabı hedeflemek için otomatik araçlar kullanabilirler. Bu zayıflık genellikle giriş portallarında, şifre sıfırlama formlarında ve Rate Limiting (hız sınırlaması) veya hesap düzeyinde koruması bulunmayan API uç noktalarında (endpoints) ortaya çıkar. Bir saldırganın perspektifinden, zayıf veya eksik bir kilitleme mekanizması sonsuz şifre denemesine izin verirken; aşırı agresif bir mekanizma ise meşru kullanıcıların hesaplarını kasıtlı olarak kilitleyerek onlara karşı bir dağıtık Denial of Service (DoS - Hizmet Reddi) saldırısı gerçekleştirmek için kötüye kullanılabilir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-ATHN-03 |
| **CWE** | CWE-307 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek* |

> *Uygulama hassas PII (Kişisel Veri), finansal veri işliyorsa veya yönetici hesapları herhangi bir ikincil kontrol olmaksızın Brute Force saldırılarına karşı savunmasızsa Önem Derecesi Yüksek olur.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/03-Testing_for_Weak_Lock_Out_Mechanism  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Araçlar:** `Burp Suite Intruder`, `THC-Hydra`, `ffuf`, `Patator`, `wfuzz`

### Belirlenmiş sayıda başarısız denemeden sonra bir hesap kilitleme mekanizması uygulanıyor mu?
- [ ] Evet — hesap tanımlanmış, makul bir eşikten sonra (örneğin 5-10 deneme) kilitlenir  
- [ ] Evet — hesap kilitlenir ancak yalnızca aşırı yüksek sayıda denemeden sonra  
- [ ] Hayır — hesap **kilitlenemez** ve süresiz Brute Force denemesine izin verir  

### Kilitleme mekanizması yaygın atlatma teknikleri kullanılarak baypas edilebiliyor mu?
- [ ] Hayır — kilitleme sunucu tarafında (server-side) uygulanır ve istemci tarafındaki değişikliklerden **etkilenmez**  
- [ ] Evet — IP adreslerini döndürerek veya bir proxy havuzu kullanarak kilitlemeyi baypas etmek **mümkündür**  
- [ ] Evet — `X-Forwarded-For` veya `User-Agent` gibi HTTP başlıklarını manipüle ederek kilitlemeyi baypas etmek **mümkündür**  
- [ ] Evet — çerezleri (cookies) temizleyerek veya yeni oturumlar başlatarak kilitlemeyi baypas etmek **mümkündür**  

### Kilitleme mekanizması hesap kurtarma veya süre dolumu için bir yol sağlıyor mu?
- [ ] Evet — hesap belirli bir süre sonra otomatik olarak veya e-posta doğrulaması yoluyla açılır  
- [ ] Evet — hesabın açılması için yönetici müdahalesi gerekir  
- [ ] Hayır — kilitleme kalıcıdır ve net bir kurtarma yolu yoktur, bu da DoS riskini artırır  

### Uygulama, kilitleme sırasında geçerli ve geçersiz kullanıcı adları arasında ayrım yapıyor mu?
- [ ] Hayır — uygulama yanıtı hem mevcut olan hem de mevcut olmayan kullanıcılar için aynıdır  
- [ ] Evet — uygulama bir hesabın kilitli olup olmadığını ifşa eder, bu da **username enumeration** (kullanıcı adı listeleme) saldırısına olanak tanır  

### Kilitleme mekanizması bir Denial of Service (DoS) saldırısına karşı hassas mı?
- [ ] Hayır — CAPTCHA veya IP tabanlı Rate Limiting gibi kontroller toplu hesap kilitlenmesini engeller  
- [ ] Evet — bir saldırgan, yanlış şifreler sağlayarak bilinen tüm kullanıcıları sistematik olarak **kilitleyebilir**

---

## WSTG-ATHN-04 — Kimlik Doğrulama Şemasını Atlatma Testi (Testing for Bypassing Authentication Schema)

Kimlik doğrulama şemasını atlatma (Bypassing the authentication schema), bir saldırganın geçerli kimlik bilgileri (credentials) sağlamadan korunan kaynaklara erişmesine izin veren uygulama mantığındaki açıkların tespit edilmesini ve istismar edilmesini içerir. Bu zafiyet genellikle uygulamanın istemci taraflı kontrollere (client-side controls) güvenmesi, her istekte sunucu taraflı yetkilendirmeyi (server-side authorization) zorunlu kılmaması veya üretim ortamında yönetici "arka kapılarını" (backdoors) ve hata ayıklama (debug) uç noktalarını (endpoints) açık bırakması durumunda ortaya çıkar. Saldırganlar; hassas URL'lere zorlamalı gezinti (Forced Browsing) yaparak, `authenticated=true` gibi HTTP parametrelerini manipüle ederek veya uygulamanın bir oturumun (Session) halihazırda kurulduğunu varsaymasını sağlamak için üstbilgi sahteciliği (Header Spoofing) yaparak bu zayıflıkları istismar ederler. Başarılı bir istismar (Exploit), kimlik doğrulama sınırının tamamen çökmesine neden olur; bu da potansiyel olarak yetkisiz veri sızdırmasına (Data Exfiltration) veya sistemin tamamen ele geçirilmesine yol açabilir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-ATHN-04 |
| **CWE** | CWE-287 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Kritik |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/04-Testing_for_Bypassing_Authentication_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Araçlar:** `Burp Suite`, `Ffuf`, `Gobuster`, `Dirsearch`, `Postman`

### Hassas dahili sayfalara aktif bir oturum (Session) olmadan doğrudan erişilebiliyor mu?
- [ ] Hayır — doğrudan erişim, giriş sayfasına yönlendirme veya 403/404 yanıtı ile sonuçlanır  
- [ ] Evet — bazı hassas sayfalara erişilebiliyor ancak sınırlı veya hassas olmayan veriler sağlanıyor  
- [ ] Evet — hassas yönetici veya kullanıcıya özel sayfalara kimlik doğrulaması olmadan tam olarak **erişilebiliyor** *(Kritik)*  

### Uygulama, kimlik doğrulama durumunu belirlemek için istemci taraflı bayraklara (flags) veya parametrelere güveniyor mu?
- [ ] Hayır — kimlik doğrulama durumu kesinlikle sunucu taraflı oturum durumu (session state) üzerinden yönetilir  
- [ ] Evet — istemci taraflı bayraklar mevcuttur ancak değişiklik yapılması korunan alanlara erişim **sağlamaz**  
- [ ] Evet — parametreleri (örneğin `is_authenticated=true`, `user_role=admin`) değiştirmek kimlik doğrulamasının atlatılmasına (Authentication Bypass) **izin verir**  

### Belirli HTTP üstbilgileri (headers) manipüle edilerek veya sahtecilik yapılarak kimlik doğrulaması atlatılabilir mi?
- [ ] Hayır — `X-Forwarded-For`, `Referer` veya özel üstbilgilerin (custom headers) kimlik doğrulama üzerinde hiçbir etkisi yoktur  
- [ ] Evet — üstbilgileri (örneğin `X-Custom-IP-Authorization`, `X-Remote-User`) manipüle etmek **mümkündür** ve yetkisiz erişim sağlar  

### Standart giriş akışını (login flow) devre dışı bırakan alternatif yollar veya geliştirme artıkları (artifacts) var mı?
- [ ] Hayır — tüm korunan kaynaklar merkezi ve canlı ortama hazır (production-ready) bir kimlik doğrulama kontrolü ile korunmaktadır  
- [ ] Evet — eski uç noktalar (legacy endpoints), hata ayıklama yolları (örneğin `/debug/login`) veya unutulmuş API sürümlerine kimlik bilgileri olmadan **erişilebiliyor**  

### Uygulama, yüksek ayrıcalıklı işlemler için yeniden kimlik doğrulama yapmada veya oturumları doğrulamada başarısız oluyor mu?
- [ ] Hayır — yüksek ayrıcalıklı veya hassas işlemler yeni veya geçerli bir oturum belirteci (Session Token) gerektirir  
- [ ] Evet — bir uç noktada (endpoint) atlatma bulunduğunda, bu durum uygulama genelinde yönetici işlemlerini gerçekleştirmek için **kullanılabilir**

---

## WSTG-ATHN-05 — Güvenlik Açıklı Beni Hatırla Fonksiyonunun Test Edilmesi

"Beni Hatırla" (Remember Me) veya kalıcı oturum açma (persistent login) fonksiyonu, bir token veya kimlik bilgisini istemci tarafında saklayarak kullanıcının kimlik doğrulaması yapılmış durumunu tarayıcı yeniden başlatılsa dahi korumak üzere tasarlanmıştır. Eğer bu özellik; açık metin (plaintext) parolaların, zayıf özetlerin (hashes) veya düşük entropili tokenların çerezlerde (cookies) saklanması gibi hatalı şekillerde yapılandırılmışsa; kullanıcıyı Cross-Site Scripting (XSS), fiziksel erişim veya ağ trafiğine müdahale edilmesi yoluyla hesap ele geçirme (account takeover) riskine maruz bırakır. Sızma testi uzmanları (pentesters), kalıcı erişim kolaylığının; Çok Faktörlü Kimlik Doğrulama (MFA) veya oturum sonlandırma gibi kritik güvenlik kontrollerini devre dışı bırakmadığından emin olmak için bu kalıcı tokenların entropisini, depolama mekanizmasına uygulanan güvenlik bayraklarını (security flags) ve oturum yaşam döngüsünü değerlendirir. İstismar süreci genellikle, orijinal kimlik bilgilerine ihtiyaç duymadan hesaplara yetkisiz erişim sağlamak için bu uzun ömürlü tokenların ele geçirilmesini içerir.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-ATHN-05 |
| **CWE** | CWE-522 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek* |

> *Kalıcı çerezde açık metin kimlik bilgileri veya tuzlanmamış (unsalted) özetler saklanıyorsa veya token Çok Faktörlü Kimlik Doğrulamayı (MFA) atlatıyorsa önem derecesi Yüksek olur.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/05-Testing_for_Vulnerable_Remember_Password  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  
* https://portswigger.net/web-security/authentication  

**Araçlar:** `Burp Suite (Cookie Editor/Sequencer)`, `Browser Developer Tools`, `EditThisCookie`

### Uygulama bir "Beni Hatırla" veya "Oturumu Açık Tut" özelliğini uyguluyor mu?
- [ ] Hayır — uygulama içinde bu özellik **bulunmamaktadır**  
- [ ] Evet — özellik mevcut ve kriptografik olarak güçlü, tahmin edilemez tokenlar kullanıyor  
- [ ] Evet — özellik mevcut ancak tahmin edilebilir veya düşük entropili tokenlar kullanıyor *(Orta)*  
- [ ] Evet — özellik mevcut ve açık metin kimlik bilgilerini veya base64 ile kodlanmış parolaları saklıyor *(Yüksek)*  

### Güvenlik bayrakları kalıcı çereze doğru şekilde uygulanmış mı?
- [ ] Evet — `HttpOnly`, `Secure` ve `SameSite` bayrakları **uygulanmıştır**  
- [ ] Evet — bazı bayraklar uygulanmış ancak `HttpOnly` **eksik**  
- [ ] Evet — bazı bayraklar uygulanmış ancak HTTPS üzerinden `Secure` **eksik**  
- [ ] Hayır — kalıcı çereze hiçbir güvenlik bayrağı **uygulanmamıştır**  

### "Beni Hatırla" tokenı, manuel çıkış işleminden sonra geçerli kalmaya devam ediyor mu?
- [ ] Hayır — token, çıkış yapıldığı anda sunucu tarafında geçersiz kılınmaktadır  
- [ ] Evet — token geçerli kalıyor ancak hassas işlemler için parola gerektiriyor  
- [ ] Evet — token geçerli kalıyor ve yeniden kimlik doğrulaması **gerekmeden** tam hesap erişimine izin veriyor *(Orta)*  

### Parola değişikliği yapıldığında kalıcı oturum sonlandırılıyor mu?
- [ ] Evet — kullanıcı parolasını değiştirdiğinde tüm kalıcı tokenlar iptal edilmektedir  
- [ ] Hayır — parola sıfırlama/değiştirme işlemi **gerçekleştirildikten sonra** "Beni Hatırla" tokenı geçerli kalmaya devam ediyor *(Yüksek)*  

### Kalıcı token, sonraki ziyaretlerde Çok Faktörlü Kimlik Doğrulamayı (MFA) atlatıyor mu?
- [ ] Hayır — bir "Beni Hatırla" tokenı olsa dahi MFA gerekmektedir  
- [ ] Evet — MFA atlatılıyor ancak token belirli bir cihaz parmak izine (device fingerprint) bağlıdır  
- [ ] Evet — herhangi bir cihazdan sadece kalıcı çerez kullanılarak MFA **tamamen atlatılıyor** *(Yüksek)*

---

## WSTG-ATHN-06 — Tarayıcı Önbelleği Zafiyeti Testi

Tarayıcı önbelleği zafiyeti (Browser cache weakness), hassas bilgiler yerel tarayıcı önbelleğinde saklandığında ve aynı fiziksel makineye erişimi olan yetkisiz bir kullanıcı tarafından geri alınabildiğinde ortaya çıkar. Uygun önbellek kontrol başlıklarının (cache control headers) eksikliği; kişisel bilgiler, hesap detayları veya oturum tanımlayıcıları (session identifiers) gibi potansiyel olarak hassas verilerin, kullanıcı oturumu kapattıktan veya oturumu kapattıktan sonra da kalıcı olmasına izin verir. Saldırganlar veya paylaşılan ya da genel terminallerdeki sonraki kullanıcılar; tarayıcı geçmişinde gezinerek, "Geri" butonunu kullanarak veya önbelleğe alınmış yanıtları dışarı sızdırmak (exfiltrate) için yerel önbellek dosyalarını inceleyerek bu durumdan faydalanabilirler. Hassas verilerin asla diske yazılmamasını sağlamak amacıyla, kimlik doğrulaması yapılmış içeriği işleyen her uygulama için `Cache-Control: no-store` ve `Pragma: no-cache` gibi katı direktiflerin uygulanması kritik önem taşır.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-ATHN-06 |
| **CWE** | CWE-525 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Şiddet** | Düşük / Orta* |

> *Uygulamaya sık sık paylaşılan/genel kiosklar üzerinden erişiliyorsa veya uygulama yüksek derecede hassas PII/PHI verileri içeriyorsa şiddet Orta (Medium) seviyesine çıkar.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/06-Testing_for_Browser_Cache_Weaknesses  

**Araçlar:** `Burp Suite (Proxy/Repeater)`, `Browser Developer Tools (Network Tab)`, `Zed Attack Proxy (ZAP)`

### Kimlik doğrulaması yapılmış veya hassas sayfalarda uygun önbellek kontrol (cache-control) başlıkları mevcut mu?
- [ ] Evet — `Cache-Control: no-store, no-cache` ve `Pragma: no-cache` başlıkları **mevcut** ve **doğru yapılandırılmış**  
- [ ] Evet — bazı başlıklar mevcut ancak `no-store` **eksik**, bu da disk önbelleğine izin veriyor  
- [ ] Hayır — varsayılan tarayıcı davranışına güveniliyor, önbellekle ilgili hiçbir başlık **uygulanmamış**  

### Uygulama, kullanıcıya özel veriler için `private` direktifini kullanıyor mu?
- [ ] Evet — ara sunucu (proxy) önbelleğe alımını önlemek için tüm kişiselleştirilmiş içeriklerde `Cache-Control: private` **etkinleştirilmiş**  
- [ ] Hayır — içerik `public` olarak işaretlenmiş veya `private` direktifi eksik, bu da içeriği ara proxy'ler tarafından **önbelleğe alınabilir** kılıyor  

### Başarılı bir oturum kapatma işleminden sonra tarayıcının "Geri" butonu ile hassas bilgilere erişilebiliyor mu?
- [ ] Hayır — tarayıcı yeniden kimlik doğrulaması istiyor veya sayfa yerel önbellekten **yüklenmiyor**  
- [ ] Evet — oturum sonlandırıldıktan sonra bile "Geri" butonu aracılığıyla hassas bilgiler **hala görüntülenebiliyor**  

### Hassas HTML dışı dosyalar (örneğin PDF'ler, CSV dışa aktarımları, JSON API yanıtları) önbelleğe almaya karşı korunuyor mu?
- [ ] Hayır — uygulama tarafından hassas hiçbir HTML dışı dosya oluşturulmuyor veya işlenmiyor  
- [ ] Evet — tüm hassas dosya indirmelerine ve API uç noktalarına (endpoints) katı önbellek kontrol başlıkları **uygulanmış**  
- [ ] Hayır — hassas indirmeler veya API yanıtları yerel tarayıcı önbelleğinde veya geçici dizinlerde **saklanıyor**  

### Uygulama, güncelliğini yitirmiş veri kullanımını önlemek için `Expires: 0` veya geçmiş bir tarih kullanıyor mu?
- [ ] Evet — `Expires` başlığı `0` veya geçmiş bir tarihe ayarlanmış, tarayıcının içeriği anında güncelliğini yitirmiş (stale) olarak değerlendirmesi **sağlanmış**  
- [ ] Hayır — hassas sayfalar için `Expires` başlığı **eksik** veya gelecekteki bir tarihe ayarlanmış

---

## WSTG-ATHN-07 — Zayıf Parola Politikası Testi

Zayıf parola politikalarının test edilmesi, bir uygulama tarafından hesap oluşturma ve kimlik bilgisi güncellemeleri sırasında zorunlu kılınan karmaşıklık, uzunluk ve entropi gereksinimlerinin değerlendirilmesini içerir. Yetersiz politikalar; kullanıcı hesaplarını otomatik sözlük saldırılarına (dictionary attacks), Brute Force girişimlerine ve kimlik bilgisi doldurma (credential stuffing) saldırılarına karşı savunmasız bırakarak gizliliğini doğrudan tehlikeye atar. Bu zafiyetler genellikle kayıt uç noktalarında (endpoints), parola sıfırlama iş akışlarında ve idari kullanıcı yönetimi panellerinde bulunur. Bir saldırganın perspektifinden bakıldığında, esnek bir politika, yaygın wordlist'ler veya kalıp tabanlı kırma yöntemleri kullanılarak kimlik bilgilerinin başarılı bir şekilde ele geçirilmesi için gereken hesaplama maliyetini ve süresini önemli ölçüde azaltır.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-ATHN-07 |
| **CWE** | CWE-521 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Düşük* |

> *Uygulama, otomatik tahminleri önlemek için hesap kilitleme (account lockout) veya Rate Limiting mekanizmalarından yoksunsa önem derecesi Yüksek (High) olur.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/07-Testing_for_Weak_Authentication_Methods  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**Araçlar:** `Burp Suite (Intruder)`, `ZAP (Fuzzer)`, `Hydra`, `Hashcat`, `Cewl`

### Uygulama tarafından minimum parola uzunluğu zorunlu tutuluyor mu?
- [ ] Evet — minimum uzunluk 12+ karakterdir *(En güvenli)*  
- [ ] Evet — minimum uzunluk 8 ile 11 karakter arasındadır  
- [ ] Evet — minimum uzunluk 8 karakterden **azdır**  
- [ ] Hayır — herhangi bir minimum uzunluk zorunluluğu yoktur  

### Karmaşıklık gereksinimleri (büyük harf, küçük harf, rakam, sembol) zorunlu tutuluyor mu?
- [ ] Evet — birden fazla karakter sınıfı zorunludur ve atlatma (bypass) **mümkün değildir**  
- [ ] Evet — karakter sınıfları önerilir ancak zorunlu **değildir**  
- [ ] Hayır — hiçbir karmaşıklık gereksinimi uygulanmamaktadır  

### Uygulama, yaygın/zayıf parolaları veya kullanıcıya özgü verileri içeren parolaları engelliyor mu?
- [ ] Evet — bilinen zayıf parolalar (örn. "password123") ve kullanıcı adı tabanlı parolalar **kullanılamaz**  
- [ ] Evet — yaygın parolalar engellenir, ancak kullanıcıya özgü veriler (örn. kullanıcı adı, e-posta) **kullanılabilir**  
- [ ] Hayır — uzunluk/karmaşıklık gereksinimlerini karşılayan her türlü karakter dizisi kabul edilir  

### Karmaşıklık veya uzunluk gereksinimleri istemci tarafı (client-side) modifikasyonu ile atlatılabilir mi?
- [ ] Hayır — politikalar **sunucu tarafında (server-side)** sıkı bir şekilde uygulanmaktadır  
- [ ] Evet — politikalar JavaScript aracılığıyla uygulanmaktadır ve isteğin araya girilerek yakalanması (intercept) ile **atlatılabilir**  

### Parola politikası, uygulama detaylarını sızdırmadan kullanıcıya net bir şekilde iletiliyor mu?
- [ ] Evet — politika giriş sırasında görünürdür ve genel (generic) hata mesajları sağlanır  
- [ ] Evet — politika görünürdür ancak hata mesajları belirli regex veya mantık açıklarını ortaya çıkarır  
- [ ] Hayır — politika görünür **değildir**, bu da deneme yanılma yoluyla keşfetmeyi zorunlu kılar

---

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

## WSTG-ATHN-10 — Alternatif Kanallarda Daha Zayıf Kimlik Doğrulama Testi

Alternatif kanallarda daha zayıf kimlik doğrulama testi; mobil API'ler, parola kurtarma iş akışları, yardım masası (help desk) arayüzleri veya eski (legacy) alt alan adları gibi, ana web portalı ile aynı güvenlik sıkılığına sahip olmayabilecek ikincil hesap erişim yollarının belirlenmesini ve analiz edilmesini içerir. Bu alternatif kanallar genellikle güçlü çok faktörlü kimlik doğrulama (MFA), katı hız sınırlama (Rate Limiting) veya karmaşık parola gereksinimlerinden yoksundur; bu da tüm kimlik doğrulama sistemini zayıflatan bir "en zayıf halka" oluşturur. Saldırganlar, farklı arayüzlerin kimlik doğrulamasını ele alma biçimlerindeki tutarsızlıklardan yararlanarak Kimlik Bilgisi Doldurma (Credential Stuffing) gerçekleştirmek veya MFA'yı atlatmak için özellikle bu göz ardı edilen giriş noktalarını hedefler. Yetkisiz erişimi önlemek ve kullanıcının oturum ve veri bütünlüğünü korumak için tüm kimlik doğrulama yüzeylerinde eşitliğin sağlanması esastır.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-ATHN-10 |
| **CWE** | CWE-287 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/10-Testing_for_Weaker_Authentication_in_Alternative_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite`, `Postman`, `cURL`, `MobSF`, `Ffuf`

### Alternatif kimlik doğrulama kanalları mevcut mu ve tanımlanmış mı?
- [ ] Hayır — yalnızca tek bir kimlik doğrulama kanalı mevcut  
- [ ] Evet — birden fazla kanal tanımlandı (örneğin; mobil uygulama API'si, masaüstü istemcisi, eski portal, SOAP servisleri)  

### Alternatif kanallar, ana kanal ile güvenlik eşitliğini sağlıyor mu?
- [ ] Evet — tüm kanallar **aynı** parola karmaşıklığı ve MFA gereksinimlerini uygular  
- [ ] Evet — kanallar parola karmaşıklığını uygular ancak MFA **zorunlu değildir** veya **atlatılabilir**  
- [ ] Hayır — alternatif kanallar **önemli ölçüde daha zayıf** kimlik doğrulama gereksinimlerine sahiptir  

### Hız sınırlama ve hesap kilitleme tüm kanallarda tutarlı mı?
- [ ] Evet — hız sınırlama (Rate Limiting) ve kilitleme tüm uç noktalarda **tutarlı bir şekilde uygulanmaktadır**  
- [ ] Evet — kontroller mevcuttur ancak belirli kanallarda (örneğin; mobil API) atlatma **mümkündür**  
- [ ] Hayır — ikincil kimlik doğrulama yollarına hız sınırlama veya kilitleme **uygulanmamıştır**  

### Alternatif bir kanala geçiş yapılarak MFA atlatılabilir mi?
- [ ] Hayır — MFA zorunludur ve giriş noktasından bağımsız olarak **atlatılamaz**  
- [ ] Evet — MFA web portalında gereklidir ancak mobil veya eski API üzerinde **uygulanmaz**  

### Parola kurtarma veya "parolamı unuttum" akışları standart girişten daha mı zayıf?
- [ ] Hayır — kurtarma akışları güçlü, bant dışı (out-of-band) doğrulama kullanır ve bilgi sızdırmaz  
- [ ] Evet — kurtarma akışları kolayca tahmin edilebilen veya araştırılabilen zayıf güvenlik soruları kullanır  
- [ ] Evet — kurtarma akışları numaralandırmaya (Enumeration) karşı savunmasızdır veya standart bir girişle aynı düzeyde kanıt **gerektirmez**

---

## WSTG-ATHN-11 — Çok Faktörlü Kimlik Doğrulamanın (MFA) Test Edilmesi

Çok faktörlü kimlik doğrulama (Multi-Factor Authentication - MFA) testi, birincil kimlik bilgilerinin ele geçirilmesi durumunda bile yetkisiz erişimi önlemek için tasarlanmış ikincil güvenlik katmanının dayanıklılığını değerlendirir. Saldırganlar sıklıkla eski API versiyonları, mobil arka uçlar veya parola sıfırlama iş akışları gibi MFA'nın tutarsız uygulandığı uç noktaları tespit ederek MFA'yı atlatmaya çalışırlar. İstismar süreci genellikle sunucu yanıtlarının manipüle edilmesini, kısa ömürlü kodların (OTP) Brute Force ile kırılmasını veya kullanıcının ikinci faktörü sağlamadan kimlik doğrulanmış bir duruma geçmesine izin veren Race Condition ve oturum yönetimi kusurlarının kötüye kullanılmasını içerir.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-ATHN-11 |
| **CWE** | CWE-287 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Kritik |

**Kaynaklar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/11-Testing_Multi-Factor_Authentication  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite (Intruder/Repeater/Turbo Intruder)`, `Postman`, `Mitproxy`

### MFA tüm kimlik doğrulama portallarında tutarlı bir şekilde zorunlu tutuluyor mu?
- [ ] Evet — Tüm web, mobil ve API tabanlı giriş denemeleri için MFA gereklidir  
- [ ] Evet — ancak belirli uç noktalarda (örneğin eski `/api/v1/login` veya mobile özel portallar) MFA **zorunlu değildir**  
- [ ] Hayır — Uygulamada MFA **uygulanmamıştır** *(Kritik)*  

### MFA doğrulama adımı, doğrudan URL navigasyonu veya yanıt manipülasyonu yoluyla atlatılabilir mi?
- [ ] Hayır — doğrudan navigasyon veya parametre tahrifatı doğrulamayı **atlatamaz**  
- [ ] Evet — MFA doğrulaması tamamlanmadan doğrudan dahili dashboard URL'lerine gitmek **mümkündür**  
- [ ] Evet — Sunucu yanıtını manipüle etmek (örneğin `401 Unauthorized` yanıtını `200 OK` olarak değiştirmek) erişim elde etmek için **mümkündür**  

### MFA kod doğrulama süreci Brute Force saldırılarına karşı korunuyor mu?
- [ ] Evet — Birden fazla başarısız OTP denemesinden sonra katı Rate Limiting veya hesap kilitleme **uygulanmaktadır**  
- [ ] Evet — Rate Limiting mevcuttur ancak IP rotasyonu veya başlık manipülasyonu ile atlatılması **mümkündür**  
- [ ] Hayır — Herhangi bir Rate Limiting uygulanmamaktadır ve kodların otomatik Brute Force ile kırılması **mümkündür**  

### Uygulama, MFA geçişi sırasında güvenli bir oturum durumunu koruyor mu?
- [ ] Evet — Yüksek yetkili oturum belirteçleri (session tokens) **yalnızca** başarılı MFA tamamlandıktan sonra oluşturulur  
- [ ] Hayır — MFA tamamlanmadan **önce** tam işlevsel bir oturum çerezi (session cookie) oluşturularak bazı özelliklere erişime izin veriliyor  
- [ ] Hayır — Uygulama, ele geçirilebilecek statik bir tanımlayıcı veya öngörülebilir bir oturum geçişi kullanıyor  

### Alternatif MFA faktörleri (SMS, E-posta, Yedek Kodlar) istismara karşı savunmasız mı?
- [ ] Hayır — Tüm faktörler güvenli, öngörülemez ve zaman sınırlı değerler kullanıyor  
- [ ] Evet — Yedek kodlar öngörülebilirdir veya **numaralandırılabilir** (enumerate)  
- [ ] Evet — "Kodu Yeniden Gönder" işlevi, SMS/E-posta flooding yapmak veya kısmi iletişim bilgilerini ifşa etmek için kötüye kullanılabilir

---

## WSTG-ATHZ-01 — Dizin Gezginliği ve Dosya Dahil Etme Testi (Testing Directory Traversal File Include)

Dizin gezginliği (Directory Traversal) ve dosya dahil etme (File Inclusion) zafiyetleri, bir uygulama kullanıcı tarafından kontrol edilebilen girdileri, yeterli doğrulama veya temizleme (sanitization) yapmadan dosya veya dizin yollarını oluşturmak için kullandığında ortaya çıkar. Saldırganlar, hedeflenen dizinin dışına çıkmak için `../` gibi dizileri enjekte ederek bu açıklardan yararlanır ve hassas sistem dosyalarına, yapılandırma verilerine veya uygulama kaynak koduna erişebilirler. Yerel Dosya Dahil Etme (Local File Inclusion - LFI) veya Uzaktan Dosya Dahil Etme (Remote File Inclusion - RFI) içeren daha ciddi durumlarda, bir saldırgan kötü amaçlı betikler dahil ederek veya günlük zehirlemesi (log poisoning) ve PHP sarmalayıcılarından (wrappers) yararlanarak uzaktan kod yürütme (Exploit) gerçekleştirebilir. Bu zafiyetler genellikle dinamik içerik yükleme, şablon motorları veya sunucu tarafı mantığının dosya sistemi yollarını işlediği görüntü getirme uç noktalarında (endpoints) kullanılan parametrelerde bulunur.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-ATHZ-01 |
| **CWE** | CWE-22 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Kritik |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/01-Testing_Directory_Traversal_File_Include  
* https://hacktricks.wiki/en/pentesting-web/file-inclusion/index.html  
* https://portswigger.net/web-security/file-path-traversal  

**Araçlar:** `Burp Suite`, `FFUF`, `DotDotPwn`, `LFI Suite`, `Wfuzz`

### Parametreler sunucu tarafı işleme için dosya adlarını veya yollarını kabul ediyor mu?
- [ ] Hayır — hiçbir parametrenin dosya sistemiyle etkileşime girdiği görülmemektedir  
- [ ] Evet — parametreler mevcuttur ancak dosya tanımlayıcıları için katı bir izin verilenler listesi (allowlist) kullanmaktadır  
- [ ] Evet — parametreler doğrudan dosya adlarını veya yollarını kabul etmektedir  

### Girdi, dizin gezginliği dizilerini önlemek için temizleniyor (sanitized) mu?
- [ ] Evet — girdi katı bir izin verilenler listesine göre doğrulanmaktadır ve gezginlik dizileri **mümkün değildir**  
- [ ] Evet — girdi `../` dizileri kaldırılarak temizlenmektedir, ancak özyinelemeli (recursive) atlatma **mümkündür**  
- [ ] Hayır — yol ile ilgili girdilere hiçbir temizleme veya doğrulama **uygulanmamaktadır**  

### Gezginlik dizileri aracılığıyla kısıtlanmış dizinin dışındaki dosyalara erişmek mümkün mü?
- [ ] Hayır — uygulama veya işletim sistemi, tanımlanan kapsam dışındaki dosyalara erişimi engellemektedir  
- [ ] Evet — web kök dizini (web root) içindeki dosyalara erişim **mümkündür**  
- [ ] Evet — hassas sistem dosyalarına (örneğin `/etc/passwd`, `C:\Windows\win.ini`) erişim **mümkündür** *(Kritik)*  

### Sunucu, uzak URL'lerin dahil edilmesine (RFI) izin veriyor mu?
- [ ] Hayır — uzaktan dosya dahil etme, sunucu/uygulama düzeyinde **devre dışıdır**  
- [ ] Evet — uzak dosyalar dahil **edilebilir** ancak yürütme (execution) **mümkün değildir**  
- [ ] Evet — uzak dosyalar dahil **edilebilir** ve sunucuda yürütülebilir *(Kritik)*  

### Filtreler, kodlama (encoding) veya özel karakterler kullanılarak atlatılabilir mi?
- [ ] Hayır — filtreler çeşitli kodlamaları ve boş baytları (null bytes) etkili bir şekilde işlemektedir  
- [ ] Evet — atlatma; URL encoding, double URL encoding veya 16-bit Unicode kullanılarak **mümkündür**  
- [ ] Evet — atlatma; boş bayt enjeksiyonu (`%00`) veya dosya sistemi sarmalayıcıları (örneğin `php://filter`) kullanılarak **mümkündür**

---

## WSTG-ATHZ-02 — Yetkilendirme Şeması Atlatma Testi

Yetkilendirme şemalarının atlatılması, bir uygulamanın erişim kontrollerini zorunlu kılmada başarısız olması ve bir saldırganın amaçlanan izinlerinin dışındaki kaynaklara veya işlevlere erişmesine izin vermesi durumunda gerçekleşir. Bu zafiyet genellikle, bir saldırganın aynı seviyedeki başka bir kullanıcıya ait verilere eriştiği Yatay Yetki Yükseltme (Horizontal Privilege Escalation) veya düşük yetkili bir kullanıcının yönetici yetenekleri kazandığı Dikey Yetki Yükseltme (Vertical Privilege Escalation) olarak kendini gösterir. Saldırganlar, kaynak kimlikleri (Resource IDs) gibi istek parametrelerini manipüle ederek, oturum belirteçlerini (session tokens) değiştirerek veya yol tabanlı kısıtlamaları aşmak için `X-Original-URL` gibi HTTP başlıklarını (headers) kullanarak bu kusurlardan faydalanırlar. Güçlü bir yetkilendirme mekanizmasının sağlanması, veri gizliliğinin korunması ve tüm sistemi tehlikeye atabilecek yetkisiz yönetici eylemlerinin önlenmesi açısından kritiktir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-ATHZ-02 |
| **CWE** | CWE-285 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Şiddet** | Yüksek / Kritik |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/02-Testing_for_Bypassing_Authorization_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**Araçlar:** `Burp Suite (Autorize extension)`, `Burp Suite (Repeater/Intruder)`, `Postman`, `cURL`, `OWASP ZAP`

### Aynı roldeki kullanıcılar arasında yatay yetki yükseltme engelleniyor mu?
- [ ] Evet — yetkilendirme kontrolleri her kaynak isteğine **uygulanmaktadır** ve atlatma **mümkün değildir**  
- [ ] Evet — yetkilendirme kontrolleri **uygulanmaktadır** ancak IDOR veya parametre kurcalama (parameter tampering) yoluyla atlatılabilir  
- [ ] Hayır — kullanıcılar sadece bir kaynak kimliğini değiştirerek diğer kullanıcılara ait verilere **erişebilirler** *(Yüksek)*  

### Düşük yetkili kullanıcılar için dikey yetki yükseltme engelleniyor mu?
- [ ] Hayır — uygulama yalnızca tek bir yetki seviyesine sahiptir  
- [ ] Evet — yönetici işlevlerine düşük yetkili kullanıcılar tarafından **erişilemez**  
- [ ] Evet — yönetici işlevleri kullanıcı arayüzünde (UI) gizlidir ancak doğrudan URL üzerinden **erişilebilir**  
- [ ] Hayır — düşük yetkili kullanıcılar, rolleri veya izinleri manipüle ederek yönetici eylemlerini **gerçekleştirebilirler** *(Kritik)*  

### HTTP fiili (verb) kurcalama kullanılarak yetkilendirme atlatılabilir mi?
- [ ] Hayır — kullanılan HTTP yönteminden bağımsız olarak erişim kontrolleri zorunlu kılınmaktadır  
- [ ] Evet — yöntemi değiştirmek (örneğin `GET`'ten `POST` veya `PUT`'a) yetkilendirme kontrollerini **atlatır**  

### Yönetici veya kısıtlanmış uç noktalarına herhangi bir kimlik doğrulama olmadan erişilebiliyor mu?
- [ ] Hayır — tüm hassas uç noktaları (endpoints) geçerli bir oturum ve uygun izinler gerektirir  
- [ ] Evet — saldırgan doğrudan URL'yi biliyorsa bazı yönetici uç noktalarına erişilebilir  
- [ ] Evet — hassas işlevlere kimlik doğrulaması yapılmadan erişim **mümkündür** *(Kritik)*  

### HTTP başlıkları yol tabanlı yetkilendirmenin atlatılmasına izin veriyor mu?
- [ ] Hayır — uygulama başlık tabanlı (header-based) atlatmalara karşı duyarlı **değildir**  
- [ ] Evet — `X-Original-URL`, `X-Rewrite-URL` veya `X-Forwarded-For` gibi başlıklar erişim kontrollerini atlatmak için **kullanılabilir**

---

## WSTG-ATHZ-03 — Yetki Yükseltme Testleri (Testing for Privilege Escalation)

Yetki yükseltme (Privilege Escalation), bir saldırganın daha yüksek yetki seviyelerine veya farklı kimliklere sahip kullanıcılara ayrılmış kaynaklara veya işlevlere erişim sağlamak için güvenlik açıklarını istismar etmesiyle gerçekleşir. Dikey yetki yükseltmede (Vertical Escalation), standart bir kullanıcı yönetici işlevlerine erişmeye çalışırken; yatay yetki yükseltmede (Horizontal Escalation) ise aynı yetki seviyesindeki başka bir kullanıcıya ait verilere erişim söz konusudur. Bu kusurlar genellikle kötü yapılandırılmış erişim kontrol listelerinde (ACL), güvensiz doğrudan nesne referanslarında (IDOR) veya oturum veya kimlik belirteçlerindeki parametre manipülasyonlarında (Parameter Tampering) ortaya çıkar. Bir saldırgan açısından bu durum genellikle sayısal ID'lerin manipüle edilmesi, çerezlerdeki (Cookies) veya JWT'lerdeki rol tabanlı parametrelerin değiştirilmesi veya sunucu tarafında yetkilendirme kontrolü bulunmayan gizli API uç noktalarının (API Endpoints) doğrudan çağrılmasıyla elde edilir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-ATHZ-03 |
| **CWE** | CWE-269 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Kritik |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/03-Testing_for_Privilege_Escalation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**Araçlar:** `Burp Suite`, `Authorize`, `AuthMatrix`, `Authz`, `Postman`, `Ffuf`

### Uygulama birden fazla yetki seviyesi veya çoklu kiracılık (multi-tenancy) yapısını uyguluyor mu?
- [ ] Hayır — uygulama tek kullanıcılı veya tek rollü bir sistemdir  
- [ ] Evet — birden fazla rol veya kiracı tanımlanmıştır ve yetkilendirme testi gerektirir  

### Yatay yetki yükseltme (aynı seviye erişim) mümkün mü?
- [ ] Hayır — tüm kaynak tanımlayıcılarına ve oturum sahiplerine yetkilendirme kontrolleri uygulanmaktadır  
- [ ] Evet — IDOR veya parametre manipülasyonu yoluyla diğer kullanıcıların verilerine erişim mümkündür  
- [ ] Evet — veri sızıntısı mümkündür ancak diğer kullanıcıların verilerinin değiştirilmesi mümkün değildir  

### Dikey yetki yükseltme (düşükten yükseğe) mümkün mü?
- [ ] Hayır — yönetici uç noktaları rol tabanlı erişim kontrollerini (RBAC) sıkı bir şekilde uygulamaktadır  
- [ ] Evet — yönetici işlevlerine düşük yetkili kullanıcılar tarafından doğrudan URL erişimi ile erişilebilir  
- [ ] Evet — yönetici işlevlerine rolle ilgili başlıklar (headers), çerezler veya JWT talepleri (claims) manipüle edilerek erişilebilir  

### Kullanıcı rolleri veya izinleri Mass Assignment veya Parameter Pollution yoluyla değiştirilebilir mi?
- [ ] Hayır — rol tabanlı alanlar kesinlikle salt okunurdur ve kullanıcılar tarafından değiştirilemez  
- [ ] Evet — profil güncellemelerinde veya kayıt sırasında gizli parametrelerin (örn. `role=admin`) eklenmesiyle kullanıcı izinleri yükseltilebilir  

### Yetkilendirme kontrolleri tüm API versiyonlarında ve HTTP metodlarında tutarlı bir şekilde uygulanıyor mu?
- [ ] Evet — kontroller tüm versiyonlarda ve metodlarda tutarlı bir şekilde uygulanmaktadır  
- [ ] Hayır — eski API versiyonları (örn. `/v1/`) veya belirli HTTP metodları (örn. `PUT`, `DELETE`, `PATCH`) yetkilendirmeyi atlatmaktadır (bypass)  
- [ ] Hayır — yönetici işlevleri kullanıcı arayüzünde (UI) gizlenmiştir ancak API seviyesinde tüm kullanıcılar için etkindir

---

## WSTG-ATHZ-04 — Güvensiz Doğrudan Nesne Referanslarının (IDOR) Test Edilmesi

Güvensiz Doğrudan Nesne Referansları (Insecure Direct Object References - IDOR), bir uygulamanın, talepte bulunanın izinlerini doğrulamak için bir yetkilendirme kontrolü gerçekleştirmeden, kullanıcı tarafından sağlanan girdilere dayalı olarak nesnelere doğrudan erişim sağladığı durumlarda ortaya çıkar. Bu zafiyet, saldırganların diğer kullanıcılara veya sisteme ait verileri görüntülemesine, değiştirmesine veya silmesine olanak tanıdığı için gizlilik ve bütünlüğü önemli ölçüde etkiler. Genellikle dahili veritabanı anahtarlarına, dosya adlarına veya hesap tanımlayıcılarına atıfta bulunan URL parametrelerinde, POST gövde verilerinde veya JSON anahtarlarında görülür. Bir saldırgan açısından bakıldığında, sömürü süreci, yetkili kapsamlarının dışındaki kaynaklara erişmek için bu tanımlayıcıların (genellikle artan tamsayılar veya UUID'lerin Fuzzing yöntemiyle taranması yoluyla) manipüle edilmesini içerir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-ATHZ-04 |
| **CWE** | CWE-639 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/04-Testing_for_Insecure_Direct_Object_References  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/access-control  

**Araçlar:** `Burp Suite (Autorize extension)`, `Caido`, `Postman`, `ffuf`, `Intruder`

### Uygulama, istek parametrelerinde doğrudan nesne tanımlayıcıları kullanıyor mu?
- [ ] Hayır — uygulama dolaylı referanslar veya şifrelenmiş haritalar kullanıyor *(En güvenli)*  
- [ ] Evet — uygulama tahmin edilemeyen tanımlayıcılar kullanıyor (örneğin; uzun UUID'ler/hash'ler)  
- [ ] Evet — uygulama tahmin edilebilir tanımlayıcılar kullanıyor (örneğin; artan tamsayılar veya kullanıcı adları)  

### Nesne tanımlayıcısı içeren her istek için sunucu tarafı yetkilendirme kontrolü yapılıyor mu?
- [ ] Evet — sunucu tarafı kontrolleri hiçbir tanımlayıcı için atlatılamaz  
- [ ] Evet — sunucu tarafı kontrolleri mevcut ancak Parameter Pollution veya Header manipülasyonu yoluyla atlatılması mümkündür  
- [ ] Hayır — talep edilen nesnenin sahipliğini doğrulamak için hiçbir yetkilendirme kontrolü uygulanmıyor  

### Bir saldırgan, diğer kullanıcılara ait nesnelere erişebilir mi veya bunları değiştirebilir mi (Yatay IDOR)?
- [ ] Hayır — diğer kullanıcıların verilerine erişim mümkün değildir  
- [ ] Evet — diğer kullanıcıların verilerinin görüntülenmesi mümkündür ancak değiştirilmesi mümkün değildir  
- [ ] Evet — diğer kullanıcıların verilerinin hem görüntülenmesi hem de değiştirilmesi mümkündür *(Kritik)*  

### Yönetici veya sistem düzeyindeki nesnelere erişmek mümkün mü (Dikey IDOR)?
- [ ] Hayır — daha yüksek yetkili nesnelere erişim mümkün değildir  
- [ ] Evet — yönetici yapılandırmalarına veya sistem dosyalarına erişim mümkündür  

### Uygulama; arama, meta veriler veya diğer uç noktalar aracılığıyla nesne tanımlayıcılarını sızdırıyor mu?
- [ ] Hayır — tanımlayıcılar istenmeden ifşa edilmiyor  
- [ ] Evet — diğer kullanıcılar/nesneler için tanımlayıcılar, ikincil uç noktalar (endpoints) veya herkese açık profiller aracılığıyla numaralandırılabilir

---

## WSTG-ATHZ-05 — OAuth Zafiyetleri Testi

OAuth zafiyetleri, OAuth 2.0 veya OpenID Connect (OIDC) protokollerinin uygulanmasındaki çeşitli hataları kapsar ve genellikle tam hesap ele geçirme (account takeover) veya yetkisiz veri erişimi ile sonuçlanır. Bu güvenlik açıkları genellikle yetkilendirme akışı (authorization flow) sırasında, özellikle `redirect_uri` doğrulaması, `state` parametresinin entropisi ve doğrulanması ile erişim (access) veya kimlik (ID) token'larının güvenli işlenmesi konularında ortaya çıkar. Saldırganlar, yetkilendirme kodlarını ele geçirerek, açık yönlendirmeler (open redirect) aracılığıyla token'ları saldırgan kontrolündeki alan adlarına yönlendirerek veya bir mağdurun oturumunu saldırganın hesabına bağlamak için Siteler Arası İstek Sahteciliği (Cross-Site Request Forgery - CSRF) gerçekleştirerek bu açıklardan yararlanırlar. OAuth genellikle birincil kimlik doğrulama mekanizması olarak hizmet ettiğinden, tek bir hatalı yapılandırma tüm kullanıcı kimlik sisteminin bütünlüğünü tehlikeye atabilir.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-ATHZ-05 |
| **CWE** | CWE-287 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Kritik |

**Kaynaklar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/05-Testing_for_OAuth_Weaknesses  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/oauth  

**Araçlar:** `Burp Suite (Repeater/Collaborator)`, `CyberChef`, `OIDC-Checker`, `OAuth-Scanner`

### CSRF'yi önlemek için `state` parametresi uygulanmış ve doğrulanmış mı?
- [ ] Evet — `state` zorunludur, oturum başına benzersizdir ve sunucuda sıkı bir şekilde doğrulanır  
- [ ] Evet — `state` mevcuttur ancak statiktir veya farklı oturumlar arasında tahmin edilebilirdir  
- [ ] Evet — `state` mevcuttur ancak uygulama geri çağırma (callback) sırasında bunu **doğrulamaz**  
- [ ] Hayır — yetkilendirme isteğinde `state` parametresi **kullanılmamaktadır** *(Kritik)*  

### `redirect_uri`, bir beyaz liste (whitelist) üzerinden sıkı bir şekilde doğrulanıyor mu?
- [ ] Evet — yalnızca önceden kaydedilmiş bir beyaz listeye karşı tam eşleşmeler **kabul edilir**  
- [ ] Evet — doğrulama uygulanmaktadır ancak path traversal veya alt alan adı (subdomain) manipülasyonu ile atlatılması **mümkündür**  
- [ ] Evet — doğrulama uygulanmaktadır ancak parametre kirliliği (parameter pollution) veya fragment enjeksiyonu ile atlatılması **mümkündür**  
- [ ] Hayır — uygulama, `redirect_uri` parametresinde rastgele URL'lere **izin verir** *(Kritik)*  

### Akışı değiştirmek için `response_type` manipüle edilebilir mi?
- [ ] Hayır — uygulama yalnızca hedeflenen akışı (örneğin `code`) kabul eder ve diğerlerini reddeder  
- [ ] Evet — `code` yerine `token` (Implicit flow) kullanımına geçiş **mümkündür** ve token'ların URL üzerinden sızmasına neden olur  
- [ ] Evet — hibrit akışlar veya yetkisiz `response_type` değerleri **etkindir** ve işlenmektedir  

### Erişim token'ları veya yetkilendirme kodları, Referer başlıkları veya tarayıcı geçmişi aracılığıyla sızıyor mu?
- [ ] Hayır — token'lar/kodlar güvenli bir şekilde işlenir ve hassas sayfalarda uygun `Referrer-Policy` kullanılır  
- [ ] Evet — token'lar/kodlar `Referer` başlığı aracılığıyla üçüncü taraf alan adlarına **sızmaktadır**  
- [ ] Evet — güvensiz önbelleğe alma veya GET istekleri nedeniyle token'lar/kodlar tarayıcı geçmişinde **görünür** durumdadır  

### Uygulama, OAuth kapsamları (scopes) için "En Az Ayrıcalık" (Least Privilege) ilkesini uyguluyor mu?
- [ ] Evet — uygulama yalnızca işlevsellik için gerekli olan minimum kapsamları talep eder  
- [ ] Hayır — uygulama varsayılan olarak aşırı veya yönetici düzeyinde kapsamlar talep eder  
- [ ] Hayır — kullanıcı, onay (consent) aşamasında belirli kapsamları inceleyemez veya bunlardan **vazgeçemez**

---

## WSTG-SESS-01 — Oturum Yönetimi Şeması Testi

Oturum yönetimi şeması testi, uygulamanın oturum tanımlayıcılarının (session identifiers) yaşam döngüsünü ve yapısını, tahmin edilmeye ve ele geçirilmeye (hijacking) karşı dirençli olduklarından emin olmak amacıyla nasıl işlediğini analiz etmeye odaklanır. Saldırganlar; örüntüler, düşük entropi veya diğer kullanıcılar için geçerli oturumlar oluşturmalarına (forge) olanak tanıyan öngörülebilir artışlar aramak için birden fazla oturum belirteci (token) yakalayarak istatistiksel analiz gerçekleştirirler. Şemadaki zayıflıklar genellikle oturum sabitleme (Session Fixation) zafiyetleri veya genellikle HTTP çerezlerinde (cookies), URL parametrelerinde veya özel üstbilgi (header) uygulamalarında bulunan yetersiz rastgele değerlerin kullanımı şeklinde ortaya çıkar. Oturum şemasının tehlikeye atılması, bir saldırganın kurbanın kimlik doğrulaması yapılmış durumuna (authenticated state), kurbanın birincil kimlik bilgilerine ihtiyaç duymadan tam erişim sağlamasına olanak tanıyan yüksek etkili bir saldırıdır.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-SESS-01 |
| **CWE** | CWE-330 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/01-Testing_for_Session_Management_Schema  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Araçlar:** `Burp Suite (Sequencer)`, `OWASP ZAP`, `Python (pandas/numpy for entropy analysis)`, `Cookie Editor`

### Oturum tanımlayıcı yeterince karmaşık ve rastgele mi?
- [ ] Evet — belirteç (token) istatistiksel rastgelelik testlerini geçer (yüksek entropi) ve atlatılması (bypass) **mümkün değildir**  
- [ ] Evet — belirteç uzun ancak öngörülebilir örüntüler veya statik bölümler içeriyor  
- [ ] Hayır — belirteç sıralıdır, öngörülebilir zaman damgalarına dayanır veya **kritik düzeyde düşük** entropiye sahiptir *(Kritik)*  

### Oturum tanımlayıcı nerede iletilir ve saklanır?
- [ ] Tanımlayıcı, `Secure` ve `HttpOnly` bayraklarına sahip bir çerezde saklanır *(En güvenli)*  
- [ ] Tanımlayıcı bir çerezde saklanır ancak `Secure` veya `HttpOnly` bayraklarından yoksundur  
- [ ] Tanımlayıcı **yalnızca** URL parametreleri veya `GET` istekleri aracılığıyla iletilir *(Yüksek Risk)*  

### Uygulama, başarılı kimlik doğrulamasından sonra yeni bir oturum tanımlayıcı oluşturuyor mu?
- [ ] Evet — girişten hemen sonra yeni ve benzersiz bir oturum kimliği (Session ID) **oluşturulur**  
- [ ] Hayır — oturum kimliği girişten önce ve sonra aynı kalır *(Oturum Sabitleme/Session Fixation mümkündür)*  

### Yeniden kullanımı önlemek için oturum tanımlayıcı belirli bir IP adresine veya User-Agent bilgisine bağlı mı?
- [ ] Evet — istemcinin parmak izi (fingerprint) önemli ölçüde değişirse oturum geçersiz kılınır  
- [ ] Hayır — oturum, ek doğrulama olmaksızın farklı IP adresleri veya cihazlar arasında **tekrar kullanılabilir**  

### Oturum tanımlayıcıları, oturum kapatıldıktan veya zaman aşımına uğradıktan sonra sunucu tarafında düzgün bir şekilde geçersiz kılınıyor mu?
- [ ] Evet — oturum kapatıldığında sunucu tarafındaki oturum durumu **tamamen yok edilir**  
- [ ] Hayır — istemci tarafındaki çerez silinse bile oturum sunucuda geçerli kalmaya devam eder  
- [ ] Hayır — oturumun **süresi asla dolmaz** veya aşırı uzun bir zaman aşımı süresine sahiptir

---

## WSTG-SESS-02 — Çerez Özniteliklerinin Test Edilmesi

Oturum yönetimi (Session management), durumu korumak için genellikle HTTP çerezlerine (cookies) dayanır; bu da söz konusu çerezlerin güvenlik özniteliklerini (attributes) saldırganlar için birincil hedef haline getirir. Uygulamalar; `HttpOnly`, `Secure` ve `SameSite` gibi öznitelikleri atlayarak veya yanlış yapılandırarak; oturum belirteçlerini Cross-Site Scripting (XSS) yoluyla çalınmaya, şifrelenmemiş kanallar üzerinden ele geçirilmeye veya Cross-Site Request Forgery (CSRF) saldırılarına maruz bırakır. Sızma testi uzmanları (Pentesters), bir saldırganın tarayıcının doküman nesne modelinden (DOM) oturum tanımlayıcılarını sızdırıp sızdıramayacağını veya tarayıcıyı bunları çapraz kaynaklı (cross-origin) isteklerde göndermeye zorlayıp zorlayamayacağını belirlemek için bu bayrakları inceler. Uygun öznitelik yapılandırması, hassas belirteçlerin yetkili ve güvenli bağlamlarla sınırlı kalmasını sağlamak için temel bir derinlemesine savunma (defense-in-depth) önlemidir.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-SESS-02 |
| **CWE** | CWE-614 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/02-Testing_for_Cookies_Attributes  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Araçlar:** `Burp Suite (Proxy/Scanner)`, `OWASP ZAP`, `Browser Developer Tools (Storage Tab)`, `EditThisCookie`, `curl`

### Çerez Bayrağı Uygulamasının Analizi
| Öznitelik | Mevcut | Eksik |
|-----------|:-------:|:-------:|
| `HttpOnly` |  |  |
| `Secure` |  |  |
| `SameSite` |  |  |

### Hassas oturum çerezlerine `HttpOnly` bayrağı uygulanmış mı?
- [ ] Evet — **tüm** hassas oturum çerezlerine uygulanmış *(En güvenli)*  
- [ ] Evet — yalnızca bazı oturum çerezlerine uygulanmış  
- [ ] Hayır — bayrak **uygulanmamış**, istemci tarafı betikler (scripts) üzerinden erişime izin veriliyor *(Kritik)*  

### HTTPS üzerinden iletilen çerezler için `Secure` bayrağı zorunlu tutuluyor mu?
- [ ] Evet — TLS üzerinden iletilen **tüm** çerezlere uygulanmış  
- [ ] Hayır — bayrak **uygulanmamış**, çerezlerin düz metin HTTP üzerinden gönderilmesine izin veriliyor  

### CSRF risklerini azaltmak için `SameSite` özniteliği kullanılıyor mu?
- [ ] Evet — oturum çerezleri için `Strict` veya `Lax` olarak ayarlanmış  
- [ ] Evet — `None` olarak ayarlanmış ancak `Secure` bayrağı mevcut  
- [ ] Hayır — öznitelik **eksik** veya `Secure` bayrağı **olmadan** `None` olarak ayarlanmış  

### `Domain` ve `Path` öznitelikleri gerekli olan minimum kapsamla sınırlandırılmış mı?
- [ ] Evet — **belirli** alt alan adı (subdomain) ve uygulama yolu ile sınırlandırılmış  
- [ ] Hayır — kapsam çok **geniş** (örneğin, bir üst alan adına veya kök dizin `/` değerine ayarlanmış), bu da saldırı yüzeyini artırıyor  

### Hassas oturum çerezleri `Expires` veya `Max-Age` öznitelikleri aracılığıyla düzgün bir şekilde sonlandırılıyor mu?
- [ ] Evet — uygun son kullanma tarihleri ayarlanmış  
- [ ] Hayır — çerezler aşırı **uzun** ömürlü ve kalıcı (persistent)  
- [ ] Hayır — çerezler yalnızca oturum bazlı (session-only) ancak sunucu tarafında zaman aşımı (timeout) zorunluluğu **bulunmuyor**

---

## WSTG-SESS-03 — Oturum Sabitleme (Session Fixation)

Oturum Sabitleme (Session Fixation), bir uygulamanın kullanıcı başarıyla kimlik doğruladıktan sonra oturum tanımlayıcısını (session identifier) geçersiz kılmaması veya yenilememesi (rotate) durumunda meydana gelir ve bir saldırganın bilinen bir oturum belirtecini (session token) bir mağdura zorla kullandırmasına olanak tanır. Uygulama, giriş işleminden sonra kimlik doğrulama öncesi oturum kimliğini (session ID) koruyorsa, saldırgan mağdura sabit bir oturum kimliği içeren özel olarak hazırlanmış bir bağlantı gönderebilir ve ardından kimliği doğrulanmış oturumu ele geçirebilir (hijack). Bu güvenlik açığı genellikle giriş formlarında veya URL parametreleri ve çerezler (cookies) aracılığıyla kabul edilen oturum tanımlayıcılarında kendini gösterir. Bir saldırganın perspektifinden bakıldığında, istismar süreci kötü amaçlı bir bağlantı veya bir başlık enjeksiyonu (header injection) güvenlik açığı aracılığıyla oturumu "sabitlemeyi" ve mağdurun kimlik bilgilerini girmesini beklemeyi içerir; bu da canlı bir belirteci çalma ihtiyacını etkili bir şekilde ortadan kaldırır.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-SESS-03 |
| **CWE** | CWE-384 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/03-Testing_for_Session_Fixation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite (Proxy/Repeater)`, `OWASP ZAP`, `EditThisCookie`, `Curl`

### Başarılı kimlik doğrulamasından sonra oturum tanımlayıcısı değişiyor mu?
- [ ] Evet — yeni bir oturum tanımlayıcısı **atanır** ve eskisi geçersiz kılınır *(En güvenli)*  
- [ ] Evet — yeni bir oturum tanımlayıcısı **atanır** ancak eskisi **geçerli kalmaya devam eder**  
- [ ] Hayır — oturum tanımlayıcısı kimlik doğrulama öncesinde ve sonrasında **aynı kalır** *(Kritik)*  

### Uygulama, URL parametreleri aracılığıyla sağlanan oturum tanımlayıcılarını kabul ediyor mu?
- [ ] Hayır — oturum kimlikleri (session IDs) **yalnızca** çerezler aracılığıyla yönetilir  
- [ ] Evet — oturum kimlikleri URL'de kabul edilir ancak giriş yapıldığında **yok sayılır** veya **yenilenir**  
- [ ] Evet — URL'deki oturum kimlikleri **kabul edilir** ve kimlik doğrulamasından sonra **korunur**  

### Saldırgan tarafından tanımlanan bir oturum kimliği uygulamaya dayatılabilir mi?
- [ ] Hayır — uygulama, kullanıcı tarafından sağlanan geçersiz veya mevcut olmayan oturum kimliklerini **reddeder**  
- [ ] Evet — uygulama, bir çerezde sağlanan herhangi bir rastgele kimliği kullanarak bir oturumu **kabul eder** ve başlatır  
- [ ] Evet — uygulama, bir URL parametresinde sağlanan herhangi bir rastgele kimliği kullanarak bir oturumu **kabul eder** ve başlatır  

### Kimlik doğrulama öncesi oturumlar doğru şekilde sonlandırılıyor mu?
- [ ] Evet — anonim oturum, giriş işlemi gerçekleştiğinde **tamamen yok edilir**  
- [ ] Hayır — anonim oturum **yok edilmez**, bu da potansiyel oturum toplama (session harvesting) işlemlerine izin verir  

### Oturum çerezi istemci tarafı enjeksiyonuna karşı korunuyor mu?
- [ ] Evet — betik tabanlı sabitlemeyi önlemek için `HttpOnly` ve `Secure` bayrakları **uygulanmıştır**  
- [ ] Hayır — bayraklar **uygulanmamıştır**, bu da oturum kimliklerinin Cross-Site Scripting (XSS) üzerinden ayarlanmasına izin verir

---

## WSTG-SESS-04 — Açığa Çıkan Oturum Değişkenlerinin Test Edilmesi

Açığa çıkan oturum değişkenleri (Exposed session variables), hassas oturum tanımlayıcılarının veya durumla ilgili verilerin; URL sorgu dizileri (URL query strings), Referer başlıkları veya sistem günlükleri (system logs) gibi güvenli olmayan kanallar üzerinden iletildiği durumlarda meydana gelir. Bu durum, tanımlayıcıların tarayıcı geçmişinde önbelleğe alınabilmesi, ara proxy'ler tarafından günlüğe kaydedilebilmesi veya Referer başlığı aracılığıyla üçüncü taraf sitelere sızabilmesi nedeniyle oturum çalma (session hijacking) riskini önemli ölçüde artırır. Penetrasyon test uzmanları (Pentesters), tüm giden istekleri izleyerek ve uygulama günlüklerini veya sunucu yapılandırmalarını kasıtsız veri sızıntılarına karşı inceleyerek bu açıkları tespit ederler. Gerçek dünya senaryolarında, saldırganlar bu sızıntıları paylaşılan makinelerden geçerli oturum belirteçlerini (session tokens) toplayarak veya kimlik doğrulamayı atlatmak ve kullanıcı hesaplarına yetkisiz erişim sağlamak amacıyla trafik modellerini analiz ederek istismar (exploit) ederler.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-SESS-04 |
| **CWE** | CWE-598 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek* |

> *Açığa çıkan değişkenin, doğrudan hesap ele geçirmeye (account takeover) izin veren bir oturum belirteci (session token) olması durumunda önem derecesi Yüksek (High) olarak değerlendirilir.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/04-Testing_for_Exposed_Session_Variables  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Araçlar:** `Burp Suite (Proxy/Logger)`, `OWASP ZAP`, `Browser DevTools`, `grep`

### Oturum tanımlayıcı (session identifier) URL sorgu dizisi (query string) içinde iletiliyor mu?
- [ ] Hayır — oturum tanımlayıcıları URL sorgu dizisinde bulunmamaktadır.
- [ ] Evet — tanımlayıcılar mevcut ancak oturum kısa süreli veya düşük risklidir.
- [ ] Evet — benzersiz oturum ID'leri (unique session IDs) URL sorgu dizisinde bulunmaktadır *(Kritik)*.

### Uygulama, Referer başlığı üzerinden oturum belirteçlerini (session tokens) sızdırıyor mu?
- [ ] Hayır — `Referrer-Policy` sızıntıyı önlüyor veya üçüncü taraf bağlantısı bulunmamaktadır.
- [ ] Evet — oturum belirteçleri Referer başlığı aracılığıyla üçüncü taraf alan adlarına iletilmektedir.

### Oturum değişkenleri sunucu tarafı veya uygulama günlüklerinde (logs) saklanıyor mu?
- [ ] Hayır — oturum değişkenleri maskelenmiş veya günlüklerden hariç tutulmuştur.
- [ ] Evet — oturum değişkenleri uygulama veya web sunucusu günlüklerine düz metin (plain text) olarak kaydedilmektedir.

### Uygulama, durum değiştiren işlemler için GET isteklerini kullanıyor mu?
- [ ] Hayır — uygulama, hassas işlemler için `POST` veya diğer idempotent olmayan yöntemleri kullanmaktadır.
- [ ] Evet — `GET` kullanılmaktadır; bu durum oturum verilerinin tarayıcı geçmişinde ve ara proxy'lerde önbelleğe alınmasına neden olmaktadır.

### `Cache-Control` başlığı, oturumla ilgili verilerin önbelleğe alınmasını önleyecek şekilde yapılandırılmış mı?
- [ ] Evet — hassas yanıtlara `Cache-Control: no-store` (veya benzeri) uygulanmıştır.
- [ ] Evet — kontroller mevcuttur ancak tarayıcı uç durumları (edge cases) üzerinden atlatma (bypass) mümkündür.
- [ ] Hayır — oturum verilerini içeren hassas yanıtlar tarayıcı tarafından önbelleğe alınabilmektedir.

---

## WSTG-SESS-05 — Testing for Cross Site Request Forgery

Cross-Site Request Forgery (Siteler Arası İstek Sahteciliği - CSRF), bir saldırganın mağdurun tarayıcısını, mağdurun halihazırda kimlik doğrulaması yaptığı farklı bir web sitesinde istenmeyen bir eylem gerçekleştirmeye zorladığı bir zafiyettir. Bu istismar (exploit), tarayıcının oturum çerezleri (session cookies) veya Authorization (Yetkilendirme) üstbilgileri gibi "ortam" kimlik bilgilerini (ambient credentials) giden isteklere otomatik olarak ekleme davranışından yararlanır. Saldırganlar genellikle üçüncü taraf bir sitede kötü amaçlı betikler veya gizli formlar barındırarak şifre değiştirme, e-posta adresi güncelleme veya finansal transferleri yürütme gibi durum değiştiren (state-changing) işlemleri hedef alır. Başarılı bir istismar, kullanıcının bilgisi veya rızası olmadan tam hesap ele geçirme (account takeover) veya yetkisiz veri değişikliği ile sonuçlanabilir.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-SESS-05 |
| **CWE** | CWE-352 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek* |

> *Zafiyetin savunmasız eylemi; hesap ele geçirme, yetki yükseltme (privilege escalation) veya yetkisiz finansal işlemlere izin vermesi durumunda önem derecesi Yüksek olur.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/05-Testing_for_Cross_Site_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/csrf-cross-site-request-forgery.html  
* https://portswigger.net/web-security/csrf  

**Araçlar:** `Burp Suite Professional (CSRF PoC Generator)`, `OWASP ZAP`, `CSRFTester`, `python3 (SimpleHTTPServer for PoC hosting)`

### Durum değiştiren istekler için anti-CSRF token'ları uygulanmış mı?
- [ ] Evet — tüm durum değiştiren eylemler için benzersiz, kriptografik olarak güçlü token'lar zorunludur  
- [ ] Evet — token'lar mevcut ancak oturum başına benzersiz değil veya tahmin edilebilir  
- [ ] Hayır — anti-CSRF token'ları uygulanmamış  

### Anti-CSRF token'ının sunucu tarafı doğrulaması sağlam mı?
- [ ] Evet — sunucu token'ı sıkı bir şekilde doğrular ve atlatma (bypass) mümkün değildir  
- [ ] Evet — doğrulama gerçekleştiriliyor ancak token parametresi kaldırılarak atlatılabiliyor  
- [ ] Evet — doğrulama gerçekleştiriliyor ancak boş veya sahte (dummy) bir token sağlanarak atlatılabiliyor  
- [ ] Evet — doğrulama gerçekleştiriliyor ancak token kullanıcının oturumuna bağlı değil  

### Uygulama, CSRF koruması için kolayca atlatılabilir yöntemlere mi güveniyor?
- [ ] Hayır — uygulama yalnızca zayıf üstbilgilere (headers) veya kaynak (origin) kontrollerine güvenmiyor  
- [ ] Evet — koruma yalnızca sahtesi oluşturulabilen veya silinebilen `Referer` veya `Origin` üstbilgisine dayanıyor  
- [ ] Evet — koruma `X-Requested-With` üstbilgisinin kontrolüne dayanıyor ve bu, CORS yapılandırma hataları aracılığıyla atlatılabiliyor  

### Oturum çerezleri `SameSite` özniteliği ile yapılandırılmış mı?
- [ ] Evet — tüm oturumla ilgili çerezlerde `SameSite` değeri `Strict` veya `Lax` olarak ayarlanmış  
- [ ] Hayır — `SameSite` özniteliği eksik, tarayıcıya özgü varsayılan davranışa bırakılmış  
- [ ] Hayır — `SameSite` değeri, `Secure` bayrağı veya ek azaltıcı önlemler olmaksızın açıkça `None` olarak ayarlanmış  

### CSRF koruması, HTTP istek yöntemi değiştirilerek atlatılabilir mi?
- [ ] Hayır — koruma, kullanılan HTTP yönteminden bağımsız olarak zorunlu kılınmıştır  
- [ ] Evet — token'lar yalnızca `POST` isteklerinde doğrulanıyor, ancak eylem `GET` üzerinden gerçekleştirilebiliyor  
- [ ] Evet — yöntemi değiştirmek (örneğin `PUT` veya `DELETE` olarak) token kontrolünü atlatıyor

---

## WSTG-SESS-06 — Oturumu Kapatma İşlevinin Test Edilmesi

Oturumu kapatma (Logout) işlevi, bir kullanıcının oturumunu sonlandırmak ve hem istemci hem de sunucu tarafındaki ilgili oturum tanımlayıcılarını (Session Identifiers) geçersiz kılmak için tasarlanmış kritik bir güvenlik kontrolüdür. Oturumu kapatma işleminin düzgün bir şekilde uygulanmaması, oturum sabitleme (Session Fixation) veya oturum ele geçirme (Hijacking) saldırılarına olanak tanır; çünkü bir kullanıcı "oturumu kapattıktan" sonra makineye erişim sağlayan bir saldırgan, hala aktif olan oturum belirtecini (Session Token) potansiyel olarak yeniden kullanabilir. Sızma testi uzmanları (Pentesters), bu durumu oturum çerezinin tarayıcıdan temizlenip temizlenmediğini, sunucu tarafındaki oturum durumunun açıkça yok edilip edilmediğini ve geri butonu ile gezinmenin önbelleğe alınmış hassas verilere erişime izin verip vermediğini kontrol ederek değerlendirir. Güvenli bir oturum kapatma uygulaması, eylem tetiklendikten sonra eski oturum belirteci ile yapılan sonraki hiçbir isteğin uygulama tarafından yetkilendirilmemesini sağlar.

| Alan | Değer |
|---|---|
| **WSTG Kimliği** | WSTG-SESS-06 |
| **CWE** | CWE-613 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/06-Testing_for_Logout_Functionality  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Araçlar:** `Burp Suite (Repeater/Proxy)`, `Browser Developer Tools`, `OWASP ZAP`

### İşlevsel bir oturumu kapatma mekanizması mevcut mu ve erişilebilir mi?
- [ ] Evet — oturumu kapatma butonu mevcut ve bir sonlandırma isteğini tetikliyor  
- [ ] Hayır — oturumu kapatma butonu **eksik** veya bir sonlandırma olayını **tetiklemiyor**  

### Oturum tanımlayıcı (Session Identifier) sunucu tarafında geçersiz kılınıyor mu?
- [ ] Evet — sunucu, eski oturum belirtecini kullanan sonraki tüm istekleri reddediyor  
- [ ] Hayır — sunucu, oturumu kapattıktan sonra eski oturum belirtecini **kabul etmeye devam ediyor**  

### Uygulama, oturumu kapattıktan sonra tarayıcıdaki oturum çerezlerini (Cookies) temizliyor mu?
- [ ] Evet — çerezlerin üzerine sona erme tarihi geçmiş veya boş bir değer yazılıyor  
- [ ] Evet — çerezler kalıyor ancak sunucuda **artık geçerli değil**  
- [ ] Hayır — çerezler tarayıcıda kalıyor ve orijinal değerlerini **koruyor**  

### Oturumu kapattıktan sonra tarayıcının "Geri" butonu aracılığıyla kimliği doğrulanmış hassas içeriğe erişilebiliyor mu?
- [ ] Hayır — `Cache-Control: no-store` veya benzeri başlıklar (Headers), oturum kapatıldıktan sonra hassas sayfaların görüntülenmesini engelliyor  
- [ ] Evet — hassas sayfalar önbelleğe alınıyor ve oturum sonlandırıldıktan sonra navigasyon yoluyla **görüntülenebiliyor**

---

## WSTG-SESS-07 — Oturum Zaman Aşımının Test Edilmesi (Testing Session Timeout)

Oturum zaman aşımı testi, bir uygulamanın önceden tanımlanmış bir hareketsizlik süresinden veya toplam süreden sonra kullanıcının oturumunu etkili bir şekilde sonlandırıp sonlandırmadığını değerlendirir. Bu kontrol; özellikle ortak kullanılan iş istasyonlarında veya bir saldırganın ağ dinleme (network interception) ya da Cross-Site Scripting (XSS) yoluyla bir oturum tanımlayıcısını ele geçirdiği senaryolarda Oturum Ele Geçirme (Session Hijacking) riskini azaltmak için kritiktir. Sızma testi uzmanları (pentesters), hem hareketsizlik süresinden sonra tetiklenen boşta kalma zaman aşımlarını (idle timeouts), hem de aktiviteden bağımsız olarak bir oturumun toplam ömrünü sınırlayan mutlak zaman aşımlarını (absolute timeouts) analiz eder. Bir saldırganın bakış açısından, belirsiz veya aşırı uzun oturumlar, yetkisiz erişimi sürdürmek ve yeniden kimlik doğrulama gereksinimini atlatmak için genişletilmiş bir fırsat penceresi sunar.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-SESS-07 |
| **CWE** | CWE-613 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/07-Testing_Session_Timeout  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Browser DevTools`

### Uygulama bir boşta kalma oturum zaman aşımı (idle session timeout) uyguluyor mu?
- [ ] Evet — oturum, kısa bir hareketsizlik süresinden (örn. 15-30 dakika) sonra sona eriyor  
- [ ] Evet — oturum sona eriyor ancak zaman aşımı süresi **aşırı uzun** (örn. > 24 saat)  
- [ ] Hayır — oturum, hareketsizlik sırasında süresiz olarak **aktif kalıyor**  

### Oturum zaman aşımı sunucu tarafında (server-side) zorunlu kılınıyor mu?
- [ ] Evet — sunucu, istemci tarafındaki durumdan bağımsız olarak süresi dolmuş belirteçlere (tokens) sahip istekleri reddediyor  
- [ ] Hayır — zaman aşımı **yalnızca** istemci tarafı JavaScript veya meta-refresh aracılığıyla zorunlu kılınıyor  

### Uygulama mutlak bir oturum zaman aşımı (absolute session timeout) uyguluyor mu?
- [ ] Evet — oturum, aktiviteden bağımsız olarak sabit bir maksimum süreden sonra sonlandırılıyor  
- [ ] Hayır — sürekli aktivite olduğu sürece oturum süresiz olarak **uzatılabiliyor**  

### Zaman aşımı durumunda oturum tanımlayıcısı sunucuda geçersiz kılınıyor mu?
- [ ] Evet — zaman aşımı eşiğine ulaşıldığında oturum belirteci (session token) **tekrar kullanılamaz**  
- [ ] Hayır — istemci oturum açma sayfasına yönlendirildikten sonra bile oturum belirteci sunucuda **hâlâ geçerlidir**  

### Otomatik kalp atışı (heartbeat) istekleri kullanılarak oturum süresiz olarak canlı tutulabilir mi?
- [ ] Hayır — mutlak zaman aşımı veya ikincil kontroller oturumun süresiz uzatılmasını **engelliyor**  
- [ ] Evet — periyodik arka plan istekleri (örn. AJAX), oturumu süresiz olarak **sürdürebilir**

---

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

## WSTG-SESS-10 — JSON Web Token'ların (JWT) Test Edilmesi

JSON Web Token'lar (JWT), durumluksuz (stateless) oturum yönetimi ve kimlik yayılımı için yaygın olarak kullanılır; ancak güvenlikleri tamamen imzalama algoritmalarının doğru uygulanmasına ve katı talep (claim) doğrulamasına bağlıdır. Saldırganlar sıklıkla "none" algoritması kusurlarını istismar ederek, zayıf HS256 gizli anahtarlarına Brute Force (kaba kuvvet) uygulayarak veya asimetrik bir genel anahtarın (public key) simetrik bir HMAC gizli anahtarı olarak kullanıldığı algoritma değiştirme (algorithm-switching) saldırıları gerçekleştirerek kimlik doğrulamayı atlatmaya çalışırlar. Bu zafiyetler tipik olarak oturum çerezlerinde (session cookies) veya Authorization başlıklarında (headers) kendini gösterir; bu da bir saldırganın, token'ın kriptografik bütünlüğünü bozmadan `role` veya `user_id` gibi talepleri değiştirerek yetki yükseltmesine (privilege escalation) veya herhangi bir kullanıcıyı taklit etmesine (impersonate) olanak tanır.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-SESS-10 |
| **CWE** | CWE-347 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Kritik |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/10-Testing_JSON_Web_Tokens  
* https://hacktricks.wiki/en/pentesting-web/hacking-jwt-json-web-tokens.html  
* https://portswigger.net/web-security/jwt  

**Araçlar:** `jwt_tool`, `Burp Suite (JWT Editor Extension)`, `hashcat`, `john`, `jose`

### "none" algoritması sunucu tarafından kabul ediliyor mu?
- [ ] Hayır — sunucu, başlıkta `alg: none` kullanan token'ları **reddediyor**  
- [ ] Evet — sunucu, `alg: none` ve boş bir imza içeren token'ları **kabul ediyor** *(Kritik)*  

### JWT imzası nasıl doğrulanıyor ve brute-force saldırılarına karşı nasıl korunuyor?
- [ ] Evet — güçlü asimetrik anahtarlar (RS256/ES256) veya yüksek entropili HMAC gizli anahtarları kullanılıyor  
- [ ] Evet — HS256 kullanılıyor ancak gizli anahtar, düşük entropi veya varsayılan değerler nedeniyle Brute Force ile **ele geçirilebilir**  
- [ ] Hayır — imza doğrulaması **uygulanmıyor** veya algoritma değiştirme (RS256'dan HS256'ya) yoluyla **atlatılabiliyor**  

### Standart talepler (exp, nbf, iat) arka uç tarafından sıkı bir şekilde denetleniyor mu?
- [ ] Evet — `exp` (son kullanma süresi) mevcut ve **atlatılamıyor**  
- [ ] Evet — `exp` mevcut ancak sunucu doğrulama sırasında bunu **zorunlu tutmuyor**  
- [ ] Hayır — son kullanma talepleri **eksik** veya görmezden geliniyor, bu da süresiz oturum tekrarına (session replay) izin veriyor  

### Uygulama, başlık tabanlı anahtar enjeksiyonunu (kid, jku, x5u) engelliyor mu?
- [ ] Evet — başlıklar temizleniyor ve yalnızca güvenilir dahili anahtarlara/kaynaklara **izin veriliyor**  
- [ ] Hayır — `kid` (Key ID) başlığı, bilinen bir dosyayı işaret etmek için dizin atlatma (directory traversal) veya SQL Injection saldırılarına karşı savunmasız  
- [ ] Hayır — `jku` veya `x5u` başlıkları, kötü amaçlı anahtarları yüklemek için saldırgan kontrolündeki URL'lere **yönlendirilebilir**

---

## WSTG-SESS-11 — Eşzamanlı Oturumların (Concurrent Sessions) Test Edilmesi

Eşzamanlı oturum testi (Concurrent session testing), bir uygulamanın tek bir kullanıcı hesabının farklı tarayıcılar, cihazlar veya IP adresleri üzerinden aynı anda birden fazla aktif oturum sürdürmesine izin verip vermediğini değerlendirir. Eşzamanlı oturumların sınırlandırılmaması, saldırganların ele geçirilmiş oturum token'larını (session tokens) veya tehlikeye atılmış kimlik bilgilerini, asıl kullanıcıyı uyarmadan veya mevcut oturumları sonlandırmadan kullanma fırsatını artırır. Bu davranış genellikle birden fazla kaynaktan kimlik doğrulaması yapılarak ve oturum kararlılığı izlenerek değerlendirilir; bu durum genellikle oturum yönetimi (session management) mantığındaki zayıflıkları veya güvenlik odaklı iş kurallarının eksikliğini ortaya çıkarır. Bir saldırganın bakış açısıyla, eşzamanlılık kontrolünün eksikliği, ilk sızmadan sonra gizli bir kalıcılık (persistence) sağlar ve paralel otomatik saldırıları kolaylaştırır.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-SESS-11 |
| **CWE** | CWE-613 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Şiddet** | Düşük / Orta |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/11-Testing_for_Concurrent_Sessions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite (Repeater/Containers)`, `Firefox Multi-Account Containers`, `Postman`, `cURL`

### Eşzamanlı oturumlar uygulama politikası tarafından sınırlandırılmış mı?
- [ ] Evet — kullanıcı başına yalnızca bir oturuma **izin verilir**; yeni girişler eskileri geçersiz kılar *(En güvenli)*  
- [ ] Evet — belirli bir sayıda eşzamanlı oturum **zorunlu tutulur** (enforced) ve bu sayı aşılamaz  
- [ ] Hayır — sınırsız sayıda eşzamanlı oturum **mümkündür**  

### Oturum sınırına ulaşıldığında uygulama nasıl tepki verir?
- [ ] En eski aktif oturum **otomatik olarak sonlandırılır** (session fixation/hijacking azaltma)  
- [ ] Yeni oturum yetkilendirilmeden önce kullanıcıdan mevcut oturumları sonlandırması **istenir**  
- [ ] Başka bir cihazdan manuel çıkış yapılana kadar yeni giriş denemesi **engellenir**  
- [ ] Hiçbir işlem yapılmaz — birden fazla oturum aynı anda **etkin** kalmaya devam eder  

### Uygulama, kullanıcı için bir oturum yönetimi arayüzü sağlıyor mu?
- [ ] Evet — kullanıcılar aktif oturumları (IP, cihaz, zaman) **görüntüleyebilir** ve bunları uzaktan sonlandırabilir  
- [ ] Evet — kullanıcılar aktif oturumları **görüntüleyebilir** ancak bunları uzaktan sonlandıramaz  
- [ ] Hayır — kullanıcılar, hesaplarıyla ilişkili diğer aktif oturumları **göremez** veya yönetemez  

### Eşzamanlı giriş faaliyeti tespit edildiğinde kullanıcı bilgilendiriliyor mu?
- [ ] Evet — yeni cihazlardan veya konumlardan yapılan girişler için bir uyarı (e-posta, SMS veya uygulama içi) **tetiklenir**  
- [ ] Hayır — eşzamanlı oturumlar oluşturulduğunda herhangi bir bildirim **sağlanmaz**

---

## WSTG-INPV-01 — Yansıtılmış Siteler Arası Betik Çalıştırma (Reflected Cross Site Scripting - XSS)

Yansıtılmış Siteler Arası Betik Çalıştırma (Reflected Cross Site Scripting - XSS), bir uygulama güvenilmeyen verileri yeterli doğrulama veya kodlama (encoding) yapmadan bir HTTP yanıtına dahil ettiğinde ve payload'un kurbanın tarayıcı bağlamında çalışmasına neden olduğunda meydana gelir. Saldırganlar; kullanıcı oturumlarını ele geçirmek, hassas çerezleri (cookies) sızdırmak veya kullanıcı adına yetkisiz eylemler gerçekleştirmek için sosyal mühendislik yoluyla, genellikle özel olarak hazırlanmış URL'ler veya formlar aracılığıyla kötü amaçlı payload'lar iletirler. Bu zafiyet; arama parametrelerinde, hata mesajlarında ve girdiyi doğrudan kullanıcı arayüzüne geri yansıtan her türlü uç noktada (endpoint) yaygın olarak bulunur. Sömürü (exploitation), kurbanın kötü amaçlı bir bağlantıyla etkileşime girmesine dayanır; bu da onu kalıcı olmayan (non-persistent) ancak belirli yönetici veya kimliği doğrulanmış kullanıcıları hedeflemek için oldukça etkili bir saldırı vektörü haline getirir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INPV-01 |
| **CWE** | CWE-79 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/01-Testing_for_Reflected_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/cross-site-scripting  

**Araçlar:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `XSStrike`, `KXS`, `dalfox`

### Kullanıcı tarafından sağlanan girdiler yanıt gövdesinde (response body) yansıtılıyor mu?
- [ ] Hayır — girdi kullanıcıya **hiçbir zaman** geri yansıtılmıyor  
- [ ] Evet — girdi yansıtılıyor ancak uygun şekilde kodlanmış (encoded) ve atlatma (bypass) **mümkün değil**  
- [ ] Evet — girdi herhangi bir kodlama veya temizleme (sanitization) **olmadan** yansıtılıyor  

### Bağlam duyarlı çıktı kodlaması (context-aware output encoding) uygulanmış mı?
- [ ] Evet — belirli yansıma bağlamına göre uygun kodlama (HTML, Attribute, JavaScript) **uygulanmış**  
- [ ] Evet — kodlama **uygulanmış** ancak belirli bağlam için yetersiz (örneğin, bir `<script>` etiketi içinde HTML kodlaması)  
- [ ] Hayır — kodlama **uygulanmamış**  

### Girdi doğrulaması veya Web Uygulaması Güvenlik Duvarı (WAF) filtreleri atlatılabilir (bypass) mi?
- [ ] Hayır — filtreler tüm yaygın ve gizlenmiş (obfuscated) XSS payload'larını etkili bir şekilde engelliyor  
- [ ] Evet — filtreler mevcut ancak karakter kodlaması, standart dışı etiketler veya poliglolar (polyglots) kullanılarak atlatma (bypass) **mümkün**  
- [ ] Hayır — hiçbir filtre veya doğrulama mekanizması devrede değil  

### Yansıtılan girdinin yürütme bağlamı (execution context) nedir?
- [ ] Güvenli — girdi, yürütülemez bir konumda yansıtılıyor (örneğin, standart `<div>` veya `<span>` etiketleri içinde)  
- [ ] Riskli — girdi HTML öznitelikleri (attributes) içinde veya etiketlerin `src`/`href` bölümlerinde yansıtılıyor  
- [ ] Kritik — girdi doğrudan `<script>` blokları, olay işleyicileri (event handlers) veya şablon sabitleri (template literals) içinde yansıtılıyor  

### XSS etkisini azaltmak için modern tarayıcı güvenlik başlıkları (security headers) mevcut mu?
- [ ] Evet — `Content-Security-Policy` (CSP), kısıtlayıcı bir script-src ile **etkinleştirilmiş**  
- [ ] Evet — `Content-Security-Policy` (CSP) **etkinleştirilmiş** ancak `unsafe-inline` veya zayıf beyaz listeler (whitelists) içeriyor  
- [ ] Hayır — hiçbir CSP veya eski `X-XSS-Protection` başlığı mevcut değil

---

## WSTG-INPV-02 — Depolanmış Cross Site Scripting (XSS)

Kalıcı XSS (Persistent XSS) olarak da bilinen Depolanmış Cross Site Scripting (XSS), bir uygulamanın bir kullanıcıdan veri alması ve bu veriyi yeterli doğrulama (validation) veya kodlama (encoding) yapmadan kalıcı bir veritabanında, dosya sisteminde veya diğer depolama ortamlarında saklaması durumunda oluşur. Bir kurban, daha sonra bu arındırılmamış veriyi geri çağıran ve görüntüleyen bir sayfaya gittiğinde, kötü niyetli betik (script), kurbanın tarayıcı bağlamında uygulamanın kaynağı (origin) altında çalışır. Bu güvenlik zafiyeti, kurbanın özel olarak hazırlanmış bir bağlantıya tıklamasını gerektirmediği için özellikle tehlikelidir; etkilenen sayfayı görüntüleyen herhangi bir kullanıcı hedef haline gelir ve bu durum oturum ele geçirme (session hijacking), yetkisiz işlemler veya kimlik bilgisi toplama (credential harvesting) gibi sonuçlara yol açabilir. Saldırganlar genellikle girdinin diğer kullanıcılar veya yöneticiler için sürekli olarak işlendiği yorum bölümlerini, kullanıcı profillerini, mesaj panolarını ve yönetim günlüklerini hedef alırlar.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INPV-02 |
| **CWE** | CWE-79 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/02-Testing_for_Stored_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/cross-site-scripting  

**Araçlar:** `Burp Suite`, `XSStrike`, `BeEF`, `KXSStrike`, `Sleepy Puppy`

### Kullanıcı girdisini daha sonra diğer kullanıcılara göstermek üzere depolayan giriş noktaları var mı?
- [ ] Hayır — uygulama, daha sonra işlenmek üzere kullanıcı tarafından kontrol edilebilen bir girdi depolamamaktadır
- [ ] Evet — girdi depolanıyor (örneğin profiller, yorumlar) ancak bir HTML bağlamında **işlenmiyor**
- [ ] Evet — girdi depolanıyor ve diğer kullanıcılara veya yöneticilere sunuluyor

### Depolanan veriler işlendiğinde çıktı kodlaması (output encoding) veya arındırma (sanitization) uygulanıyor mu?
- [ ] Evet — bağlam duyarlı (context-aware) çıktı kodlaması **uygulanıyor** ve bypass **mümkün değil** *(En güvenli)*
- [ ] Evet — arındırma (örneğin `DOMPurify`) **uygulanıyor** ancak yapılandırma hataları nedeniyle bypass **mümkün**
- [ ] Hayır — veriler, kodlama veya arındırma yapılmadan doğrudan DOM içerisine **aktarılıyor** *(Yüksek)*

### Girdi doğrulaması veya arındırma, alternatif payload'lar kullanılarak bypass edilebilir mi?
- [ ] Hayır — filtreler; çeşitli kodlamaları, standart dışı etiketleri ve olay yöneticilerini (event handlers) güvenli bir şekilde ele alıyor
- [ ] Evet — HTML varlık kodlaması (HTML entity encoding) veya belirli etiketler (örneğin `<svg>`, `<img>`) kullanılarak bypass **mümkün**
- [ ] Evet — poliglot payload'lar veya mutasyon tabanlı XSS (mXSS) ile bypass **mümkün**

### İçerik Güvenliği Politikası (CSP), ikincil bir savunma katmanı sağlıyor mu?
- [ ] Evet — sıkı bir CSP **etkinleştirilmiştir** ve satır içi betiklerin (inline scripts) ve yetkisiz kaynakların çalıştırılmasını engeller
- [ ] Evet — bir CSP **etkinleştirilmiştir** ancak hatalı yapılandırılmıştır (örneğin `unsafe-inline` veya geniş kapsamlı `script-src`)
- [ ] Hayır — uygulama için herhangi bir CSP **etkinleştirilmemiştir**

### "Depolanmış XSS'ten RCE'ye" veya diğer yüksek etkili zincirleme saldırıları (Kör XSS / Blind XSS) gerçekleştirmek mümkün mü?
- [ ] Hayır — etki, istemci tarafındaki tarayıcı bağlamı ile sınırlıdır
- [ ] Evet — depolanmış XSS, dahili panellerdeki yöneticileri hedeflemek (Blind XSS) için **kullanılabilir**
- [ ] Evet — depolanmış XSS, diğer zafiyetlerle (örneğin CSRF, hassas veri sızdırma) **zincirlenebilir**

---

## WSTG-INPV-03 — HTTP Verb Tampering (HTTP Fiil Kurcalama) Testi

HTTP Verb Tampering (HTTP Fiil Kurcalama), web sunucularının ve uygulama çatılarının kullanılan HTTP yöntemine bağlı olarak belirli kaynaklara erişimi yetkilendirme şeklindeki zayıflıkları istismar eder. Saldırganlar, `GET` veya `POST` gibi standart yöntemleri; `HEAD`, `PUT`, `OPTIONS` gibi alternatiflerle veya arka ucun (backend) tutarsız bir şekilde işleyebileceği rastgele, standart dışı dizelerle değiştirerek güvenlik kısıtlamalarını atlatmaya çalışırlar. Bu güvenlik zafiyeti genellikle, Java EE web.xml filtreleri veya .NET yetkilendirme kuralları gibi güvenlik yapılandırmalarının izin verilen yöntemleri açıkça listelediği ancak diğerlerini hesaba katmadığı durumlarda veya "her şeyi reddet" (deny-all) yaklaşımı yerine "yönteme göre reddet" (deny-by-method) yaklaşımı kullanıldığında ortaya çıkar. Başarılı bir istismar; yönetim işlevlerine yetkisiz erişim, veri değişikliği veya sunucunun dahili yapılandırmasıyla ilgili bilgilerin ifşa edilmesiyle sonuçlanabilir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INPV-03 |
| **CWE** | CWE-288 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek* |

> *Yönetim işlevleri veya veri değişikliği (PUT/DELETE) kimlik doğrulaması olmadan gerçekleştirilebiliyorsa önem derecesi Yüksek olur.

**Kaynaklar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/03-Testing_for_HTTP_Verb_Tampering  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite (Repeater/Intruder)`, `curl`, `nmap`, `ffuf`

### Uygulama, standart dışı veya rastgele HTTP yöntemlerine yanıt veriyor mu?
- [ ] Hayır — uygulama, açıkça gerekli olanlar dışındaki tüm yöntemleri reddeder  
- [ ] Evet — uygulama standart yöntemlere (OPTIONS, TRACE) yanıt verir ancak güvenlik atlatılamaz  
- [ ] Evet — uygulama rastgele dizeleri (örneğin FOO, TEST) kabul eder ve bunları `GET` veya `POST` istekleri gibi işler  

### Kimlik doğrulama/yetkilendirme, HTTP yöntemi değiştirilerek atlatılabilir mi?
- [ ] Hayır — güvenlik denetimleri, HTTP fiilinden bağımsız olarak küresel olarak uygulanır  
- [ ] Evet — denetimler mevcuttur ancak `HEAD` yöntemi kullanılarak bypass (atlatma) mümkündür  
- [ ] Evet — güvenlik denetimleri alternatif yöntemlere uygulanmaz, bu da kısıtlanmış uç noktalara (endpoints) yetkisiz erişime izin verir  

### Sunucuda `PUT` veya `DELETE` gibi tehlikeli yöntemler etkin mi?
- [ ] Hayır — tehlikeli yöntemler devre dışıdır veya 405 Method Not Allowed döndürür  
- [ ] Evet — yöntemler etkindir ancak geçerli, yüksek ayrıcalıklı kimlik doğrulaması gerektirir  
- [ ] Evet — yöntemler etkindir ve yetkisiz dosya yüklemeye veya kaynak silmeye izin verir *(Kritik)*  

### `OPTIONS` yöntemi, izin verilen fiiller hakkında hassas bilgiler açığa çıkarıyor mu?
- [ ] Hayır — `OPTIONS` devre dışıdır veya genel bir yanıt döndürür  
- [ ] Evet — `OPTIONS`, `Allow` üst bilgisini döndürür ancak kısıtlanmış dahili yöntemleri ifşa etmez  
- [ ] Evet — `OPTIONS`, daha fazla istismara (exploit) yardımcı olan yalnızca dahili veya yönetici yöntemlerini ifşa eder  

### `TRACE` yöntemi etkin mi ve potansiyel olarak Cross-Site Tracing (XST) (Siteler Arası İzleme) işlemine izin veriyor mu?
- [ ] Hayır — `TRACE` ve `TRACK` yöntemleri devre dışıdır  
- [ ] Evet — `TRACE` etkindir ve hassas çerezler/belirteçler (tokens) dahil olmak üzere HTTP üst bilgilerini geri yansıtır

---

## WSTG-INPV-04 — HTTP Parametre Kirliliği (HTTP Parameter Pollution) Testi

HTTP Parametre Kirliliği (HTTP Parameter Pollution - HPP), bir uygulamanın aynı isme sahip birden fazla HTTP parametresi alması ve bunları tutarsız veya güvensiz bir şekilde işlemesi durumunda ortaya çıkar. Saldırgan, yinelenen parametreler sağlayarak uygulamanın dahili mantığını manipüle edebilir; bu durum Web Uygulaması Güvenlik Duvarlarının (Web Application Firewall - WAF), girdi doğrulama filtrelerinin veya erişim kontrol mekanizmalarının atlatılmasına (bypass) yol açabilir. Bu güvenlik açığı, yinelenen parametrelerden ilkini, sonuncusunu veya birleştirilmiş (concatenated) bir versiyonunu seçebilen temel web sunucusuna veya uygulama çatısına (framework) büyük ölçüde bağımlıdır. İstismar girişimi tipik olarak parametreleri arka uç API'lerine (API), veritabanı sorgularına veya URL yönlendirme mekanizmalarına ileten uç noktaları (endpoints) hedef alır; bu da Cross-Site Scripting (XSS) ile yetkisiz veri sızdırma (data exfiltration) arasında değişen etkilere neden olabilir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INPV-04 |
| **CWE** | CWE-235 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/04-Testing_for_HTTP_Parameter_Pollution  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite (Repeater/Intruder)`, `Arjun`, `HPP Finder`, `Wfuzz`

### Uygulama aynı isme sahip birden fazla HTTP parametresini nasıl işliyor?
- [ ] Hayır — yinelenen parametreler sunucu tarafından **reddediliyor** veya yok sayılıyor  
- [ ] Evet — sunucu tutarlı bir şekilde yalnızca **ilk** veya **son** parametreyi seçiyor  
- [ ] Evet — sunucu değerleri **birleştiriyor**, bu da potansiyel olarak bir enjeksiyona (injection) izin veriyor  

### HPP, WAF veya girdi filtresi gibi güvenlik kontrollerini atlatmak için kullanılabilir mi?
- [ ] Hayır — güvenlik kontrolleri bir parametrenin **tüm** örneklerini doğru şekilde denetliyor  
- [ ] Evet — bir WAF veya filtre yalnızca **ilk** örneği denetliyor, bu da kötü niyetli **ikinci** bir örneğin geçmesine izin veriyor  
- [ ] Evet — değerlerin birleştirilmesi, bir saldırganın tespitten kaçmak için bir Payload'u birden fazla parametreye **bölmesine** (split) olanak tanıyor  

### Uygulama, kirlenmiş parametreleri uygun bir arındırma işlemi yapmadan yanıta yansıtıyor mu?
- [ ] Hayır — parametreler kullanıcı arayüzünde (UI) yansıtılmadan önce arındırılıyor (sanitization) veya kodlanıyor (encoding)  
- [ ] Evet — kirlilik (pollution) yansıtılan içerikle sonuçlanıyor ancak arındırma işlemi **uygulanıyor**  
- [ ] Evet — kirlilik yansıtılan içerikle sonuçlanıyor ve arındırma işlemi **uygulanmıyor**; bu durum XSS veya mantıksal hatalara yol açıyor  

### Dahili API çağrılarına veya arka uç sistemlere iletilen parametreler kirliliğe karşı savunmasız mı?
- [ ] Hayır — arka uç istekleri güvenli, dinamik olmayan oluşturma yöntemleri kullanıyor  
- [ ] Evet — HPP, bir saldırganın arka uç istek yapısına parametre **enjekte etmesine** veya mevcut parametrelerin üzerine **yazmasına** (overwrite) olanak tanıyor  

### Uygulama, parametreler GET ve POST isteklerinde kirlendiğinde farklı bir davranış sergiliyor mu?
- [ ] Hayır — davranış tüm HTTP yöntemlerinde tutarlıdır  
- [ ] Evet — yalnızca GET parametreleri kirliliğe karşı savunmasızdır  
- [ ] Evet — yalnızca POST (body) parametreleri kirliliğe karşı savunmasızdır

---

## WSTG-INPV-05 — SQL Injection

SQL Injection, kullanıcı tarafından sağlanan girdilerin, uygun parametrelendirme (parameterization) veya temizleme (sanitization) işlemleri yapılmadan SQL sorgularına dahil edilmesiyle ortaya çıkar ve saldırganların sorgu mantığını manipüle etmesine olanak tanır. Başarılı bir istismar; yetkisiz veri sızdırma (data exfiltration), kimlik doğrulama atlatma (authentication bypass), veri değiştirme ve bazı durumlarda `xp_cmdshell` veya `LOAD_FILE()` gibi veritabanı özellikleri aracılığıyla uzaktan kod yürütmeye (Remote Code Execution - RCE) yol açabilir. Bu zafiyet genellikle giriş formlarını, arama işlevlerini, REST API parametrelerini, HTTP başlıklarını ve kullanıcı kontrollü girdilerden dinamik SQL sorguları oluşturan tüm uç noktaları (endpoint) etkiler. Bir saldırganın bakış açısından, bir enjeksiyon noktası tespit etmek, veritabanının tamamen ele geçirilmesine ve altyapı içerisinde olası yanal hareketlere (lateral movement) giden birincil kapıdır.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INPV-05 |
| **CWE** | CWE-89 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Kritik |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/05-Testing_for_SQL_Injection  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html  
* https://portswigger.net/web-security/sql-injection  
* https://portswigger.net/web-security/nosql-injection  

**Araçlar:** `sqlmap`, `Burp Suite (Intruder/Repeater)`, `Ghauri`, `jSQL Injection`, `BBQSQL`

### Kullanıcı tarafından kontrol edilebilen parametreler güvenli veritabanı erişim yöntemleri kullanılarak mı işleniyor?
- [ ] Evet — uygulama özel olarak ORM veya parametrelendirilmiş sorgular kullanıyor ve atlatma (bypass) **mümkün değil**  
- [ ] Evet — parametrelendirilmiş sorgular kullanılıyor ancak uç durumlar (örneğin `ORDER BY` yordamları) üzerinden atlatma **mümkün**  
- [ ] Hayır — SQL sorguları oluşturulurken parametrelendirme **yapılmadan** ham dizgi birleştirme (string concatenation) kullanılıyor  

### Kimlik doğrulama mekanizması SQL Injection aracılığıyla atlatılabiliyor mu?
- [ ] Hayır — giriş parametreleri enjeksiyona karşı **savunmasız değil**  
- [ ] Evet — totoloji tabanlı payload'lar (örneğin `' OR 1=1 --`) kullanılarak kimlik doğrulama atlatma **mümkün**  

### Bant içi (in-band) veya bant dışı (out-of-band) tekniklerle veri sızdırma mümkün mü?
- [ ] Hayır — herhangi bir veri sızdırma tespit edilmedi  
- [ ] Evet — UNION tabanlı veya hata tabanlı (error-based) veri sızdırma **mümkün**  
- [ ] Evet — kör (Boolean veya Zaman tabanlı - Time-based) veri sızdırma **mümkün**  
- [ ] Evet — bant dışı (DNS/HTTP) veri sızdırma **mümkün**  

### Uygulama işlevleri ikinci derece (second-order) SQL Injection zafiyetleri barındırıyor mu?
- [ ] Hayır — kaydedilen veriler daha sonraki sorgular için geri çağrıldığında güvenli bir şekilde işleniyor  
- [ ] Evet — kullanıcı girdisi kaydediliyor ve daha sonra doğrulama (validation) **yapılmadan** güvensiz bir SQL sorgusunda kullanılıyor  

### Veritabanı servis hesabı yetkileri gerekli olan minimum seviyeyle sınırlandırılmış mı?
- [ ] Evet — servis hesabı **en az yetki ilkesine** (least privilege) sahip (örneğin, belirli tablolar/eylemler ile sınırlı)  
- [ ] Hayır — servis hesabı yükseltilmiş yetkilere sahip (örneğin, `DROP TABLE`, `FILE` yetkileri veya `xp_cmdshell` **etkin**)

---

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

## WSTG-INPV-07 — XML Injection

XML Injection (XML Enjeksiyonu), bir uygulamanın kullanıcı tarafından sağlanan verileri; XML meta karakterlerini uygun şekilde doğrulamadan (validation), temizlemeden (sanitization) veya kaçış karakteri (escaping) kullanmadan bir XML belgesine veya akışına dahil etmesiyle oluşur. Saldırgan, belirli XML etiketlerini veya yapısal öğeleri enjekte ederek belgenin mantığını manipüle edebilir, kimlik doğrulama mekanizmalarını atlatabilir veya arka uç tarafından işlenen verilerin bütünlüğüne müdahale edebilir. Bu zafiyet genellikle SOAP tabanlı web servislerinde, XML kabul eden REST API'lerde, SAML (Security Assertion Markup Language) bildirimlerinde ve dinamik olarak XML yapılandırma dosyaları veya raporları oluşturan uygulamalarda bulunur. Bir saldırganın bakış açısından hedef, belge yapısını değiştirmek için hedeflenen veri bağlamından çıkmaktır; bu da potansiyel olarak yetkisiz ayrıcalık yükseltmeye (privilege escalation) veya istenmeyen iş mantığının yürütülmesine yol açar.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INPV-07 |
| **CWE** | CWE-91 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Orta |

**Kaynaklar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/07-Testing_for_XML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/xxe  

**Araçlar:** `Burp Suite (Intruder/Repeater)`, `OWASP ZAP`, `XMLSpy`, `CyberChef`

### Uygulama, kullanıcıdan gelen XML formatlı girdiyi kabul edip işliyor mu?
- [ ] Hayır — uygulama XML girdisini **işlememektedir**  
- [ ] Evet — XML girdisi API'ler (SOAP/REST) aracılığıyla işlenmektedir  
- [ ] Evet — XML, dosya yüklemeleri (SVG, DOCX vb.) aracılığıyla işlenmektedir  

### XML meta karakterleri (örn. `<`, `>`, `&`, `'`, `"`) uygun şekilde kaçış karakterine dönüştürülmüş mü veya temizlenmiş mi?
- [ ] Evet — tüm meta karakterler **uygun şekilde** kaçış karakterine dönüştürülmüş veya temizlenmiştir *(En güvenli)*  
- [ ] Evet — bazı filtrelemeler uygulanmaktadır ancak kodlama (encoding) kullanılarak atlatma (bypass) **mümkündür**  
- [ ] No — ham girdi, temizleme **yapılmadan** doğrudan XML yapılarına yerleştirilmektedir  

### Tüm XML girdileri için sunucu tarafında şema doğrulaması (XSD/DTD) zorunlu kılınmış mı?
- [ ] Evet — katı şema doğrulaması **etkindir** ve yapısal değişiklikleri engeller  
- [ ] Evet — doğrulama **etkindir** ancak veri alanları içindeki etiket enjeksiyonunu **engellemez**  
- [ ] Hayır — şema doğrulaması **devre dışıdır** veya uygulanmamıştır  

### Bir saldırgan, belge yapısını veya mantığını değiştirmek için yeni XML etiketleri enjekte edebilir mi?
- [ ] Hayır — girdiden bağımsız olarak yapı bozulmadan kalır  
- [ ] Evet — etiketler enjekte edilebilir ancak iş mantığını **değiştiremez**  
- [ ] Evet — etiket enjeksiyonu **mümkündür** ve mantık atlatmaya (bypass) veya veri değişikliğine izin verir *(Kritik)*  

### XML ayrıştırıcısı (parser), CDATA bölümleri veya XML yorumları kullanılarak manipüle edilebilir mi?
- [ ] Hayır — ayrıştırıcı, CDATA/yorum işaretlerini doğru şekilde işler veya reddeder  
- [ ] Evet — filtreleri gizlemek veya atlatmak için `<![CDATA[...]]>` veya `<!-- ... -->` enjeksiyonu **mümkündür**

---

## WSTG-INPV-08 — SSI Enjeksiyonu Testi (Testing for SSI Injection)

Sunucu Tarafı İçerme (SSI) Enjeksiyonu (Server-Side Includes (SSI) Injection), bir uygulama kullanıcı girdisini sunucunun SSI motoru tarafından işlenen HTML dosyalarına dahil etmeden önce arındırmadığında meydana gelir. Saldırganlar, rastgele kabuk (shell) komutları yürütmek, yerel dosyaları okumak veya ortam değişkenlerini dışarı sızdırmak için `<!--#exec cmd="id" -->` gibi SSI yönergelerini (directives) enjekte ederek bu durumu istismar ederler. Bu zafiyet genellikle `.shtml`, `.shtm` veya `.stm` gibi eski dosya uzantılarını kullanan uygulamalarda veya web sunucusunun standart HTML dosyaları içinde SSI çözümlemesi yapacak şekilde açıkça yapılandırıldığı durumlarda bulunur. Başarılı bir istismar (exploit), saldırgana web sunucusu süreciyle aynı yetkileri vererek genellikle tam sistem ele geçirme veya hassas veri maruziyeti (sensitive data exposure) ile sonuçlanır.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INPV-08 |
| **CWE** | CWE-97 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Kritiklik** | Yüksek |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/08-Testing_for_SSI_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite (Intruder/Repeater)`, `ffuf`, `Wfuzz`, `curl`

### Web sunucusu SSI yönergelerini destekliyor ve işliyor mu?
- [ ] Hayır — sunucu SSI desteklemiyor veya `.shtml` gibi dosya uzantıları devre dışı bırakılmış  
- [ ] Evet — SSI etkin ancak kullanıcı girdisi işlenen sayfalara yansıtılmıyor  
- [ ] Evet — SSI etkin ve kullanıcı girdisi işlenen sayfalara yansıtılıyor  

### `#exec` yönergesi aracılığıyla rastgele sistem komutları yürütülebiliyor mu?
- [ ] Hayır — `#exec` yönergesi devre dışı bırakılmış veya sunucu yapılandırmasıyla sınırlandırılmış  
- [ ] Evet — `#exec cmd` veya `#exec cgi` yükleri (payloads) aracılığıyla komut yürütme mümkündür  

### Hassas dosyaların veya ortam değişkenlerinin dışarı sızdırılması mümkün mü?
- [ ] Hayır — `#include` veya `#config` gibi yönergeler devre dışı bırakılmış  
- [ ] Evet — `#include virtual` aracılığıyla yerel dosyaları (örn. `/etc/passwd`) okumak mümkündür  
- [ ] Evet — sunucu ortam değişkenleri `#printenv` veya `#echo` aracılığıyla dışarı sızdırılabilir  

### Girdi doğrulama veya WAF kontrolleri SSI yüklerine karşı etkili mi?
- [ ] Evet — sıkı girdi doğrulama uygulanmaktadır ve bypass mümkün değildir  
- [ ] Evet — doğrulama/WAF uygulanmaktadır ancak karakter kodlaması veya farklı SSI sözdizimi kullanılarak bypass mümkündür  
- [ ] Hayır — SSI karakterleri (`<`, `!`, `#`, `-`, `"`) için herhangi bir doğrulama veya WAF koruması mevcut değildir

---

## WSTG-INPV-09 — XPath Enjeksiyonu Testi (Testing for XPath Injection)

XPath Enjeksiyonu (XPath Injection), bir uygulama XML verileri için bir XPath sorgusu oluştururken kullanıcı tarafından sağlanan bilgileri kullandığında ve bir saldırganın sorgu mantığına müdahale etmesine izin verdiğinde meydana gelir. Tek tırnak, köşeli parantez ve mantıksal operatörler gibi belirli sözdizimlerini enjekte ederek, bir saldırgan XML doküman yapısında gezinebilir, kimlik doğrulamayı atlatabilir veya hassas veri düğümlerini (data nodes) sızdırabilir (exfiltrate). Bu güvenlik açığı; yaygın olarak web servislerinde, yapılandırma yönetimi arayüzlerinde ve geleneksel SQL veritabanları yerine XML tabanlı veri depolarına dayanan eski (legacy) sistemlerde bulunur. Bir saldırganın perspektifinden bu durum, XML ağacını sistematik olarak haritalandırmak ve arka uçtan (backend) gizli değerleri elde etmek için genellikle mantıksal tabanlı (boolean-based) veya hata tabanlı (error-based) teknikler kullanılarak istismar edilir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INPV-09 |
| **CWE** | CWE-643 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Kritik |

**Kaynaklar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/09-Testing_for_XPath_Injection  
* https://hacktricks.wiki/en/pentesting-web/xpath-injection.html  

**Araçlar:** `Burp Suite (Intruder)`, `XCat`, `XPath-Injection-Tool`, `FFUF`

### Kullanıcı girdileri, XPath sorguları oluşturulurken uygun şekilde temizleniyor (sanitize) veya parametreleştiriliyor mu?
- [ ] Evet — girdiler XPath sorgularında **kullanılmıyor** veya kesin olarak parametreleştiriliyor  
- [ ] Evet — güçlü girdi doğrulama/kodlama (encoding) **uygulanıyor** ve atlatılması (bypass) **mümkün değil**  
- [ ] Evet — bazı doğrulamalar **uygulanıyor** ancak kodlanmış karakterler kullanılarak atlatma **mümkün**  
- [ ] Hayır — kullanıcı girdisi doğrudan XPath sorgularına ekleniyor (concatenated) *(Kritik)*  

### Uygulama, hata mesajları aracılığıyla XML/XPath yapısal detaylarını ifşa ediyor mu?
- [ ] Hayır — genel hata sayfaları gösteriliyor ve **hiçbir** XML detayı sızdırılmıyor  
- [ ] Evet — hata mesajları XML işlemenin varlığını ortaya çıkarıyor ancak **hiçbir** yol (path) detayı vermiyor  
- [ ] Evet — ayrıntılı hata mesajları XPath sözdizimini, düğüm adlarını veya dosya yollarını ifşa ediyor  

### XPath mantığı kullanılarak kimlik doğrulama veya yetkilendirme mantığını atlatmak mümkün mü?
- [ ] Hayır — kimlik doğrulama mantığı XML/XPath yapısına **dayanmıyor** veya güvenli bir şekilde uygulanmış  
- [ ] Evet — giriş veya izin kontrolleri `' or 1=1 or '` gibi mantık tabanlı yükler (payloads) kullanılarak atlatılabiliyor  

### XML doküman yapısı veya verileri kör enjeksiyon (blind injection) yoluyla elde edilebilir mi?
- [ ] Hayır — uygulama, mantıksal tabanlı (boolean-based) veya zaman tabanlı (time-based) çıkarımlara karşı **duyarlı değil**  
- [ ] Evet — düğüm adları ve veriler, mantıksal tabanlı çıkarım kullanılarak **sızdırılabiliyor**  
- [ ] Evet — tam XML ağacı gezintisi (traversal) ve veri çıkarımı **mümkün**

---

## WSTG-INPV-10 — IMAP SMTP Injection

IMAP ve SMTP enjeksiyonu (IMAP SMTP Injection) zafiyetleri, bir web uygulaması kullanıcı tarafından sağlanan verileri mail sunucusuna gönderilen komutlara dahil etmeden önce düzgün bir şekilde filtrelemediğinde ortaya çıkar. Sıfır başı ve satır besleme (Carriage Return and Line Feed - CRLF) karakterleri enjekte edilerek, saldırgan amaçlanan komutu sonlandırabilir ve ek alıcılar (CC/BCC) eklemek veya mesaj gövdesini değiştirmek gibi rastgele posta talimatları ekleyebilir. Bu kusurlar en sık web-to-mail ağ geçitlerinde, iletişim formlarında ve IMAP4 veya SMTP gibi protokoller aracılığıyla mail sunucularıyla doğrudan iletişim kuran e-posta istemcilerinde bulunur. Başılı bir istismar; spam'in yetkisiz bir şekilde iletilmesine (relay), hassas e-posta içeriklerinin sızdırılmasına ve bazı durumlarda posta sunucusunun bütünlüğünün tamamen tehlikeye atılmasına olanak tanır.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INPV-10 |
| **CWE** | CWE-93 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/10-Testing_for_IMAP_SMTP_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite (Repeater/Intruder)`, `netcat`, `telnet`, `nmap`

### Uygulama, IMAP veya SMTP komutları oluşturmak için kullanıcı tarafından sağlanan girdileri işliyor mu?
- [ ] Hayır — uygulama kullanıcı girdisi aracılığıyla mail sunucularıyla etkileşime girmez  
- [ ] Evet — uygulama mail sunucularıyla etkileşime girer ancak güvenli bir API veya kütüphane kullanır  
- [ ] Evet — uygulama kullanıcı kontrollü parametreleri kullanarak ham (raw) mail komutları oluşturur  

### Girdi, Carriage Return (`\r`) ve Line Feed (`\n`) dizilerinin enjeksiyonunu önlemek için doğrulanıyor mu?
- [ ] Evet — CRLF karakterleri **sıkı** bir şekilde filtrelenir, kodlanır veya reddedilir  
- [ ] Evet — girdi, karakterlerden oluşan sıkı bir izin verme listesine (allowlist) göre doğrulanır  
- [ ] Hayır — CRLF karakterleri filtrelenmez, bu da komut sonlandırma ve enjeksiyona izin verir  

### Bir saldırgan `Bcc:` veya `Cc:` gibi ek mail başlıkları (headers) enjekte edebilir mi?
- [ ] Hayır — sıkı girdi kısıtlamaları nedeniyle başlık (header) değişikliği **mümkün değildir**  
- [ ] Evet — CRLF dizileri aracılığıyla ek başlıkların enjeksiyonu **mümkündür**  
- [ ] Evet — harici alan adlarına mail iletimi (mail relaying) **etkindir** ve istismar edilebilir *(Kritik)*  

### Uygulama, yetkisiz posta kutularına erişmek için IMAP komutlarının manipüle edilmesine izin veriyor mu?
- [ ] Hayır — uygulama IMAP **kullanmaz** veya posta kutusu erişimini kimliği doğrulanmış kullanıcıyla sıkı bir şekilde sınırlar  
- [ ] Evet — IMAP komut enjeksiyonu **mümkündür** ancak meta veri listeleme (enumeration) ile sınırlıdır  
- [ ] Evet — IMAP komut enjeksiyonu, diğer kullanıcıların e-postalarının okunmasına veya silinmesine olanak tanır  

### Arka uç (backend) mail sunucusu komut ardışık düzenine (command pipelining) karşı savunmasız mı?
- [ ] Hayır — mail sunucusu toplu komutları veya tek bir oturumdaki birden fazla komutu reddeder  
- [ ] Evet — mail sunucusu tek bir girdi alanı üzerinden enjekte edilen birden fazla komutu işler

---

## WSTG-INPV-11 — Kod Enjeksiyonu (Code Injection)

Kod enjeksiyonu (Code Injection), bir uygulamanın kullanıcı tarafından sağlanan kod parçacıklarını, genellikle `eval()`, `exec()` veya `system()` gibi güvenli olmayan dile özgü fonksiyonlar aracılığıyla kendi çalışma zamanı ortamında (runtime environment) yürütmesiyle oluşur. Bu zafiyet, işletim sistemi kabuğunu değil, programlama dilinin yürütme bağlamını (örneğin PHP, Python, Node.js) hedeflediği için komut enjeksiyonundan (command injection) farklıdır. Saldırganlar, rastgele mantık yürütmek, ortam değişkenlerini (environment variables) sızdırmak veya sunucu üzerinde tam uzaktan kod yürütme (Remote Code Execution - RCE) elde etmek için bu noktaları istismar ederler. Sızma testi uzmanları (pentesters); matematiksel ifadeleri, sunucu tarafı şablonlarını (server-side templates) veya girdinin yürütülebilir kod olarak yorumlanabileceği dinamik yapılandırma parametrelerini işleyen özellikleri incelemelidir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INPV-11 |
| **CWE** | CWE-94 |
| **Test Durumu** | Yürütülmedi |
| **Önem Derecesi** | Kritik |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11-Testing_for_Code_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/deserialization  
* https://portswigger.net/web-security/llm-attacks  

**Araçlar:** `Burp Suite (Repeater/Intruder)`, `FFUF`, `PayloadsAllTheThings`, `Commix`

### Herhangi bir uygulama özelliği, dile özgü değerlendirme fonksiyonları aracılığıyla dinamik girdileri yürütüyor mu?
- [ ] Hayır — kod tabanında dinamik değerlendirme fonksiyonları **bulunmamaktadır**  
- [ ] Evet — `eval()`, `exec()` veya `include()` gibi fonksiyonlar kullanılmaktadır ancak kullanıcı girdisi **kabul etmemektedir**  
- [ ] Evet — fonksiyonlar kullanılmaktadır ve kullanıcı tarafından sağlanan girdileri **işlemektedir**  

### Değerlendirme öncesinde girdiye girdi doğrulama (validation) veya temizleme (sanitization) mekanizmaları uygulanıyor mu?
- [ ] Evet — girdi, **sabit kodlanmış bir beyaz listeye (whitelist)** karşı sıkı bir şekilde doğrulanmaktadır  
- [ ] Evet — girdi, kara liste (blacklisting) yoluyla temizlenmektedir ancak atlatma (bypass) **mümkündür**  
- [ ] Hayır — girdi doğrudan değerlendirme fonksiyonuna aktarılmaktadır ve **hiçbir kontrol** uygulanmamaktadır  

### Yürütme ortamı, sistem düzeyinde erişimi engellemek için izole edilmiş (sandboxed) veya kısıtlanmış mı?
- [ ] Evet — yürütme, sistem çağrılarının **yapılamadığı** oldukça kısıtlı bir korumalı alanda (sandbox) gerçekleşmektedir  
- [ ] Evet — bazı kısıtlamalar mevcuttur ancak korumalı alandan kaçış (sandbox escape) **mümkündür**  
- [ ] Hayır — ortam, temel dil uygulama programlama arayüzlerine (API) ve işletim sistemi fonksiyonlarına tam erişim sağlamaktadır *(Kritik)*  

### Kod enjeksiyonu (code injection) hassas bilgileri sızdırmak için kullanılabilir mi?
- [ ] Hayır — yürütme kördür (blind) ve bant dışı (out-of-band) iletişim **mümkün değildir**  
- [ ] Evet — ortam değişkenleri veya yerel dosyalar HTTP/DNS istekleri aracılığıyla **sızdırılabilir**  
- [ ] Evet — yürütülen kodun sonuçları doğrudan uygulama yanıtına **yansıtılmaktadır**  

### Uygulama, uzaktan dosya dahil etmeye (Remote File Inclusion - RFI) veya dinamik kütüphane yüklemeye izin veriyor mu?
- [ ] Hayır — yalnızca yerel ve önceden tanımlanmış kod yolları yürütülebilir  
- [ ] Evet — uygulama, uzak veya saldırgan kontrolündeki bir kaynaktan kod yüklemeye ve yürütmeye **zorlanabilir**

---

## WSTG-INPV-12 — Command Injection

Command Injection (Komut Enjeksiyonu), bir uygulamanın form girişleri, HTTP başlıkları veya çerezler gibi güvenli olmayan kullanıcı verilerini bir sistem kabuğuna (shell) iletmesi durumunda ortaya çıkar. Bu zafiyet, bir saldırganın sunucu üzerinde, genellikle web uygulaması sürecinin yetkileriyle rastgele işletim sistemi komutları yürütmesine olanak tanır. Saldırgan açısından bu durum; noktalı virgül, pipe veya backtick gibi shell meta karakterleri kullanarak zararlı komutları hedeflenen yürütme akışına zincirleme yoluyla istismar edilir (Exploit). Etkisi genellikle yıkıcıdır; tam sistem ele geçirme, yetkisiz veri sızdırma veya iç ağda yanal hareket (Lateral Movement) ile sonuçlanabilir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INPV-12 |
| **CWE** | CWE-78 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Kritik |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/12-Testing_for_Command_Injection  
* https://hacktricks.wiki/en/pentesting-web/command-injection.html  
* https://portswigger.net/web-security/os-command-injection  

**Araçlar:** `Commix`, `Burp Suite (Intruder/Repeater)`, `dnsbin`, `Interactsh`, `Collaborator`

### Uygulama, sistem düzeyinde komutlar veya shell yürütülebilir dosyaları çağırıyor mu?
- [ ] Hayır — uygulama işletim sistemi kabuğu (shell) ile etkileşime **girmiyor**  
- [ ] Evet — uygulama, sistem görevleri için dahili API'ler veya üst düzey kütüphaneler kullanıyor  
- [ ] Evet — uygulama; `system()`, `exec()` veya `popen()` gibi sistem çağrıları (system calls) aracılığıyla shell komutlarını doğrudan çağırıyor  

### Kullanıcı girdisi, sistem çağrılarına (system calls) iletilmeden önce doğrulanıyor mu veya temizleniyor mu?
- [ ] Evet — sıkı izin listesi (allow-listing) ve girdi doğrulaması (Input Validation) **uygulanıyor**  
- [ ] Evet — temizleme (sanitization) veya kaçış karakteri (escaping) kullanımı mevcut ancak bypass **mümkün**  
- [ ] Hayır — kullanıcı girdisi doğrulama **olmadan** doğrudan shell'e iletiliyor  

### Komut yürütme akışını manipüle etmek için shell meta karakterleri kullanılabilir mi?
- [ ] Hayır — meta karakterler doğru bir şekilde filtreleniyor veya kaçış karakteri ile etkisiz hale getiriliyor  
- [ ] Evet — komut çıktısının yanıta yansıdığı bant içi (in-band) yürütme **mümkündür**  
- [ ] Evet — hiçbir çıktının yansımadığı kör (blind) yürütme **mümkündür**  

### Ana makineden bant dışı (OOB) veri sızdırma (exfiltration) mümkün mü?
- [ ] Hayır — sunucunun dışa giden (egress) bağlantısı yok veya OOB sinyalleri (DNS/HTTP) başarısız oluyor  
- [ ] Evet — DNS veya HTTP tabanlı OOB sinyalleri aracılığıyla veri sızdırma **mümkündür**  

### Uygulama süreci yüksek sistem yetkileriyle mi çalışıyor?
- [ ] Hayır — uygulama özel, düşük yetkili bir servis hesabı ile çalışıyor  
- [ ] Evet — uygulama `root`, `Administrator` veya yüksek yetkili bir kullanıcı olarak çalışıyor *(Kritik)*

---

## WSTG-INPV-13 — Format String Injection

Format string injection (format dizgisi enjeksiyonu), bir web uygulamasının kullanıcı tarafından kontrol edilen girdiyi doğrudan C'nin `printf` ailesi veya düşük seviyeli günlükleme (logging) sarmalayıcıları gibi değişken argümanlı (variadic) bir fonksiyonun format dizgisi argümanına iletmesiyle oluşur. Modern yönetilen kod (managed-code) web framework'lerinde daha az yaygın olsa da, bu zafiyet eski nesil CGI uygulamalarında, yerel (native) eklentilerde veya düşük seviyeli sistem kütüphaneleriyle etkileşime giren uç durum günlükleme sistemlerinde kritikliğini korumaktadır. Saldırganlar, yığın (stack) üzerindeki hassas bellek adreslerini ve verileri sızdırmak için `%x` veya `%p` gibi format belirleyicilerini (format specifiers) ya da rastgele bellek yazma işlemleri gerçekleştirmek için `%n` belirleyicisini sağlayarak bu durumdan faydalanırlar. Başarılı bir istismar (exploit), genellikle Uzaktan Kod Yürütme (Remote Code Execution - RCE) yoluyla sistemin tamamen ele geçirilmesiyle veya en azından süreç çökmeleri aracılığıyla etkili bir hizmet dışı bırakma (Denial-of-Service - DoS) durumuyla sonuçlanır.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INPV-13 |
| **CWE** | CWE-134 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Kritik |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/13-Testing_for_Format_String_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite`, `GDB`, `strings`, `radare2`, `Checksec`, `AFL++`

### Uygulama, kullanıcı girdisini yerel kod (native code) veya eski nesil CGI arayüzleri üzerinden mi işliyor?
- [ ] Hayır — uygulama tamamen yönetilen kod framework'leri (örneğin JVM, CLR) üzerine inşa edilmiştir  
- [ ] Evet — uygulama, format string zafiyetlerinin **bulunabileceği** JNI, yerel C/C++ eklentileri veya eski nesil ikili (binary) CGI'lar kullanmaktadır  

### Kullanıcı tarafından sağlanan girdiler, format farkındalığı olan fonksiyonlara birincil argüman olarak mı iletiliyor?
- [ ] Hayır — girdiler veri argümanı olarak iletilir ve format dizgileri **statik** ve **sabit kodludur (hardcoded)**  
- [ ] Evet — kullanıcı girdisi doğrudan format dizgisi argümanına eklenir ancak temizleme (sanitization) **uygulanmaktadır**  
- [ ] Evet — kullanıcı girdisi doğrudan format dizgisi olarak kullanılır ve hiçbir temizleme **uygulanmamaktadır**  

### Format belirleyicileri kullanılarak bellek içeriğini sızdırmak mümkün mü?
- [ ] Hayır — girdi doğrulanır ve `%p`, `%x` veya `%s` gibi belirleyiciler **işlenmez**  
- [ ] Evet — `%p` veya `%x` belirleyicilerinin sağlanması, yığın (stack) veya öbek (heap) bellek adreslerinin yansımasıyla sonuçlanır  
- [ ] Evet — hassas veriler (örneğin işaretçiler, stack cookies) format belirleyicileri aracılığıyla **sızdırılabilir**  

### Uygulama `%n` belirleyicisi kullanılarak çökertilebilir mi veya manipüle edilebilir mi?
- [ ] Hayır — bellek yazma işlemlerine izin veren belirleyiciler **devre dışı bırakılmıştır** veya derleyici/ortam tarafından engellenmiştir  
- [ ] Evet — `%s%s%s%s` veya benzeri dizgiler sağlandığında uygulama çöker (DoS)  
- [ ] Evet — `%n` üzerinden rastgele bellek yazma işlemleri **mümkündür** ve potansiyel olarak Uzaktan Kod Yürütme (RCE) ile sonuçlanabilir  

### Modern ikili korumalar (ASLR, DEP, Stack Canaries), bu enjeksiyon yoluyla atlatılabilir mi?
- [ ] Hayır — ortam sıkılaştırılmıştır (hardened) ve bellek sızıntısı korumaları atlatmak için **kullanılamaz**  
- [ ] Evet — format string injection, temel adresleri sızdırmak için **kullanılabilir** ve ASLR/DEP atlatmayı **mümkün** kılar

---

## WSTG-INPV-14 — İnkübe Edilmiş (Incubated) Zafiyetlerin Test Edilmesi

İnkübe edilmiş zafiyetler (Incubated vulnerabilities), kalıcı (persistent) veya ikinci derece (second-order) zafiyetler olarak da adlandırılır; kötü niyetli bir Payload'un uygulama tarafından "soğuk" (cold) bir durumda saklanması ve daha sonra farklı bir bağlamda çalıştırılması veya işlenmesi durumunda ortaya çıkar. Bu saldırı deseni tipik olarak, verileri yeterli yeniden doğrulama veya çıktı kodlaması (output encoding) yapmadan paylaşılan bir veritabanından veya dosya sisteminden alan arka uç (back-end) sistemlerini, yönetici konsollarını veya otomatik raporlama araçlarını hedef alır. Pentester'lar, kullanıcı girişinin yaşam döngüsünü giriş noktasından ("inkübatör") bu verinin nihayetinde diğer bileşenler veya ikincil uygulamalar tarafından işlendiği veya kullanıldığı her konuma kadar izlemelidir. Başarılı bir sömürü (exploitation), genellikle saklı XSS (Stored XSS) yoluyla yönetici oturumu ele geçirme veya dahili raporlama modüllerinde ikinci derece SQL Injection aracılığıyla veri sızdırma (data exfiltration) gibi yüksek etkili sonuçlara yol açar.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INPV-14 |
| **CWE** | CWE-20 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/14-Testing_for_Incubated_Vulnerability  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html#second-order-injection  
* https://portswigger.net/web-security/web-cache-deception  
* https://portswigger.net/web-security/web-cache-poisoning  

**Araçlar:** `Burp Suite (Collaborator)`, `sqlmap`, `OWASP ZAP`, `KXSs`, `Dalfox`

### Kullanıcı kontrollü girişlerin, diğer kullanıcılar veya süreçler tarafından daha sonra alınmak üzere saklandığı uç noktalar (endpoints) var mı?
- [ ] Hayır — uygulama kullanıcı girişini daha sonra kullanmak üzere **saklamıyor**  
- [ ] Evet — giriş; veritabanı alanlarında, günlük (log) dosyalarında veya yapılandırma ayarlarında **saklanıyor**  

### Veriler kalıcı depolama katmanına yazılmadan önce girdi doğrulaması (input validation) veya temizleme (sanitization) uygulanıyor mu?
- [ ] Evet — giriş noktasında katı izin listesi (allow-list) doğrulaması **uygulanıyor**  
- [ ] Evet — genel temizleme **uygulanıyor** ancak kodlama veya standart dışı karakterler aracılığıyla atlatma (bypass) **mümkün**  
- [ ] Hayır — giriş ham, doğrulanmamış formunda saklanıyor  

### Saklanan veriler yönetici veya ikincil arayüzlerde işlendiğinde (render) uygun şekilde kodlanıyor veya temizleniyor mu?
- [ ] Evet — verinin işlendiği her yerde bağlam duyarlı çıktı kodlaması (context-aware output encoding) **uygulanıyor**  
- [ ] Evet — bazı konumlarda kodlama **uygulanıyor** ancak diğerlerinde (örneğin dahili yönetici paneli) **eksik**  
- [ ] Hayır — veri herhangi bir kodlama veya temizleme yapılmadan işleniyor ve payload yürütülmesine izin veriyor  

### Veritabanında saklanan payload'lar arka uç toplu iş (batch) süreçlerini veya bant dışı (out-of-band) izleme sistemlerini etkileyebilir mi?
- [ ] Hayır — arka uç süreçleri güvenli ayrıştırma (parsing) yöntemleri veya parametreli sorgular (parameterized queries) kullanıyor  
- [ ] Evet — saklanan payload'lar arka uç mantığında etkileşimleri **tetikleyebilir** (örneğin CSV Injection, günlüklerde Command Injection)  
- [ ] Evet — saklanan payload'lar saldırgan kontrollü sunuculara bant dışı (OOB) istekleri **tetikleyebilir**

---

## WSTG-INPV-15 — HTTP Splitting Smuggling

HTTP Splitting ve Smuggling zafiyetleri, ön uç (frontend) vekilleri ile arka uç (backend) sunucularının, özellikle `Content-Length` ve `Transfer-Encoding` başlıkları açısından HTTP istek sınırlarını nasıl yorumladığı ve işlediği arasındaki tutarsızlıklardan kaynaklanır. Saldırgan, belirsiz istekler kurgulayarak arka uca gizli bir istek "sızdırabilir" (smuggle) veya bir yanıtı bölmek için CRLF dizileri enjekte edebilir; bu da önbellek zehirlenmesi (cache poisoning), istek ele geçirme (request hijacking) veya güvenlik kontrollerinin atlatılmasına yol açar. Bu kusurlar genellikle tutarsız ayrıştırma (parsing) mantığına sahip ters vekiller (reverse proxy), yük dengeleyiciler (load balancer) veya CDN'lerin kullanıldığı karmaşık ortamlarda ortaya çıkar. Bir saldırgan açısından bu durum, kullanıcı trafiğinin yönlendirilmesine, hassas oturum jetonlarının (session tokens) çalınmasına ve diğer kullanıcıların oturumları bağlamında yetkisiz eylemlerin gerçekleştirilmesine olanak tanır.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INPV-15 |
| **CWE** | CWE-444 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Kritik |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/15-Testing_for_HTTP_Response_Splitting  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  

**Araçlar:** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `smuggler.py`, `curl`

### Ortam, CL.TE veya TE.CL tutarsızlıkları yoluyla istek sızdırmaya (request smuggling) karşı duyarlı mı?
- [ ] Hayır — ön uç ve arka uç sunucuları istek sınırlarını tutarlı bir şekilde işlemektedir  
- [ ] Evet — tutarsızlıklar mevcuttur ancak altyapıdaki iyileştirmeler nedeniyle istismar **mümkün değildir**  
- [ ] Evet — CL.TE veya TE.CL sızdırma (smuggling) **mümkündür**, gizli isteklerin arka uca ulaşmasına izin verir *(Yüksek)*  
- [ ] Evet — ön uç filtrelerini atlatmak için TE.TE (çift kodlama/gizleme) **mümkündür** *(Kritik)*  

### Başlıklardaki CRLF enjeksiyonu (CRLF injection) yoluyla HTTP Yanıt Bölme (HTTP Response Splitting) gerçekleştirilebilir mi?
- [ ] Hayır — uygulama, tüm başlık girişlerindeki CRLF dizilerini uygun şekilde temizlemektedir  
- [ ] Evet — CRLF dizileri başlıklara yansıtılmaktadır ancak yanıt bölme (response splitting) **mümkün değildir**  
- [ ] Evet — CRLF enjeksiyonu **mümkündür**, başlık enjeksiyonuna veya önbellek zehirlenmesine (cache poisoning) izin verir  

### Güvenlik kontrolleri (WAF/ACL'ler), sızdırılan istekler kullanılarak atlatılabiliyor mu?
- [ ] Hayır — güvenlik kontrolleri hem dış isteklere hem de sızdırılan isteklere uygulanmaktadır  
- [ ] Evet — sızdırılan istekler ön uç WAF kurallarını veya IP tabanlı ACL'leri **atlatabilir**  

### Diğer kullanıcıların oturumlarını ele geçirmek veya trafiklerini yönlendirmek mümkün mü?
- [ ] Hayır — istek/yanıt akışları izoledir ve birbirine karıştırılamaz  
- [ ] Evet — istek sızdırma (request smuggling) **mümkündür** ve diğer kullanıcıların isteklerinin ele geçirilmesine izin verir *(Kritik)*  
- [ ] Evet — yanıt bölme (response splitting) **mümkündür** ve tarayıcı tarafında önbellek zehirlenmesine veya XSS'e izin verir

---

## WSTG-INPV-16 — HTTP Request Smuggling

HTTP Request Smuggling (HRS), genellikle çelişen `Content-Length` ve `Transfer-Encoding` başlıkları nedeniyle bir HTTP sunucu zincirinin (yük dengeleyici ve arka uç web sunucusu gibi) bir isteğin sınırlarını farklı şekilde yorumlamasıyla oluşur. Bir saldırgan, bu desenkronizasyonu kullanarak arka uç sunucusunun istek arabelleğine bir giriş "sızdırır" (smuggle) ve bir sonraki meşru kullanıcının isteğinin başına saldırgan kontrollü bir segment ekleyerek bu isteği fiilen ön ekler. Bu teknik; oturum ele geçirme (session hijacking), güvenlik kontrollerini atlatma ve önbellek zehirlemesi (cache poisoning) gibi ciddi saldırılara olanak tanır. İstismar genellikle zaman tabanlı analizle veya sunucuya sonraki istekler gönderildiğinde arka uçtan gelen beklenmedik yanıtların gözlemlenmesiyle gerçekleştirilir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INPV-16 |
| **CWE** | CWE-444 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Kritik / Yüksek |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/16-Testing_for_HTTP_Request_Smuggling  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  
* https://portswigger.net/web-security/request-smuggling  

**Araçlar:** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `curl`, `SmuggleHunter`

### Ön uç ve arka uç sunucuları `Transfer-Encoding: chunked` başlığını tutarlı bir şekilde işliyor mu?
- [ ] Evet — her iki sunucu da chunked kodlamayı aynı şekilde işler ve desenkronizasyon **mümkün değildir**  
- [ ] Hayır — sunucular başlıkları farklı yorumlar ancak sanitizasyon/normalizasyon **uygulanmaktadır**  
- [ ] Hayır — CL.TE (Content-Length/Transfer-Encoding) desenkronizasyonu **mümkündür** *(Kritik)*  
- [ ] Hayır — TE.CL (Transfer-Encoding/Content-Length) desenkronizasyonu **mümkündür** *(Kritik)*  

### Saldırgan, sızdırılmış (smuggled) bir istek kullanarak sunucu veya istemci tarafı önbelleğini zehirleyebilir mi?
- [ ] Hayır — önbellekleme **devre dışıdır** veya zehirlenmeye karşı duyarlı değildir  
- [ ] Evet — sızdırılmış istekler, önbellek zehirlemesi (cache poisoning) yoluyla kullanıcıları kötü amaçlı alan adlarına yönlendirebilir veya betik enjekte edebilir  

### İstek birleştirme (request concatenation) yoluyla diğer kullanıcıların isteklerini veya oturum belirteçlerini (session tokens) ele geçirmek mümkün müdür?
- [ ] Hayır — sızdırma (smuggling), kullanıcılar arası veri ifşasına **izin vermez**  
- [ ] Evet — sızdırma, bir sonraki kullanıcının isteğinin saldırgan kontrollü bir `POST` gövdesine eklenmesine izin vererek veri sızdırmayı (exfiltration) **mümkün kılar**  

### Altyapı HTTP/2 kullanıyor mu ve H2.CL veya H2.TE desenkronizasyonuna karşı duyarlı mı?
- [ ] Hayır — uygulama yalnızca HTTP/1.1 kullanıyor veya HTTP/2, sürüm düşürme (downgrade) olmaksızın güvenli bir şekilde uygulanmış  
- [ ] Evet — HTTP/2'den HTTP/1.1'e düz metin sürüm düşürmeleri (cleartext downgrades) gerçekleşiyor ve desenkronizasyon **mümkündür**  

### Hatalı biçimlendirilmiş başlıklar (örneğin, boşluk içeren `Transfer-Encoding:  chunked`) güvenli bir şekilde işleniyor mu?
- [ ] Evet — hatalı biçimlendirilmiş başlıklar tüm katmanlarda reddedilir veya normalize edilir  
- [ ] Hayır — hatalı biçimlendirilmiş veya gizlenmiş başlıkların tutarsız işlenmesi nedeniyle TE.TE desenkronizasyonu **mümkündür**

---

## WSTG-INPV-17 — Host Header Enjeksiyonu Testi (Testing for Host Header Injection)

Host Header Enjeksiyonu (Host Header Injection), bir uygulamanın kullanıcı tarafından sağlanan `Host` başlığına güvenmesi ve bu başlığı yeterli doğrulama yapmadan bağlantı oluşturma veya yönlendirme gibi sunucu tarafı mantığına dahil etmesi durumunda meydana gelir. Saldırganlar; web önbellek zehirlenmesini (web cache poisoning) kolaylaştırmak, hassas belirteçleri (sensitive tokens) harici bir alana yönlendirerek parola sıfırlama zehirlenmesine (password reset poisoning) olanak tanımak veya başlık tabanlı yönlendirmeye dayanan güvenlik kontrollerini atlatmak amacıyla bu başlığı manipüle eder. Başarılı bir istismar, saldırganın oturum verilerini sızdırmasına, hesap ele geçirme (account takeover) işlemlerini gerçekleştirmesine veya şüphelenmeyen kullanıcıları kötü niyetli altyapılara yönlendirmesine izin verir. Bu güvenlik açığı, en yaygın olarak yük dengelemeli ortamlarda veya gelen istek bağlamına göre dinamik olarak mutlak URL'ler (absolute URLs) oluşturan uygulamalarda bulunur.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INPV-17 |
| **CWE** | CWE-20 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek* |

> *Host başlığı, parola sıfırlama gibi e-postalarda hassas bağlantılar oluşturmak için kullanılıyorsa Önem Derecesi Yüksek (High) olur.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/17-Testing_for_Host_Header_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/host-header  

**Araçlar:** `Burp Suite (Repeater/Collaborator)`, `curl`, `Param Miner`, `nmap (http-vhosts)`

### Uygulama `Host` başlığı değerini bağlantılarda, yönlendirmelerde veya betiklerde yansıtıyor mu?
- [ ] Hayır — uygulama sabit kodlanmış (hardcoded) temel URL'ler veya sıkı bir şekilde doğrulanmış yapılandırma kullanıyor  
- [ ] Evet — `Host` başlığı yansıtılıyor ancak bir beyaz listeye (whitelist) göre sıkı bir şekilde doğrulanıyor  
- [ ] Evet — `Host` başlığı yansıtılıyor ve hatalı yapılandırılmış değerler (örneğin, `beklenen.com:saldirgan.com`) aracılığıyla atlatma (bypass) **mümkün**  
- [ ] Evet — `Host` başlığı herhangi bir doğrulama **olmadan** yansıtılıyor  

### Uygulama veya altyapı tarafından alternatif ana makine (host) ile ilgili başlıklar işleniyor mu?
- [ ] Hayır — `X-Forwarded-Host`, `X-Host` veya `Forwarded` gibi başlıklar yoksayılıyor  
- [ ] Evet — alternatif başlıklar işleniyor ancak dahili mantığı (internal logic) **geçersiz kılmıyor**  
- [ ] Evet — alternatif başlıklar, birincil `Host` başlığı değerini geçersiz kılmak için **kullanılabiliyor**  

### `Host` başlığı, sistem tarafından oluşturulan e-postaları (örneğin parola sıfırlama) zehirlemek için manipüle edilebilir mi?
- [ ] Hayır — e-posta bağlantıları, sunucu yapılandırmasındaki statik ve güvenilir bir alanı kullanıyor  
- [ ] Evet — `Host` başlığı e-posta bağlantılarını etkiliyor ancak doğrulama **atılatılamıyor**  
- [ ] Evet — `Host` başlığı, parola sıfırlama belirteçlerini saldırgan kontrolündeki bir alana yönlendirmek için manipüle **edilebiliyor** *(Kritik)*  

### Uygulama, `Host` başlığı aracılığıyla Web Önbellek Zehirlenmesine (Web Cache Poisoning) karşı savunmasız mı?
- [ ] Hayır — herhangi bir önbellekleme mekanizması mevcut değil veya `Host` başlığı önbellek anahtarının (cache key) bir parçası  
- [ ] Evet — bir önbellek mevcut ancak anahtarlanmamış girdiler (unkeyed inputs) için `Host` başlığını sıkı bir şekilde doğruluyor veya yoksayıyor  
- [ ] Evet — bir önbellek mevcut ve diğer kullanıcılara kötü niyetli içerik sunmak için enjekte edilmiş bir `Host` başlığı ile **zehirlenebiliyor**  

### Sunucu, sanal ana makine yönlendirmesi (virtual host routing) için rastgele `Host` başlıklarını kabul ediyor mu?
- [ ] Hayır — sunucu, tanınmayan `Host` başlıkları için bir hata veya varsayılan bir sayfa döndürüyor  
- [ ] Evet — sunucu her türlü `Host` başlığını kabul ediyor; bu durum keşif (reconnaissance) veya dahili servis numaralandırma (internal service enumeration) için **kullanışlıdır**

---

## WSTG-INPV-18 — Sunucu Taraflı Şablon Enjeksiyonu (Server-Side Template Injection) Testi

Sunucu Taraflı Şablon Enjeksiyonu (Server-Side Template Injection - SSTI), bir uygulamanın kullanıcı kontrollü girdiyi yeterli temizleme (sanitization) veya kaçış (escaping) işlemi yapmadan sunucu taraflı bir şablon motoruna (template engine) yerleştirmesiyle oluşur. Bu durum, bir saldırganın sunucuda yürütülen kötü niyetli şablon direktifleri enjekte etmesine olanak tanır ve Uzaktan Kod Çalıştırma (Remote Code Execution - RCE) yoluyla sistemin tamamen ele geçirilmesine yol açabilir. SSTI genellikle Jinja2, Twig veya FreeMarker gibi motorları kullanan dinamik e-posta oluşturma, profil sayfaları ve bildirim sistemleri gibi özelliklerde görülür. Bir saldırgan açısından bu zafiyet, şablon motorunun yerel nesne ve metotlarından yararlanarak hassas ortam değişkenlerini (environment variables) sızdırmak, dahili dosyaları okumak veya sistem komutlarını çalıştırmak için sıklıkla istismar edilir.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INPV-18 |
| **CWE** | CWE-1336 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Kritik |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/18-Testing_for_Server-side_Template_Injection  
* https://hacktricks.wiki/en/pentesting-web/ssti-server-side-template-injection/index.html  
* https://portswigger.net/web-security/server-side-template-injection  

**Araçlar:** `tplmap`, `Burp Suite (Intruder/Repeater)`, `ffuf`, `SSTI-Payload-Generator`

### Uygulama, kullanıcı girdisini işlemek için sunucu taraflı bir şablon motoru kullanıyor mu?
- [ ] Hayır — uygulama dinamik içerik için sunucu taraflı şablonlar **kullanmıyor**  
- [ ] Evet — şablonlar kullanılıyor ancak kullanıcı girdisi şablon direktiflerine **yerleştirilmiyor**  
- [ ] Evet — kullanıcı girdisi, uygun temizleme işlemi **yapılmadan** şablonlara yerleştiriliyor  

### Girdi alanlarına enjekte edildiğinde temel aritmetik ifadeler değerlendiriliyor mu?
- [ ] Hayır — `{{7*7}}` veya `${7*7}` gibi payload'lar düz metin dizeleri (literal strings) olarak yansıtılıyor  
- [ ] Evet — ifadeler değerlendiriliyor (örneğin `49` sonucu dönüyor), bu da şablon motorunun **zafiyet barındırdığını** doğruluyor  

### Şablon motorunun yerleşik nesnelerine veya metotlarına erişilebiliyor mu?
- [ ] Hayır — tehlikeli nesnelere, metotlara veya yapılandırmalara erişim **kısıtlanmış** veya sandbox (kum havuzu) içine alınmış  
- [ ] Evet — hassas verileri sızdırmak için yerleşik nesnelere (örneğin `self`, `config`, `__class__`) **erişilebiliyor**  
- [ ] Evet — sistem çağrıları veya işletim sistemi kütüphaneleri aracılığıyla Uzaktan Kod Çalıştırma (RCE) **mümkündür**  

### Tehlikeli direktiflerin yürütülmesini engelleyen bir sandbox veya izin verilenler listesi (allowlist) mekanizması var mı?
- [ ] Evet — sıkı bir izin verilenler listesi veya güvenli sandbox mevcut ve atlatılması (bypass) **mümkün değil**  
- [ ] Evet — sandbox mevcut ancak motora özgü belirli payload'lar ile atlatılması (bypass) **mümkündür**  
- [ ] No — şablon motoruna herhangi bir sandbox veya izin verilenler listesi **uygulanmıyor**

---

## WSTG-INPV-19 — Sunucu Taraflı İstek Sahteciliği (Server-Side Request Forgery - SSRF) Testi

Sunucu Taraflı İstek Sahteciliği (Server-Side Request Forgery - SSRF), bir saldırganın web uygulamasını, sunucudan dahili veya harici kaynaklara yetkisiz istekler yapması için etkileyebildiği durumlarda ortaya çıkar. URL, ana makine adları (hostnames) veya IP adresleri bekleyen parametreleri manipüle ederek, saldırganlar güvenlik duvarları (firewalls) ve ACL'ler gibi ağ düzeyi kontrolleri atlayarak dahili ağ segmentlerini tarayabilir, bulut meta veri servislerine (IMDS) erişebilir veya savunmasız dahili API'ler ve veritabanları ile etkileşime girebilir. Bu zafiyet genellikle görsel getirme, PDF oluşturma, webhook'lar veya URL üzerinden dosya yükleme içeren özelliklerde kendini gösterir. Saldırgan perspektifinden SSRF; izole ortamlara sızmak (pivot), hassas yapılandırma verilerini dışarı sızdırmak (exfiltrate) veya Redis ya da Memcached gibi dahili servislerle etkileşime girerek potansiyel olarak uzaktan kod çalıştırma (Remote Code Execution - RCE) elde etmek için kullanılan güçlü bir temel öğedir (primitive).


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INPV-19 |
| **CWE** | CWE-918 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Kritik |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/19-Testing_for_Server-Side_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/ssrf-server-side-request-forgery/index.html  
* https://portswigger.net/web-security/ssrf  

**Araçlar:** `Burp Suite (Collaborator/Interactsh)`, `Interact.sh`, `Gopherus`, `ffuf`, `cURL`, `DNSRebind Tool`

### Uygulama kullanıcı tarafından sağlanan URL'leri veya ana makine adlarını işliyor mu?
- [ ] Hayır — harici URL veya ana makine adlarını kabul eden bir özellik bulunmamaktadır  
- [ ] Evet — özellik mevcuttur ancak girdi bir izin listesine (allow-list) göre sıkı bir şekilde doğrulanmaktadır  
- [ ] Evet — özellik mevcuttur ve kullanıcı tarafından sağlanan URL'leri kabul etmektedir  

### İstenen hedef için doğrulama kontrolleri veya kara listeler uygulanıyor mu?
- [ ] Evet — güvenilir alan adlarından/IP'lerden oluşan sıkı bir izin listesi (allow-list) zorunlu tutulmaktadır  
- [ ] Evet — bir engelleme listesi (deny-list) kullanılmaktadır (örneğin; `127.0.0.1`, `169.254.169.254` adreslerini engellemek gibi)  
- [ ] Hayır — girdiye herhangi bir doğrulama veya filtreleme uygulanmamaktadır  

### Kodlama, yönlendirmeler veya DNS yeniden bağlama yoluyla filtreleri atlamak mümkün müdür?
- [ ] Hayır — filtreler güçlüdür ve yönlendirmeleri/DNS çözümlemesini güvenli bir şekilde işlemektedir  
- [ ] Evet — URL kodlaması (encoding) veya alternatif IP formatları (hex, octal) kullanılarak atlatma mümkündür  
- [ ] Evet — dahili hedeflere yönelik 3xx HTTP yönlendirmeleri üzerinden atlatma mümkündür  
- [ ] Evet — dahili IP'leri çözümlemek için DNS yeniden bağlama (DNS rebinding) saldırılarıyla atlatma mümkündür  

### Uygulama dahili meta veri servislerine veya yerel arayüzlere ulaşabiliyor mu?
- [ ] Hayır — yerel ağ ve bulut meta veri uç noktalarına (endpoints) ulaşılamamaktadır  
- [ ] Evet — localhost (`127.0.0.1`) veya geri döngü (loopback) servislerine erişim mümkündür  
- [ ] Evet — bulut meta veri servislerine (örneğin; AWS/GCP/Azure IMDS) erişim mümkündür *(Kritik)*  

### SSRF yanıtının niteliği nedir?
- [ ] Blind (Kör) — kullanıcıya herhangi bir yanıt veya meta veri döndürülmez  
- [ ] Partial (Kısmi) — kullanıcıya yanıt meta verileri (başlıklar, boyut, zamanlama) döndürülür  
- [ ] Full (Tam) — dahili kaynaktan gelen tam yanıt gövdesi (response body) kullanıcıya yansıtılır

---

## WSTG-INPV-20 — Mass Assignment

Mass Assignment (Toplu Atama), Overposting (Aşırı Gönderim) veya parametrelerin Güvensiz Deserialization (Insecure Deserialization) işlemi olarak da bilinir; bir web uygulamasının, gelen HTTP istek parametrelerini uygun öznitelik filtrelemesi yapmadan dahili nesne özelliklerine otomatik olarak bağlaması durumunda ortaya çıkar. Bu güvenlik zafiyeti, JSON, XML veya form-kodlu verilerin doğrudan uygulama tarafındaki veri modellerine deserialize edildiği modern MVC framework'lerinde ve RESTful API'lerde yaygındır. Saldırganlar, geliştiricilerin kullanıcı kontrolüne açmayı amaçlamadığı hassas alanları değiştirmek için bir isteğe —`isAdmin`, `role` veya `account_balance` gibi— ek ve listede olmayan parametreler enjekte ederek bu davranışı suistimal ederler. Başarılı bir sömürü, genellikle yetki yükseltme (privilege escalation), yetkisiz veri değişikliği veya kritik iş mantığı (business logic) iş akışlarının atlatılması ile sonuçlanır.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INPV-20 |
| **CWE** | CWE-915 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Orta* |

> *Yönetimsel öznitelikler, izin seviyeleri veya faturalandırma alanları değiştirilebiliyorsa önem derecesi Yüksek olarak kabul edilir.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/20-Testing_for_Mass_Assignment  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Arjun`, `Param Miner`, `Burp Suite (Repeater)`, `Postman`, `ffuf`

### Uygulama, gelen istek parametreleri için otomatik bağlama (automatic binding) kullanıyor mu?
- [ ] Hayır — parametreler manuel olarak belirli değişkenlere veya güvenli nesnelere eşlenmektedir  
- [ ] Evet — otomatik bağlama kullanılmaktadır ancak katı **beyaz listeler (white-lists)** veya DTO'lar (Data Transfer Objects) zorunlu tutulmaktadır  
- [ ] Evet — otomatik bağlama kullanılmaktadır ve hassas dahili öznitelikler **değiştirilebilmektedir**  

### Yönetimsel veya izinle ilgili öznitelikler başarıyla enjekte edilebiliyor mu?
- [ ] Hayır — enjekte edilen parametreler yok sayılır, temizlenir veya bir doğrulama (validation) hatasına neden olur  
- [ ] Evet — ek parametreler kabul edilir ancak arka uç (backend) nesne durumunu **etkilemez**  
- [ ] Evet — `is_admin`, `role` veya `permissions` gibi parametrelerin enjekte edilmesi yetkileri **başarıyla** yükseltir *(Kritik)*  

### Overposting yoluyla hassas iş mantığı veya finansal alanları değiştirmek mümkün mü?
- [ ] Hayır — `price`, `balance` veya `status` gibi alanlar toplu atamaya (mass assignment) karşı korunmaktadır  
- [ ] Evet — hassas iş öznitelikleri, JSON veya form istek gövdesine (request body) eklenerek **değiştirilebilmektedir**  

### Uygulama, parametre tahminini kolaylaştıran dahili nesne yapılarını ifşa ediyor mu?
- [ ] Hayır — yanıtlar yalnızca amaçlanan genel (public) alanları içerir ve dokümantasyon kısıtlıdır  
- [ ] Evet — dahili nesne alanları GET yanıtlarında görünür durumdadır ve bu durum parametre keşfini **mümkün** kılmaktadır  
- [ ] Evet — API dokümantasyonu (Swagger/OpenAPI) atanabilen dahili özellikleri açıkça listelemektedir

---

## WSTG-ERRH-01 — Uygunsuz Hata Yönetimi (Improper Error Handling) Testi

Uygunsuz hata yönetimi, bir web uygulamasının hata yanıtları aracılığıyla yığın izleri (stack traces), veritabanı şeması bilgileri veya dahili dosya yolları gibi hassas teknik detayları ifşa etmesi durumunda ortaya çıkar. Saldırganlar, uygulamanın temel mimarisini haritalamak ve daha fazla istismar (exploit) için potansiyel vektörleri belirlemek amacıyla hatalı biçimlendirilmiş girdi (malformed input) göndererek, var olmayan kaynaklara erişerek veya sunucu tarafı istisnaları (server-side exceptions) tetikleyerek bu hataları uyarırlar. Bu bilgi sızıntısı, saldırgana kesin ortam spesifikasyonları sağlayarak genellikle SQL Enjeksiyonu (SQL Injection) veya Dizin Gezginliği (Path Traversal) dahil olmak üzere daha ciddi saldırıların öncüsü olur. Güvenli bir uygulama, ayrıntılı tanılama verilerini yalnızca güvenli ve sunucu tarafı günlüklerde (logs) tutarken, kullanıcıya genel ve kullanıcı dostu mesajlar sunmalıdır.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-ERRH-01 |
| **CWE** | CWE-209 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Düşük / Orta |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/01-Testing_For_Improper_Error_Handling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite (Repeater/Intruder)`, `ffuf`, `wfuzz`, `Arjun`, `curl`

### Uygulama, işlenmemiş istisnalar için genel ve global bir hata işleyici kullanıyor mu?
- [ ] Evet — genel hata sayfaları **etkindir** ve hiçbir teknik detay **ifşa edilmemektedir**  
- [ ] Evet — özel hata sayfaları kullanılmaktadır ancak belirli başlıklar (headers) aracılığıyla sızıntı **mümkündür**  
- [ ] Hayır — varsayılan sunucu hata sayfaları (ör. Tomcat, IIS, Nginx) **görünür durumdadır**  

### Hatalı biçimlendirilmiş girdiler veya sınır durumlar aracılığıyla teknik bilgiler numaralandırılabiliyor mu?
- [ ] Hayır — hatalar, günlükleme için benzersiz referans kimlikleri (IDs) ile düzgün bir şekilde ele alınmaktadır  
- [ ] Evet — yanıtlarda yığın izleri (stack traces) veya veritabanı sorguları **ifşa edilmektedir**  
- [ ] Evet — dahili dosya yolları, ortam değişkenleri (environment variables) veya sunucu sürüm bilgileri **ifşa edilmektedir**  

### API yanıtları ayrıntılı hata nesneleri veya hata ayıklama bilgileri sızdırıyor mu?
- [ ] Hayır — API'lar standartlaştırılmış hata kodları ve temizlenmiş (sanitized) JSON mesajları döndürmektedir  
- [ ] Evet — API'lar, yanıt gövdesinde ayrıntılı hata ayıklama (debug) nesneleri veya tam istisna detayları döndürmektedir  
- [ ] Evet — JSON yanıtının `details` veya `exception` alanlarında yığın izleri yer almaktadır  

### Hatalar oluştuğunda uygulama farklı (Zaman tabanlı veya İçerik tabanlı) davranıyor mu?
- [ ] Hayır — hata türünden bağımsız olarak yanıt imzaları ve zamanlamaları tutarlıdır  
- [ ] Evet — farklılaşan yanıtlar, geçerli ve geçersiz durumları numaralandırmak (ör. Kullanıcı Numaralandırma - User Enumeration) için **kullanılabilir**

---

## WSTG-ERRH-02 — Yığın İzleme (Stack Trace) Testleri

Bir uygulama bir istisnayı (exception) düzgün bir şekilde yönetemediğinde yığın izleme (stack trace) çıktıları oluşturulur ve bu durum dahili yürütme durumunun ve çağrı hiyerarşisinin son kullanıcıya ifşa edilmesiyle sonuçlanır. Bir sızma testi uzmanı (penetration tester) için bu izler; uygulama çatısını (framework), kütüphane versiyonlarını, dahili dosya sistemi yollarını ve mantık akışını ortaya çıkardığı için keşif (reconnaissance) aşamasında gereken çabayı önemli ölçüde azalttığından paha biçilemezdir. Sömürme (exploitation) süreci; uygulamayı kararsız bir duruma zorlamak ve temel teknoloji yığını (technology stack) hakkında bilgi sızdırmak amacıyla bozuk girdiler (malformed input), beklenmedik veri türleri veya sınır koşulu (boundary condition) ihlalleri yoluyla kasıtlı olarak hataların tetiklenmesini içerir. Bu sızıntı, genellikle savunmasız üçüncü taraf bileşenleri tanımlamak veya ifşa edilen kod yollarındaki mantıksal kusurlar (logic flaws) için özel exploitler hazırlamak için gerekli bağlamı sağlar.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-ERRH-02 |
| **CWE** | CWE-209 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Düşük / Orta* |

> *Yığın izleme (stack trace) hassas mimari detayları, veritabanı sorgularını (database queries) veya dahili yapılandırma parametrelerini ifşa ediyorsa önem derecesi "Orta" seviyeye yükselir.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/02-Testing_for_Stack_Traces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite`, `ffuf`, `Arjun`, `Wfuzz`, `Wappalyzer`

### Uygulama hataları durumunda tam yığın izleme (stack trace) çıktıları istemciye döndürülüyor mu?
- [ ] Hayır — uygulama genel hata sayfaları veya özel hata mesajları döndürüyor  
- [ ] Evet — yığın izleme çıktıları **etkinleştirilmiş** ancak yalnızca hassas olmayan belirli modüller için  
- [ ] Evet — tam yığın izleme çıktıları **etkinleştirilmiş** ve HTTP yanıt gövdesinde (HTTP response body) görünür durumda  

### Hata mesajları hassas çevresel bilgileri ifşa ediyor mu?
- [ ] Hayır — hata mesajları teknik veya çevresel detay içermiyor  
- [ ] Evet — mesajlar **dahili dosya yollarını (internal file paths)** veya sunucu tarafı **dizin yapılarını** ifşa ediyor  
- [ ] Evet — mesajlar **veritabanı şemalarını (database schemas)**, **SQL sorgularını (SQL queries)** veya **üçüncü taraf kütüphane versiyonlarını** ifşa ediyor  

### Girdi parametreleri manipüle edilerek yığın izleme (stack trace) tetiklenebiliyor mu?
- [ ] Hayır — bozuk girdi (malformed input) düzgün bir şekilde yönetiliyor veya doğrulama (validation) tarafından reddediliyor  
- [ ] Evet — izler, **beklenmedik veri türleri** (örneğin, string beklenen bir yere array gönderilmesi) ile tetiklenebiliyor  
- [ ] Evet — izler, **null baytlar (null bytes)**, **özel karakterler** veya **sınır değerler (boundary values)** ile tetiklenebiliyor  

### Uygulama genelinde tutarlı bir küresel hata işleyici (global error handler) uygulanmış mı?
- [ ] Evet — tüm test edilen uç noktalarda (endpoints) bir küresel istisna işleyici **tutarlı bir şekilde uygulanmış**  
- [ ] Evet — bir işleyici mevcut ancak eski (legacy), API veya yeni eklenen uç noktalara **uygulanmamış**  
- [ ] Hayır — herhangi bir küresel hata yönetimi mekanizması **tespit edilmedi**, bu da varsayılan sunucu hatalarına neden oluyor

---

## WSTG-CRYP-01 — Zayıf Taşıma Katmanı Güvenliğinin Test Edilmesi (Testing for Weak Transport Layer Security)

Zayıf taşıma katmanı güvenliğinin (transport layer security) test edilmesi, verilerin iletimi sırasında gizliliğini ve bütünlüğünü sağlamak amacıyla SSL/TLS uygulamalarının ve konfigürasyonlarının analiz edilmesini kapsar. Saldırganlar; Ortadaki Adam (Man-in-the-Middle - MitM) saldırılarını gerçekleştirmek için güncelliğini yitirmiş protokoller (SSLv2, TLS 1.0), zayıf şifreleme paketleri (cipher suites) (NULL, RC4 veya anonim) ve geçersiz veya zayıf imzalanmış sertifikalar gibi yanlış yapılandırmaları hedeflerler. Bu test genellikle uygulamanın web sunucusunu veya şifreli iletişimin kurulduğu yük dengeleyici (load balancer) uç noktalarını hedefler. Başarılı bir istismar (exploit), bir saldırganın hassas verileri ele geçirmesine, kullanıcı oturumlarını çalmasına (hijack) veya protokol düşürme (protocol downgrade) ya da trafik deşifreleme (traffic decryption) yoluyla bağlantıları güvensiz durumlara indirmesine olanak tanır.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CRYP-01 |
| **CWE** | CWE-326 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Orta* |

> *Hassas veriler (PII, kimlik bilgileri, oturum belirteçleri) iletiliyorsa Önem Derecesi Yüksek; genel bilgilendirme amaçlı trafik için Ortadır.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/01-Testing_for_Weak_Transport_Layer_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `testssl.sh`, `sslyze`, `nmap`, `OpenSSL`, `Qualys SSL Labs`

### Güncelliğini yitirmiş veya güvensiz TLS/SSL protokolleri destekleniyor mu?
- [ ] Hayır — yalnızca TLS 1.2 ve TLS 1.3 **etkinleştirilmiştir**  
- [ ] Evet — TLS 1.0 veya TLS 1.1 **etkinleştirilmiştir**  
- [ ] Evet — SSLv2 veya SSLv3 **etkinleştirilmiştir** *(Kritik)*  

### Sunucu tarafından zayıf veya güvensiz şifreleme paketleri (cipher suites) kabul ediliyor mu?
- [ ] Hayır — yalnızca güçlü ve modern şifrelemeler (örn. AES-GCM, ChaCha20) **desteklenmektedir**  
- [ ] Evet — zayıf şifrelemeye sahip (örn. 3DES, RC4) veya düşük bit uzunluklu şifrelemeler **desteklenmektedir**  
- [ ] Evet — NULL, anonim (ADH) veya dışa aktarım (export) şifrelemeleri **desteklenmektedir** *(Kritik)*  

### Sunucu sertifikası geçerli ve güvenilir mi?
- [ ] Evet — sertifika geçerlidir, alan adı (domain) ile eşleşmektedir ve **güvenilir bir CA** tarafından imzalanmıştır  
- [ ] Hayır — sertifikanın **süresi dolmuştur** veya **zayıf bir imza algoritması** (örn. SHA-1) kullanmaktadır  
- [ ] Hayır — sertifika **kendi kendine imzalanmıştır (self-signed)** veya alan adı **uyumsuzluğu (mismatch)** mevcuttur  

### Sunucu, Güvenli Yeniden Anlaşmayı (Secure Renegotiation) destekliyor mu ve Sürüm Düşürme (Downgrade) saldırılarını engelliyor mu?
- [ ] Evet — Secure Renegotiation **desteklenmektedir** ve TLS Fallback SCSV **etkinleştirilmiştir**  
- [ ] Hayır — Secure Renegotiation **desteklenmemektedir**, bu durum potansiyel olarak MitM enjeksiyonuna izin verebilir  
- [ ] Hayır — Sunucu **POODLE** veya **ROBOT** saldırılarına karşı savunmasızdır  

### HTTP Sıkı Taşıma Güvenliği (HSTS) düzgün bir şekilde uygulanmış mı?

| Üstbilgi (Header) | Mevcut | Eksik |
|--------|:-------:|:-------:|
| `Strict-Transport-Security` |  |  |

- [ ] Evet — HSTS üstbilgisi mevcuttur, uzun bir `max-age` değerine sahiptir ve `includeSubDomains` **etkinleştirilmiştir**  
- [ ] Evet — HSTS üstbilgisi mevcuttur ancak `includeSubDomains` eksiktir veya **kısa bir max-age** değerine sahiptir  
- [ ] Hayır — HSTS üstbilgisi **mevcut değildir**

---

## WSTG-CRYP-02 — Padding Oracle Testi

Padding Oracle zafiyetleri, bir uygulamanın deşifreleme (decryption) sürecinin, deşifre edilen bir şifreli metnin (ciphertext) dolgulamasının (padding) geçerli olup olmadığına dair bilgi sızdırdığı durumlarda ortaya çıkar. Farklı HTTP durum kodları, belirli hata mesajları veya yanıt süresi farklılıkları gibi bu yan kanal (side-channel) yanıtlarını gözlemleyerek; bir saldırgan, şifreleme anahtarını bilmeden verileri deşifre edebilir ve potansiyel olarak rastgele açık metinleri (plaintext) yeniden şifreleyebilir. Bu zafiyet genellikle PKCS#7 dolgulamalı Cipher Block Chaining (CBC) modunda blok şifreleyiciler (block ciphers) kullanan şifrelenmiş oturum çerezlerinde, kimlik doğrulama belirteçlerinde veya URL parametrelerinde görülür. İstismar edilmesi, gizliliğin tamamen kaybolmasına neden olur ve sahte şifrelenmiş blokların oluşturulması yoluyla bütünlüğün (integrity) bozulmasına yol açabilir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CRYP-02 |
| **CWE** | CWE-209 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Kritik* |

> *Zafiyet; oturum yönetimi, kimlik belirteçleri veya PII (Kişisel Veri) şifrelemesinde bulunuyorsa Önem Derecesi Kritiktir.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/02-Testing_for_Padding_Oracle  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `PadBuster`, `Burp Suite (Intruder/Padding Oracle Solver)`, `pkcs7-padbuster`, `CyberChef`

### Hassas veriler için CBC modunda bir blok şifreleyici kullanılıyor mu?
- [ ] Hayır — uygulama kimliği doğrulanmış şifreleme (AES-GCM/ChaCha20-Poly1305) kullanıyor  
- [ ] Evet — uygulama blok şifreleyiciler kullanıyor ancak dolgulama (padding) **gerekmiyor** (örn. Akış şifreleyiciler veya CTR modu)  
- [ ] Evet — uygulama CBC modu kullanıyor ve dolgulama (padding) **uygulanıyor**  

### Sunucu, dolgulama geçerliliğini yan kanallar aracılığıyla ifşa ediyor mu?
- [ ] Hayır — sunucu tek tip hata mesajları ve tutarlı yanıt süreleri döndürüyor  
- [ ] Evet — sunucu, dolgulama hataları için farklı HTTP durum kodları (örn. 200'e karşı 500) döndürüyor  
- [ ] Evet — sunucu belirli hata mesajları döndürüyor (örn. "Invalid Padding", "Decryption failed")  
- [ ] Evet — sunucu deşifreleme hatası sırasında ölçülebilir zamanlama farkları sergiliyor  

### Şifreli metnin otomatik olarak deşifre edilmesi mümkün mü?
- [ ] Hayır — yan kanallar (side channels) istismar için yeterince **kararlı değil**  
- [ ] Evet — şifreli metin (ciphertext) **kısmen** deşifre edilebiliyor (son birkaç blok)  
- [ ] Evet — `PadBuster` gibi araçlar kullanılarak **tam açık metin (plaintext) kurtarma mümkündür**  

### Saldırgan bit-flipping gerçekleştirebilir mi veya geçerli şifreli metinler üretebilir mi?
- [ ] Hayır — bütünlük kontrolleri (HMAC), deşifreleme işleminden **önce** doğrulanıyor  
- [ ] Evet — herhangi bir bütünlük kontrolü mevcut değil, ancak Payload oluşturma **uygulanabilir değil**  
- [ ] Evet — rastgele şifreli metin oluşturma **mümkündür**; bu da yetki yükseltmeye (privilege escalation) veya veri enjeksiyonuna olanak tanır

---

## WSTG-CRYP-03 — Şifrelenmemiş Kanallar Üzerinden Gönderilen Hassas Bilgilerin Test Edilmesi

Hassas verilerin düz metin HTTP gibi şifrelenmemiş kanallar üzerinden iletilmesi, bilgilerin Aradaki Adam (Man-in-the-Middle - MITM) saldırıları yoluyla ele geçirilmesine ve değiştirilmesine yol açar. Bu zafiyet genellikle web uygulamalarının tüm uç noktalarda (endpoints) Taşıma Katmanı Güvenliği'ni (Transport Layer Security - TLS) zorunlu kılmadığı durumlarda; özellikle eski (legacy) bileşenlerde, giriş formlarında veya HTTP'den HTTPS'e ilk geçiş aşamasında meydana gelir. Bir saldırgan açısından bu durum, halka açık Wi-Fi gibi ele geçirilmiş veya güvenilmeyen ağ bölümlerinde kimlik doğrulama bilgilerinin, oturum belirteçlerinin (session tokens) ve kişisel verilerin (Personally Identifiable Information - PII) pasif olarak dinlenmesine (sniffing) olanak tanır. Tüm hassas verilerin sağlam bir TLS tüneli içine kapsüllenmesini sağlamak, kullanıcı etkileşimlerinin gizliliğini ve bütünlüğünü korumak ve oturum çalmayı (session hijacking) önlemek için temel bir gerekliliktir.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CRYP-03 |
| **CWE** | CWE-319 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Orta |

> *Kimlik doğrulama bilgileri, oturum tanımlayıcıları veya PII düz metin (cleartext) olarak iletiliyorsa önem derecesi Yüksek (High) olarak kabul edilir.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/03-Testing_for_Sensitive_Information_Sent_via_Unencrypted_Channels  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Wireshark`, `tcpdump`, `Burp Suite (Proxy/Alerts)`, `nmap`, `sslyze`, `testssl.sh`

### Uygulama, hassas uç noktalar için şifrelenmemiş HTTP üzerinden iletişime izin veriyor mu?
- [ ] Hayır — tüm uygulama trafiği kesinlikle HTTPS üzerinden zorlanmaktadır *(En güvenli)*  
- [ ] Evet — hassas olmayan sayfalar HTTP üzerinden erişilebilir ancak hassas uç noktalara **erişilemez**  
- [ ] Evet — hassas uç noktalar (oturum açma, ödeme, profil) şifrelenmemiş HTTP üzerinden **erişilebilirdir** *(Yüksek Risk)*  

### HTTP'den HTTPS'e global bir yönlendirme mevcut mu?
- [ ] Evet — uygulama tüm HTTP istekleri için HTTPS'e 301/302 yönlendirmesi yapar  
- [ ] Evet — yönlendirme yalnızca belirli hassas uç noktalar için gerçekleştirilir  
- [ ] Hayır — HTTP'den HTTPS'e herhangi bir yönlendirme **uygulanmamıştır**  

### HTTP Strict Transport Security (HSTS) uygulanmış mı?
- [ ] Evet — HSTS, uzun bir `max-age` değeri ile **etkindir** ve `includeSubDomains` ile `preload` direktiflerini içerir  
- [ ] Evet — HSTS **etkindir** ancak `includeSubDomains` veya `preload` direktifleri eksiktir  
- [ ] Hayır — uygulama sunucusunda HSTS **etkinleştirilmemiştir**  

### Hassas çerezler (cookies) düz metin iletimine karşı korunuyor mu?

| Çerez Adı | Secure Bayrağı Mevcut | Secure Bayrağı Eksik |
|-------------|:-------------------:|:------------------:|
| `SessionID` |                     |                    |
| `AuthToken` |                     |                    |
| `CSRF-Token`|                     |                    |

### Uygulama, üçüncü taraf API'lara şifrelenmemiş kanallar üzerinden hassas veri iletiyor mu?
- [ ] Hayır — tüm harici API çağrıları şifreli (HTTPS) tüneller üzerinden gerçekleştirilir  
- [ ] Evet — bazı hassas olmayan telemetri verileri HTTP üzerinden gönderilir  
- [ ] Evet — API anahtarları, PII veya kimlik bilgileri üçüncü taraflara şifrelenmemiş HTTP üzerinden **gönderilmektedir**

---

## WSTG-CRYP-04 — Zayıf Kriptografik Primitiflerin Test Edilmesi

Zayıf kriptografik primitifler (Weak Cryptographic Primitives), çakışma saldırılarına (collision attacks), frekans analizine veya kaba kuvvet (Brute Force) şifre çözümüne karşı duyarlı olan MD5, SHA-1, DES veya RC4 gibi modası geçmiş veya güvenli olmayan algoritmaların kullanılmasını içerir. Bu zayıflıklar genellikle parola özetleme (password hashing), durağan veri şifreleme (data encryption at rest), oturum belirteci oluşturma (session token generation) veya uygulama arka ucundaki dijital imza doğrulama işlemlerinde ortaya çıkar. Bir saldırgan açısından bu primitiflerin belirlenmesi, açık metin parolaları elde etmek için yakalanan özetlerin (hashes) kırılması veya kimlik doğrulama belirteçlerinin (authentication tokens) sahtesinin oluşturulması gibi veri bütünlüğünün ve gizliliğinin tehlikeye atılmasına olanak tanır. Modern uygulamalar bunun yerine, kripto analize karşı sağlam koruma sağlamak amacıyla Argon2, Bcrypt veya AES-GCM gibi endüstri standardı olan, çakışmaya dayanıklı ve yüksek entropili primitifler kullanmalıdır.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CRYP-04 |
| **CWE** | CWE-327 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/04-Testing_for_Weak_Cryptographic_Primitives  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `hashcat`, `John the Ripper`, `Burp Suite`, `CyberChef`, `OpenSSL`, `nmap (ssl-enum-ciphers)`

### Hassas veri depolama için MD5 veya SHA-1 gibi desteği sonlandırılmış (deprecated) özetleme algoritmaları kullanılıyor mu?
- [ ] Hayır — uygulama Argon2 veya Bcrypt gibi modern, çakışmaya dayanıklı algoritmalar kullanıyor  
- [ ] Evet — desteği sonlandırılmış algoritmalar kullanılıyor ancak **yalnızca** güvenlik açısından hassas olmayan bütünlük kontrolleri için  
- [ ] Evet — parolalar veya hassas belirteçler (tokens) için desteği sonlandırılmış algoritmalar **kullanılıyor** *(Yüksek)*  

### DES, 3DES veya RC4 gibi zayıf simetrik şifreleme algoritmaları (ciphers) şu anda kullanılıyor mu?
- [ ] Hayır — GCM veya CBC gibi güvenli bir modda AES (128-bit veya daha yüksek) kullanılıyor  
- [ ] Evet — eski şifreleme algoritmaları geriye dönük uyumluluk için **etkinleştirilmiş** durumda ancak varsayılan değil  
- [ ] Evet — eski şifreleme algoritmaları, durağan veya iletim halindeki verileri korumak için birincil yöntemdir  

### Uygulama "kendi yapımı" (roll-your-own) veya standart dışı bir kriptografik mantık uyguluyor mu?
- [ ] Hayır — uygulama kesinlikle standart, hakemli incelemeden geçmiş (peer-reviewed) kriptografik kütüphanelere bağlı kalıyor  
- [ ] Evet — özel sarmalayıcılar (wrappers) mevcut ancak temel primitifler endüstri standardıdır  
- [ ] Evet — hassas verilere özel mülkiyetli (proprietary) kriptografik algoritmalar veya özel mantık **uygulanıyor** *(Kritik)*  

### Kriptografik tuzlar (Salts) ve başlatma vektörleri (IVs) en iyi uygulamalara göre mi yönetiliyor?
- [ ] Evet — tuzlar kullanıcı başına benzersizdir ve IV'ler her işlem için tahmin edilemezdir  
- [ ] Evet — tuzlar kullanılıyor ancak tüm kayıtlarda benzersiz **değil**  
- [ ] Hayır — tuzlar statik, sabit kodlanmış (hardcoded) veya mevcut değil ve IV'ler birden fazla oturumda **yeniden kullanılıyor**  

### Asimetrik anahtarların (RSA, Diffie-Hellman) bit uzunluğu modern standartlar için yeterli mi?
- [ ] Evet — RSA/DH anahtarları 2048 bit veya daha yüksektir ya da Eliptik Eğri (ECC) kullanılıyor  
- [ ] Hayır — anahtarlar 2048 bitten azdır ancak atlatılması (bypass) kolay değildir  
- [ ] Hayır — anahtarlar 1024 bit veya daha küçüktür, bu da onları çarpanlara ayırma veya hesaplamalı saldırılara karşı **savunmasız** hale getirir

---

## WSTG-BUSL-01 — İş Mantığı Veri Doğrulama Testi

İş mantığı veri doğrulama testi (Business Logic Data Validation), uygulamanın sözdizimsel (syntactic) olarak doğru ancak iş alanı bağlamında mantıksal olarak geçersiz olan verileri tanımlama ve reddetme yeteneğine odaklanır. Saldırganlar, kısıtlamaları aşmak ve uygulama durumunu manipüle etmek için bir alışveriş sepetindeki negatif miktarlar, gelecekteki etkinlikler için geçmiş tarihler veya aşırı büyük tam sayılar gibi beklenmedik değerler göndererek bu açıkları istismar ederler. Bu zafiyetler genellikle sunucu tarafı mantığının (server-side logic), kullanıcı tarafından sağlanan verilerin bağlamsal bütünlüğünü doğrulamada başarısız olduğu çok aşamalı iş akışlarında, ödeme süreçlerinde ve hesap yönetimi modüllerinde bulunur. Başarılı bir istismar (Exploit); finansal kayba, veri bütünlüğünün bozulmasına ve kısıtlanmış özelliklere veya verilere yetkisiz erişime yol açabilir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-BUSL-01 |
| **CWE** | CWE-20 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Orta |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/01-Test_Business_Logic_Data_Validation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**Araçlar:** `Burp Suite (Repeater/Intruder)`, `Postman`, `Python (Requests)`, `OWASP ZAP`

### Uygulama, sayısal girdiler üzerinde mantıksal aralık sınırlarını zorunlu kılıyor mu?
- [ ] Evet — uygulama, uygunsuz durumlarda negatif, sıfır veya aşırı büyük değerleri doğru bir şekilde reddeder *(En güvenli)*  
- [ ] Evet — uygulama bazı geçersiz aralıkları reddeder ancak tam sayı taşması (integer overflow) veya sınır atlatmaya karşı **savunmasızdır**  
- [ ] Hayır — uygulama, iş bağlamına bakılmaksızın her türlü sayısal değeri kabul eder *(Kritik)*  

### Sunucu tarafı doğrulama kontrolleri uygulamanın tüm katmanlarında tutarlı mı?
- [ ] Evet — doğrulama, hem API/servis hem de veritabanı katmanlarında tutarlı bir şekilde uygulanır  
- [ ] Evet — doğrulama UI/Frontend tarafında **uygulanır** ancak doğrudan API isteği yoluyla atlatma (bypass) **mümkündür**  
- [ ] Hayır — doğrulama **uygulanmaz** veya tutarsızdır, bu da mantıksal veri bozulmasına izin verir  

### İş mantığı, çok aşamalı iş akışlarındaki veriler manipüle edilerek bozulabilir mi?
- [ ] No — durum (state) sunucuda güvenli bir şekilde tutulur ve önceki adımlardan gelen veriler **değiştirilemez**  
- [ ] Evet — doğrulama ilk adımlarda gerçekleşir ancak son gönderim verilerin bütünlüğünü **tekrar doğrulamaz**  
- [ ] Evet — adımları atlayarak veya sıra dışı veriler göndererek iş akışı atlatma (workflow bypass) **mümkündür**  

### Uygulama, iş kısıtlamalarını aşan "uç durum" (edge case) verilerini düzgün bir şekilde yönetiyor mu?
- [ ] Evet — kısıtlamalar sağlamdır ve olağandışı ancak sözdizimsel olarak geçerli girdileri (örneğin artık yıllar, saat dilimi değişimleri) işler  
- [ ] Evet — bazı uç durumlar işlenir ancak belirli kombinasyonlar beklenmedik davranışlara yol **açabilir**  
- [ ] Hayır — uç durumlar; ücretsiz satın alımlar veya yetkisiz durum değişiklikleri gibi doğrudan mantık hatalarına yol açar

---

## WSTG-BUSL-02 — İstek Sahteciliği Kabiliyetinin Test Edilmesi

İş mantığı (Business Logic) bağlamında isteklerin sahte olarak oluşturulması (Forging requests), hedeflenen iş akışının veya yetkilendirme seviyesinin dışındaki eylemleri gerçekleştirmek amacıyla uygulama tarafından tanımlanan parametrelerin manipüle edilmesini içerir. Saldırganlar; ödeme sistemleri veya hesap kaydı gibi, durumun (state) istemci tarafındaki parametreler aracılığıyla korunduğu ve sunucunun bu parametreleri güvenilir bir arka uç kaynağına karşı yeniden doğrulamadığı çok adımlı süreçleri hedeflerler. Gizli alanların (hidden fields), JSON değerlerinin veya fiyatlar, miktarlar ya da dahili durum bayrakları (internal status flags) gibi REST parametrelerinin değiştirilmesi yoluyla bir saldırgan; ödeme gereksinimlerini atlatabilir, yetki yükseltebilir (escalate privileges) veya istenmeyen durum değişikliklerini tetikleyebilir. Bu zafiyet genellikle, sunucunun bir işlem sırasında iş mantığı durumuna ilişkin istemci beyanına örtülü olarak güvendiği durum bilgisi koruyan (stateful) web formlarında veya API'lerde ortaya çıkar.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-BUSL-02 |
| **CWE** | CWE-602 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Kritik* |

> *Sahte oluşturulan istek; finansal kayba, yetkisiz yönetici eylemlerine veya tam hesap ele geçirmeye (account takeover) olanak tanıyorsa önem derecesi "Kritik" olarak değerlendirilir.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/02-Test_Ability_to_Forge_Requests  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite (Proxy, Repeater, Intruder)`, `Postman`, `OWASP ZAP`, `CyberChef`

### Uygulama, işlem parametrelerini sunucu tarafındaki bir "doğruluk kaynağına" (source of truth) karşı doğruluyor mu?
- [ ] Evet — tüm hassas parametreler (fiyat, rol, ID) veritabanına karşı doğrulanır ve atlatılması (bypass) **mümkün değildir**  
- [ ] Evet — doğrulama uygulanır ancak Parametre Kirliliği (Parameter Pollution) veya alternatif kodlama (encoding) yoluyla atlatılabilir  
- [ ] Hayır — uygulama, bir işlemin maliyetini veya sonucunu belirlemek için istemci tarafındaki değerlere güvenir *(Kritik)*  

### İş açısından kritik değerler (örneğin miktarlar, tutarlar) yetkisiz değerlerle değiştirilebilir mi?
- [ ] Hayır — negatif değerler, sıfır değerleri veya aşırı yüksek değerler sunucu tarafından reddedilir  
- [ ] Evet — sunucu değiştirilmiş değerleri kabul eder ancak sadece sınırlı bir aralık dahilinde  
- [ ] Evet — negatif veya sıfır değerler gönderilebilir, bu da finansal kayıplara veya mantık atlatmalarına yol açar  

### Çok adımlı süreçlerde durumu korumak için gizli alanlar veya API parametreleri kullanılıyor mu?
- [ ] Hayır — durum (state), sunucu tarafındaki oturumlar (sessions) veya imzalı belirteçler (tokens) aracılığıyla güvenli bir şekilde korunur  
- [ ] Evet — durum istemci üzerinden aktarılır ancak bütünlük kontrolleri (HMAC/İmzalar) **etkindir** ve güçlüdür  
- [ ] Evet — durum istemci üzerinden aktarılır ve bütünlük kontrolleri **devre dışıdır** veya kaldırılabilir  

### Bir iş akışındaki zorunlu ara adımları atlayan istekler oluşturmak mümkün mü?
- [ ] Hayır — sunucu, her işlem için katı bir operasyon sırası dayatır  
- [ ] Evet — adımlar atlanabilir ancak nihai eylem hala önceki adımlardan gelen geçerli verileri gerektirir  
- [ ] Evet — yetkilendirme veya ödeme adımlarını atlamak için nihai "gönder" (submit) veya "tamamla" (complete) isteği doğrudan sahte olarak oluşturulabilir

---

## WSTG-BUSL-03 — Bütünlük Kontrollerinin Test Edilmesi

Bütünlük kontrolü testi (Integrity check testing), verilerin çeşitli iş süreçlerinden geçerken veya istemci ile sunucu arasında aktarılırken değiştirilmeden kalmasını ve güvenilirliğinin korunmasını doğrulamaya odaklanır. Saldırganlar; HTTP istekleri, gizli alanlar (hidden fields) veya çerezler (cookies) içerisindeki ürün fiyatları, miktarlar, kullanıcı yetkileri veya işlem durumları gibi kritik parametreleri manipüle ederek (tampering) bu mantığı hedef alırlar. Uygulama; sunucu taraflı doğrulama (server-side verification) veya Karma İşlemi (Hashing) ya da HMAC gibi güçlü bütünlük kontrollerinden yoksunsa, bir saldırgan bu değerleri finansal dolandırıcılık yapmak, kendi yetkilerini yükseltmek veya hedeflenen iş kısıtlamalarını atlatmak (bypass) için manipüle edebilir. Bu test; finansal işlemler, hassas durum geçişleri veya bir aşamanın çıktısının bir sonraki aşama için güvenilir girdi işlevi gördüğü çok adımlı iş akışlarını (workflows) yöneten tüm uygulamalar için hayati önem taşır.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-BUSL-03 |
| **CWE** | CWE-353 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek* |

> *Bütünlük hatasının finansal hırsızlığa, yetkisiz yetki yükseltmeye (privilege escalation) veya kalıcı kayıtların değiştirilmesine olanak tanıması durumunda önem derecesi Yüksek (High) hale gelir.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/03-Test_Integrity_Checks  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite (Proxy/Repeater)`, `CyberChef`, `Postman`, `Python (Requests)`

### Hassas iş parametreleri istemci taraflı manipülasyona (tampering) açık mı?
- [ ] Hayır — hassas değerler yalnızca sunucu tarafında yönetilmektedir  
- [ ] Evet — fiyat, miktar veya durum bayrakları (status flags) gibi değerler dışa aktarılmaktadır ancak şifrelenmiştir  
- [ ] Evet — değerler; gizli alanlar, URL parametreleri veya çerezler içerisinde düz metin (plain text) olarak dışa aktarılmaktadır  

### İstemci kontrollü parametrelere bir bütünlük mekanizması (örneğin; HMAC, Sağlama Toplamı (Checksum)) uygulanıyor mu?
- [ ] Evet — her istekte güçlü bir bütünlük kontrolü uygulanmakta ve doğrulanmaktadır  
- [ ] Evet — bir bütünlük kontrolü uygulanmaktadır ancak zayıf/tahmin edilebilir bir algoritma veya sabit kodlanmış (hardcoded) bir gizli anahtar (secret) kullanılmaktadır  
- [ ] Hayır — hassas parametrelere herhangi bir bütünlük kontrolü uygulanmamaktadır  

### Bütünlük kontrolü bir saldırgan tarafından atlatılabilir (bypass) veya yeniden hesaplanabilir mi?
- [ ] Hayır — imza doğrulaması (signature verification) sıkı bir şekilde uygulanmaktadır ve atlatılması mümkün değildir  
- [ ] Evet — imza parametresi (signature parameter) atlanarak imza doğrulaması devre dışı bırakılabilir  
- [ ] Evet — gizli anahtar veya mantık açığa çıktığı/zayıf olduğu için imza yeniden hesaplanabilir  

### Uygulama, verilerin güvenilir bir kaynağa karşı arka uçta (backend) yeniden doğrulamasını gerçekleştiriyor mu?
- [ ] Evet — sunucu, tüm değerleri veri tabanına karşı yeniden doğrular ve atlatılması mümkün değildir  
- [ ] Evet — sunucu kısmi doğrulama yapar ancak belirli uç durumlar (edge cases) üzerinden atlatılması mümkündür  
- [ ] Hayır — sunucu, arka uç doğrulaması yapmadan istemci tarafından sağlanan verilere güvenir *(Kritik)*  

### Çok adımlı bir sürecin durumunu (state) manipüle etmek mümkün mü?
- [ ] Hayır — oturum durumu (session state) sunucu tarafında yönetilmektedir ve manipüle edilemez  
- [ ] Evet — durum bilgisi istemci üzerinden iletilmektedir ve adımları atlamak veya işlemleri tekrarlamak için manipülasyon mümkündür

---

## WSTG-BUSL-04 — İşlem Zamanlama Testi (Test for Process Timing)

İşlem zamanlama saldırıları (Process Timing), dahili durumlar veya verilerin varlığı hakkında hassas bilgiler elde etmek için bir uygulamanın belirli istekleri işleme süresinin ölçülmesini içerir. Genellikle erken çıkışlı (early-exit) koşullu mantık, veritabanı aramaları veya sabit zamanlı olmayan kriptografik karşılaştırmaların neden olduğu milisaniye düzeyindeki yanıt süresi farklarını analiz ederek, saldırganlar geçerli kullanıcı adlarını numaralandırmak (enumerate), parola parçalarını doğrulamak veya belirli kayıtların varlığını teyit etmek için yan kanal analizi (side-channel analysis) gerçekleştirebilir. Bu güvenlik açıkları tipik olarak kimlik doğrulama uç noktalarında, parola sıfırlama formlarında ve arka uç mantığının girdi geçerliliğine göre dallandığı kaynak yoğunluklu API filtrelerinde ortaya çıkar. Bir saldırganın perspektifinden, bu zamanlama farkları, uygulama kullanıcıya özdeş ve genel hata mesajları döndürse bile arka uç verileri hakkındaki gerçekleri ortaya çıkaran bir "mantıksal kehanet" (boolean oracle) görevi görür.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-BUSL-04 |
| **CWE** | CWE-208 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta (Medium) |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/04-Test_for_Process_Timing  
* https://hacktricks.wiki/en/pentesting-web/timing-attacks.html  
* https://portswigger.net/web-security/race-conditions  

**Araçlar:** `Burp Suite (Timing Advisor / Logger++)`, `ffuf`, `curl`, `Python (time module)`, `THC-Hydra`

### Uygulama, geçerli ve geçersiz kaynak istekleri için farklı yanıt süreleri sergiliyor mu?
- [ ] Hayır — yanıt süreleri, girdi geçerliliğinden bağımsız olarak aynıdır  
- [ ] Evet — ihmal edilebilir zamanlama farkları mevcuttur ancak istatistiksel olarak anlamlı **değildir**  
- [ ] Evet — tutarlı zamanlama farkları **gözlemlenebilir** düzeydedir ve güvenilir bir kehanet (oracle) sağlar  

### Yanıt sürelerini normalize etmek için sabit zamanlı (constant-time) karşılaştırma fonksiyonları veya yapay gecikmeler uygulandı mı?
- [ ] Evet — tüm hassas karşılaştırma işlemlerine sabit zamanlı (constant-time) algoritmalar **uygulanmıştır** *(En güvenli)*  
- [ ] Evet — yapay gecikmeler veya rastgele gecikme varyasyonları (jitter) **etkindir**, ancak temel mantık sabit zamanlı değildir  
- [ ] Hayır — herhangi bir zamanlama iyileştirmesi **uygulanmamıştır**, bu da mantıksal dallanmaların doğrudan ölçülmesine olanak tanır  

### Bir saldırgan, zamanlama sinyalini ağ gürültüsünden ayırmak için istatistiksel analiz kullanabilir mi?
- [ ] Hayır — ağdaki gecikme dalgalanmaları (jitter) ve uygulama gürültüsü zamanlama analizini **mümkün kılmaz**  
- [ ] Evet — yeterli örneklem büyüklüğü ile zamanlama sinyali gürültüden **ayrıştırılabilir**  
- [ ] Evet — zamanlama farkı (delta) istatistiksel normalleştirme **gerektirmeyecek** kadar büyüktür  

### Oturum açma veya parola sıfırlama işlevleri aracılığıyla kullanıcı numaralandırma (account enumeration) mümkün mü?
- [ ] Hayır — hesap varlığı zamanlama yoluyla tespit edilemez  
- [ ] Evet — zamanlama farklılıkları, uzak bir saldırganın geçerli kullanıcı adlarını veya e-postaları **numaralandırmasına** (enumerate) olanak tanır  

### Kaynak yoğunluklu işlemler (örneğin; dosya işleme, karmaşık aramalar) zamanlama yoluyla veri varlığı sızıntısına neden oluyor mu?
- [ ] Hayır — kaynak tüketimi, bir kaydın bulunup bulunmadığından bağımsız olarak sabittir  
- [ ] Evet — hassas kayıtların varlığı, arka uç işlem süresinin ölçülmesiyle **anlaşılabilir**

---

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

## WSTG-BUSL-06 — İş Akışlarını Atlatma Testi (Work Flow Circumvention)

İş akışını atlatma (Workflow circumvention), çok adımlı süreçlerde hedeflenen sıralamaları veya ön koşulları devre dışı bırakmak için uygulama mantığının manipüle edilmesini içerir. Saldırganlar; ödeme onayları, idari onaylar veya MFA tamamlama ekranları gibi hassas uç noktaları (endpoints) tespit eder ve zorunlu ara adımları atlayarak bu noktalara doğrudan erişmeye çalışırlar. Bu zafiyet, sunucu tarafı mantığının sağlam bir durum makinesi (state machine) sürdürmemesi ve bunun yerine kullanıcının kullanıcı arayüzü (UI) odaklı yolu izleyeceği varsayımına güvenmesi durumunda ortaya çıkar. Başarılı bir istismar; yetkisiz işlemler, yetki yükseltme (privilege escalation) ve kimlik doğrulama mekanizmalarının atlatılması dahil olmak üzere kritik iş mantığı (business logic) hatalarına yol açabilir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-BUSL-06 |
| **CWE** | CWE-841 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek* |

> *Atlatmanın yetkisiz finansal işlemlere, yetki yükseltmeye veya idari erişime izin vermesi durumunda önem derecesi Yüksek olur.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/06-Testing_for_the_Circumvention_of_Work_Flows  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**Araçlar:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Postman`, `Caido`

### Uygulama çok aşamalı iş süreçleri içeriyor mu?
- [ ] Hayır — uygulama yalnızca tek isteklik eylemlerden oluşuyor  
- [ ] Evet — çok aşamalı süreçler (örneğin; alışveriş sepetleri, sihirbaz formları, kayıt işlemleri) mevcuttur  

### Adım sıralaması sunucu tarafından sıkı bir şekilde denetleniyor mu?
- [ ] Evet — sunucu tarafı durum takibi (state tracking), adımların atlanamamasını sağlar  
- [ ] Evet — istemci tarafı kontroller bir sıralama önerir, ancak sunucu ilerlemeyi doğrulamaz  
- [ ] Hayır — uygulama belirli bir işlem sırasını zorunlu kılmaz  

### Bir saldırgan, terminal URL'sini veya uç noktasını (endpoint) doğrudan talep ederek bir sürecin son aşamasına ulaşabilir mi?
- [ ] Hayır — terminal adımlarına, önceki adımlar tamamlanmadan doğrudan erişim mümkün değildir  
- [ ] Evet — terminal adımlarına (örneğin; `/api/v1/checkout/complete`) doğrudan istek ile ulaşılabilir  

### Uygulama, önceki adımlardan gelen ön koşul verilerinin mevcut ve geçerli olduğunu doğruluyor mu?
- [ ] Evet — sunucu devam etmeden önce tüm önceki adım verilerini doğrular  
- [ ] Hayır — sunucu, verinin beklendiği ancak doğrulanmadığı durumlarda adımların "atlanmasına" karşı zafiyet barındırır  

### MFA veya kimlik doğrulama gibi güvenlik kontrolleri iş akışı manipüle edilerek atlatılabilir mi?
- [ ] Hayır — kritik kontroller akış manipülasyonu yoluyla atlatılamaz  
- [ ] Evet — doğrulama sonrası adıma atlayarak MFA veya kimlik kontrollerinin atlatılması mümkündür

---

## WSTG-BUSL-07 — Uygulama Suistimaline Karşı Savunmaların Test Edilmesi

Uygulama suistimaline karşı savunmalar; uygulamanın, amaçlanan iş mantığından (Business Logic) sapan anormal kullanıcı davranışlarını tanımlama, günlüğe kaydetme (logging) ve bunlara yanıt verme yeteneğini değerlendirir. Geleneksel imza tabanlı güvenliğin aksine bu savunmalar; hızlı istekler (rapid-fire requests), kimlik bilgisi doldurma (Credential Stuffing) veya otomatik suistimali işaret eden ardışık kaynak numaralandırma (sequential resource enumeration) gibi kalıpları tespit etmeye odaklanır. Bir saldırganın perspektifinden bu test; uygulamanın, standart güvenlik uyarılarını tetiklemeden veri sızdırmak veya kaynakları tüketmek için tasarlanan otomasyona ve "düşük ve yavaş" (low and slow) saldırılara karşı direncini belirler. Bu savunmaların uygulanmaması; genellikle büyük ölçekli veri kazıma (data scraping), hesap ele geçirme (account takeover) veya meşru ancak kaynak yoğun uygulama özellikleri üzerinden hizmet dışı bırakma (Denial of Service - DoS) ile sonuçlanır.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-BUSL-07 |
| **CWE** | CWE-799 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/07-Test_Defenses_Against_Application_Misuse  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite (Intruder/Turbo Intruder)`, `OWASP ZAP`, `Python (Requests/Selenium)`, `Caido`, `K6`

### Hassas uç noktalarda hız sınırlama veya kısıtlama mekanizmaları zorunlu kılınmış mı?
- [ ] Evet — tüm hassas uç noktalarda katı hız sınırlama (Rate Limiting) **uygulanmaktadır** ve zorunludur *(En güvenli)*  
- [ ] Evet — hız sınırlama **uygulanmaktadır** ancak IP rotasyonu veya başlık (header) manipülasyonu ile atlatılabilmektedir  
- [ ] Evet — hız sınırlama yalnızca kimlik doğrulama uç noktalarına **uygulanmaktadır**, diğerleri açıkta bırakılmıştır  
- [ ] Hayır — hiçbir uygulama özelliği için hız sınırlama veya kısıtlama (throttling) **uygulanmamaktadır**  

### Uygulama, otomatik etkileşim kalıplarını tespit edip engelliyor mu?
- [ ] Evet — davranışsal analiz veya CAPTCHA'lar **etkindir** ve botları etkili bir şekilde engellemektedir  
- [ ] Evet — otomasyon karşıtı önlemler **etkindir** ancak başlıksız tarayıcılar (headless browsers) veya oturum döngüsü (session cycling) kullanılarak atlatma **mümkündür**  
- [ ] Hayır — uygulama, insan kullanıcı ile otomatik bir betik (script) arasındaki farkı **ayırt edememektedir**  

### Anormal davranışlar günlüğe kaydediliyor mu ve ardından savunmacı bir yanıt veriliyor mu?
- [ ] Evet — suistimal, otomatik engellemeyi tetikler ve güvenlik ekibini uyarır  
- [ ] Evet — suistimal denetim için günlüğe kaydedilir ancak otomatik bir savunma eylemi **gerçekleştirilmez**  
- [ ] Hayır — uygulama, olağan dışı aktivite kalıplarını günlüğe **kaydetmemekte** veya bunlara yanıt **vermemektedir**  

### Savunmalar, istemci tarafı tanımlayıcıları veya başlıkları manipüle edilerek atlatılabilir mi?
- [ ] Hayır — savunmalar, sunucu tarafı durumuna ve doğrulanmış telemetriye dayanmaktadır  
- [ ] Evet — kontroller, çerezleri (cookies) temizleyerek veya `User-Agent` dizelerini döndürerek **atlatılabilmektedir**  
- [ ] Evet — kontroller, `X-Forwarded-For` veya `X-Real-IP` gibi başlıklar (headers) enjekte edilerek **atlatılabilmektedir**

---

## WSTG-BUSL-08 — Beklenmeyen Dosya Türlerinin Yüklenmesinin Test Edilmesi

Beklenmeyen dosya türlerinin yüklenmesi, saldırganların uygulamanın işlemeyi amaçlamadığı dosyaları göndererek iş mantığı kısıtlamalarını atlamasına olanak tanır ve potansiyel olarak Remote Code Execution (RCE), Cross-Site Scripting (XSS) veya Denial of Service (DoS) saldırılarına yol açabilir. Saldırganlar, sunucu tarafındaki doğrulama mantığını kötü amaçlı içeriği kabul etmesi için kandırmak amacıyla dosya uzantılarını, MIME türlerini ve Magic Bytes (sihirli baytlar) değerlerini değiştirerek bu zafiyetleri yoklarlar. Bu durum tipik olarak, arka uçta (back-end) yetersiz doğrulamanın olduğu profil resmi yüklemelerinde, doküman yönetim sistemlerinde veya destek talebi eklerinde meydana gelir. Başarılı bir istismar (Exploit), yüklenen dosyanın çalıştırılması durumunda ana sunucuyu tehlikeye atabilir veya dosyanın yanlış bir Content-Type ile diğer kullanıcılara sunulması durumunda istemci tarafı (client-side) saldırılarına yol açabilir.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-BUSL-08 |
| **CWE** | CWE-434 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Kritik |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/08-Test_Upload_of_Unexpected_File_Types  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**Araçlar:** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`

### Uygulama, dosya yüklemelerini belirli bir uzantı kümesiyle sınırlandırıyor mu?
- [ ] Evet — katı bir uzantı izin listesi (whitelist) uygulanmaktadır ve atlatılması (bypass) **mümkün değildir**  
- [ ] Evet — uzantı izin listesi mevcuttur ancak çift uzantı veya null byte'lar ile atlatılması **mümkündür**  
- [ ] Hayır — herhangi bir dosya uzantısı **yüklenebilir**  

### Dosya içeriği, dosya uzantısının ötesinde doğrulanıyor mu?
- [ ] Evet — sunucu tarafı mantığı Magic Bytes doğrulaması yapar ve derin içerik incelemesi gerçekleştirir  
- [ ] Evet — sunucu tarafı mantığı MIME türünü yalnızca `Content-Type` başlığı üzerinden doğrular  
- [ ] Hayır — uzantı kontrolünden sonra herhangi bir içerik doğrulaması **uygulanmamaktadır**  

### Bir saldırgan, web shell veya betik yükleyerek kod çalıştırabilir mi?
- [ ] Hayır — yüklenen dosyalar yürütülemez (non-executable) bir dizinde veya bir depolama biriminde (S3/Azure Blob) saklanmaktadır  
- [ ] Evet — dosyalar web üzerinden erişilebilir bir dizinde saklanmaktadır ancak sunucu yapılandırması yoluyla çalıştırma **devre dışı bırakılmıştır**  
- [ ] Evet — yüklenen kötü amaçlı betikler, öngörülebilir bir URL yolu üzerinden doğrudan **çalıştırılabilir** *(Kritik)*  

### Yüklenen dosya türleri üzerinden XSS gibi istemci tarafı saldırıları mümkün müdür?
- [ ] Hayır — dosyalar `Content-Disposition: attachment` ve `X-Content-Type-Options: nosniff` başlıkları ile sunulmaktadır  
- [ ] Evet — SVG veya HTML dosyaları **yüklenebilir** ve tarayıcıda işlenerek XSS'e yol açabilir  
- [ ] Evet — görüntü meta verileri (EXIF) **işlenmekte** ve temizleme (sanitization) yapılmadan kullanıcı arayüzünde yansıtılmaktadır

---

## WSTG-BUSL-09 — Zararlı Dosya Yükleme Testi (Test Upload of Malicious Files)

Dosya yükleme fonksiyonu, kullanıcıların sunucuya veri göndermesine olanak tanıyarak uygulama ortamına zararlı içerik aktarılması için doğrudan bir vektör sağlar. Saldırganlar; Web Shell, kötü amaçlı yazılım (Malware) veya SVG ya da HTML gibi özel olarak yapılandırılmış dosyaları yüklemek için zayıf doğrulama mantıklarını istismar ederek Uzaktan Kod Çalıştırma (Remote Code Execution - RCE) veya Siteler Arası Betik Çalıştırma (Cross-Site Scripting - XSS) elde edebilirler. Bu zafiyet genellikle profil ayarları, doküman yönetim sistemleri ve sunucunun dosya türlerini, içeriğini ve çalıştırma izinlerini düzgün bir şekilde doğrulamadığı ek dosyası işleyicileri gibi özelliklerde bulunur. Başarılı bir istismar genellikle tam sistem ele geçirme, veri sızıntısı (Data Exfiltration) veya sunucunun iç ağ saldırıları için bir atlama noktası (Pivot Point) olarak kullanılmasıyla sonuçlanır.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-BUSL-09 |
| **CWE** | CWE-434 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Kritik |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/09-Test_Upload_of_Malicious_Files  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**Araçlar:** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`, `Weevely`

### Uygulama dosya yükleme fonksiyonu sağlıyor mu?
- [ ] Hayır — herhangi bir dosya yükleme fonksiyonu bulunmamaktadır  
- [ ] Evet — fonksiyon, kimliği doğrulanmış kullanıcılar için mevcuttur  
- [ ] Evet — fonksiyon, **kimliği doğrulanmamış** kullanıcılar için mevcuttur *(Kritik)*  

### Sunucu tarafında dosya uzantısı doğrulaması zorunlu tutuluyor mu?
- [ ] Evet — katı bir uzantı izin listesi (Allowlist) **uygulanmaktadır** ve bypass **mümkün değildir**  
- [ ] Evet — bir izin listesi kullanılmaktadır ancak büyük/küçük harf duyarlılığı veya çift uzantı yoluyla bypass **mümkündür**  
- [ ] Evet — bir engelleme listesi (Blocklist) kullanılmaktadır ve bu liste alternatif uzantılarla (örneğin `.phtml`, `.asa`) bypass **edilebilir**  
- [ ] Hayır — sunucu tarafında herhangi bir uzantı doğrulaması **uygulanmamaktadır**  

### Dosya içeriği, uzantı veya MIME türünün ötesinde doğrulanıyor mu?
- [ ] Evet — uygulama sihirli baytları (Magic Bytes) doğrular ve derin içerik incelemesi gerçekleştirir  
- [ ] Evet — uygulama yalnızca kolayca taklit edilebilen (Spoofed) `Content-Type` başlığını kontrol eder  
- [ ] Hayır — uygulama doğrulama için tamamen dosya uzantısına güvenmektedir  

### Yüklenen dosyalar çalıştırma izinlerine sahip bir dizinde mi saklanıyor?
- [ ] Hayır — dosyalar özel bir depolama servisinde (örneğin S3) veya çalıştırılamaz bir dizinde saklanmaktadır  
- [ ] Evet — dosyalar web sunucusunda saklanmaktadır ancak yapılandırma yoluyla çalıştırma **devre dışı bırakılmıştır**  
- [ ] Evet — dosyalar web üzerinden erişilebilir bir dizinde saklanmaktadır ve çalıştırma **etkindir** *(Kritik)*  

### Güvenlik filtreleri dosya adı manipülasyonu ile bypass edilebilir mi?
- [ ] Hayır — dosya adları sunucu tarafından sanitize edilir veya rastgele yeniden oluşturulur  
- [ ] Evet — filtreler null byte'lar (örneğin `shell.php%00.jpg`) kullanılarak bypass **edilebilir**  
- [ ] Evet — dizin atlatma (Path Traversal) karakterleri (örneğin `../../`), dosyaları rastgele dizinlere yüklemek için **kullanılabilir**

---

## WSTG-BUSL-10 — Ödeme Fonksiyonelliğinin Test Edilmesi

Ödeme fonksiyonelliğinin test edilmesi, bir saldırganın ödeme geçitlerini (Payment Gateway) atlatmasına, sipariş toplamlarını manipüle etmesine veya başarılı işlem sinyallerini taklit etmesine olanak tanıyan iş mantığı (Business Logic) hatalarının tespit edilmesini içerir. Bu zafiyetler genellikle uygulama, istemci tarayıcısı ve Stripe, PayPal gibi üçüncü taraf ödeme işlemcileri veya dahili muhasebe sistemleri arasındaki iletişimde bulunur. Bir saldırgan, iletim sırasındaki fiyat parametrelerini değiştirmeyi, toplam maliyeti düşürmek için negatif miktarlar göndermeyi veya gerçek bir ödeme yapmadan siparişleri tamamlamak için başarılı işlem geri çağırmalarını (Callback) yeniden oynatmayı (Replay) deneyebilir. Başarılı bir istismar (Exploit), doğrudan finansal kayba, envanter tutarsızlığına ve uygulamanın e-ticaret bütünlüğünün potansiyel olarak bozulmasına yol açar.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-BUSL-10 |
| **CWE** | CWE-840 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Kritik |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/10-Test-Payment-Functionality  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite`, `Turbo Intruder`, `Postman`, `Hackvertor`

### İşlem tutarı işlenmeden önce sunucu tarafında doğrulanıyor mu?
- [ ] Evet — tutar, sunucu tarafındaki ürün veritabanına göre sıkı bir şekilde doğrulanmaktadır *(En güvenli)*  
- [ ] Evet — tutar doğrulanmaktadır ancak Parameter Pollution veya para birimi değiştirme yoluyla mantık atlatma (Logic Bypass) **mümkündür**  
- [ ] Hayır — işlem tutarı istemci tarafındaki istekte **değiştirilebilir** ve arka uç tarafından kabul edilir  

### Ürün miktarları, sıfır veya negatif bir toplama neden olacak şekilde manipüle edilebilir mi?
- [ ] Hayır — negatif veya sıfır miktarlar uygulama doğrulama mantığı tarafından reddedilir  
- [ ] Evet — negatif miktarlar sepette kabul edilir ancak toplam tutar ödeme (Checkout) sırasında düzeltilir  
- [ ] Evet — negatif miktarlar toplam sipariş tutarını düşürmek veya kredi elde etmek için **kullanılabilir** *(Kritik)*  

### Uygulama, ödeme geçidi geri çağırmalarını (Callback) veya Webhook'ları güvenli bir şekilde işliyor mu?
- [ ] Evet — geri çağırmalar, taklit edilemeyen (Spoof) geçerli bir kriptografik imza gerektirir  
- [ ] Evet — imzalar kontrol edilir ancak gizli anahtar (Secret) zayıftır veya doğrulama **atlatılabilir**  
- [ ] Hayır — uygulama, ödeme durumunu onaylamak için kimliği doğrulanmamış veya doğrulanmamış geri çağırmalara dayanır  

### Uygulama, ödeme veya çıkış aşamasında yarış durumlarına (Race Condition) karşı savunmasız mı?
- [ ] Hayır — işlemsel bütünlük ve veritabanı kilitleme mekanizmaları **etkindir**  
- [ ] Evet — Race Condition **mümkündür** ancak nihai fiyatı veya sipariş karşılamayı etkilemez  
- [ ] Evet — eşzamanlı istekler, tek bir ödeme için birden fazla sipariş karşılamaya veya hediye kartlarının "mükerrer harcanmasına" (Double-spending) yol **açabilir**  

### İndirim kodları veya sadakat puanları manipülasyona veya yeniden kullanıma tabi mi?
- [ ] Hayır — kodlar tek kullanımlık olarak doğrulanır ve mantıksal kısıtlamalar **uygulanır**  
- [ ] Evet — kodlar yeniden kullanılabilir veya uygun olmayan öğelere uygulanabilir, ancak etki sınırlıdır  
- [ ] Evet — saldırganlar birden fazla çelişen kod **uygulayabilir** veya son kullanma tarihi ve kullanım sınırlarını atlatabilir

---

## WSTG-CLNT-01 — DOM Tabanlı Cross-Site Scripting (XSS) Testi

DOM tabanlı Cross-Site Scripting (DOM XSS), bir uygulama güvenilmeyen bir kaynaktan gelen veriyi güvenli olmayan bir şekilde işleyen istemci tarafı JavaScript (client-side JavaScript) içerdiğinde, genellikle veriyi tehlikeli bir alıcı (sink) aracılığıyla Belge Nesne Modeli (DOM) üzerine geri yazdığında meydana gelir. Reflected veya Stored XSS'in aksine, bu zafiyet tamamen istemci tarafı kodunda mevcuttur; `innerHTML`, `eval()` veya `document.write()` gibi alıcılar tarafından işlendiği için yük (Payload) genellikle sunucuya hiç gönderilmez. Saldırganlar, kurbanın tarayıcısı tarafından yüklendiğinde kullanıcının oturum bağlamında rastgele JavaScript yürüten, kötü amaçlı fragmanlar veya parametreler içeren URL'ler hazırlayarak bu durumdan faydalanırlar. Bu durum, oturum belirteçlerinin (session tokens) sızdırılmasına, hassas veri hırsızlığına ve kimliği doğrulanmış kullanıcı adına yetkisiz işlemler gerçekleştirilmesine yol açabilir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CLNT-01 |
| **CWE** | CWE-79 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/01-Testing_for_DOM-based_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  
* https://portswigger.net/web-security/cross-site-scripting  
* https://portswigger.net/web-security/dom-based  

**Araçlar:** `Burp Suite (DOM Invader)`, `Browser DevTools`, `KNOXSS`, `XSStrike`, `Snyk (SAST)`

### JavaScript içerisinde kullanıcı kontrollü verileri yakalamak için güvenilmeyen kaynaklar kullanılıyor mu?
- [ ] Hayır — JavaScript `location.hash`, `location.search` veya `document.referrer` **kullanmamaktadır**  
- [ ] Evet — güvenilmeyen kaynaklar kullanılmaktadır ancak veri herhangi bir yürütme alıcısına (execution sink) **iletilmemektedir**  
- [ ] Evet — güvenilmeyen kaynaklar kullanılmaktadır ve veri doğrudan istemci tarafı mantığına akmaktadır  

### Kullanıcı kontrollü veri, tehlikeli DOM alıcılarına (sinks) iletiliyor mu?
- [ ] Hayır — `innerHTML`, `outerHTML`, `document.write()` veya `eval()` gibi alıcılar **kullanılmamaktadır**  
- [ ] Evet — tehlikeli alıcılar mevcuttur ancak veri `DOMPurify` gibi güvenli bir kütüphane kullanılarak sıkı bir şekilde **temizlenmektedir (sanitized)**  
- [ ] Evet — tehlikeli alıcılar mevcuttur ve veri **yetersiz** veya özel regex tabanlı filtreleme ile işlenmektedir  
- [ ] Evet — tehlikeli alıcılar mevcuttur ve veri herhangi bir temizleme veya kodlama (encoding) **yapılmadan** iletilmektedir *(Kritik)*  

### Yürütme alıcısına (execution sink) URL tabanlı bir yük (Payload) ile ulaşılabilir mi?
- [ ] Hayır — kaynaktan alıcıya (source-to-sink) akış, harici girdi yoluyla **tetiklenemez**  
- [ ] Evet — akış **mümkündür** ancak karmaşık kullanıcı etkileşimi gerektirir  
- [ ] Evet — akış **mümkündür** ve basit bir URL fragmanı veya sorgu parametresi ile tetiklenebilir  

### Modern güvenlik başlıkları veya tarayıcı tabanlı hafifletmeler riski etkisiz hale getiriyor mu?
- [ ] Evet — `script-src` ve `trusted-types` içeren sıkı bir `Content-Security-Policy` (CSP) **etkindir** ve yürütmeyi engeller  
- [ ] Evet — bir CSP mevcuttur ancak `unsafe-inline` veya zayıf hash değerleri bir atlatmayı (bypass) **mümkün** kılmaktadır  
- [ ] Hayır — DOM tabanlı yürütmeyi hafifletmek için bir CSP bulunmamaktadır  

### Uygulama, yerleşik korumalara sahip istemci tarafı framework'leri kullanıyor mu?
- [ ] Evet — framework (örneğin; Angular, React) veriyi otomatik olarak kodlar ve atlatma (bypass) **mümkün değildir**  
- [ ] Evet — framework kullanılmaktadır ancak `dangerouslySetInnerHTML` gibi "kaçış noktaları" **etkindir**  
- [ ] Hayır — uygulama, otomatik kaçış (auto-escaping) özelliği olmayan yalın JavaScript (vanilla JavaScript) veya eski kütüphaneler (örneğin; eski jQuery sürümleri) kullanmaktadır

---

## WSTG-CLNT-02 — JavaScript Yürütme Testi

JavaScript yürütme testi, bir uygulamanın kullanıcı tarafından sağlanan verileri hatalı şekilde işlemesi sonucunda mağdurun tarayıcı bağlamında yetkisiz betiklerin çalıştırılmasına neden olan durumların tespit edilmesine odaklanır. Bu zafiyet tipik olarak, saldırganların oturum çerezlerini (session cookies) ele geçirmesine, Document Object Model (DOM) yapısını manipüle etmesine veya kullanıcı adına eylemler gerçekleştirmesine olanak tanıyan Cross-Site Scripting (XSS) ile sonuçlanır. Sızma testi uzmanları; URL fragmanları, `localStorage` veya `window.name` gibi kaynaklardan (sources) gelen sanitize edilmemiş verilerin işlenebileceği `innerHTML`, `document.write()` ve `eval()` gibi alıcıları (sinks) inceler. Başarılı bir istismar, kullanıcı oturumunun bütünlüğünü ve gizliliğini tehlikeye atar ve daha karmaşık istemci taraflı (client-side) saldırılar için bir basamak (pivot) görevi görebilir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CLNT-02 |
| **CWE** | CWE-79 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/02-Testing_for_JavaScript_Execution  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/dom-based  
* https://portswigger.net/web-security/prototype-pollution  

**Araçlar:** `Burp Suite`, `Browser Developer Tools`, `XSStrike`, `DOMPurify`, `KNOXSS`

### Uygulama, kullanıcı tarafından sağlanan verileri tehlikeli JavaScript alıcılarında (sinks) işliyor mu?
- [ ] Hayır — tüm veriler sanitize edilmiştir veya `textContent` gibi güvenli alıcılarda kullanılmaktadır  
- [ ] Evet — veriler tehlikeli alıcılarda işlenmektedir ancak sanitization/encoding **uygulanmıştır**  
- [ ] Evet — veriler tehlikeli alıcılarda işlenmektedir ve sanitization **uygulanmamıştır**  

### Betik yürütmeyi hafifletmek için bir Content Security Policy (CSP) mevcut mu?
- [ ] Evet — kısıtlayıcı bir CSP **etkindir** ve bypass **mümkün değildir**  
- [ ] Evet — bir CSP **etkindir** ancak `unsafe-inline` veya güvensiz CDN'ler aracılığıyla bypass **mümkündür**  
- [ ] Hayır — herhangi bir CSP **etkin değildir**  

### JavaScript, URL parametreleri veya fragmanları aracılığıyla yürütülebiliyor mu (DOM-based XSS)?
- [ ] Hayır — girdi uygun şekilde encode edilmiştir ve yürütme **mümkün değildir**  
- [ ] Evet — girdi DOM'a yansıtılmaktadır ancak yürütme modern framework korumaları tarafından **engellenmektedir**  
- [ ] Evet — girdi doğrudan tarayıcıda yürütülmektedir ve istismar (exploitation) **mümkündür**  

### Olay işleyiciler (event handlers) veya URI şemaları betik enjeksiyonuna karşı savunmasız mı?
- [ ] Hayır — girdi, olay özniteliklerini ve `javascript:` şemalarını kaldıracak şekilde sanitize edilmiştir  
- [ ] Evet — olay işleyiciler enjekte **edilebilmektedir** ancak filtreler payload karmaşıklığını sınırlandırmaktadır  
- [ ] Evet — `onerror`, `onload` veya `href` öznitelikleri aracılığıyla keyfi JavaScript yürütülmesi **mümkündür**

---

## WSTG-CLNT-03 — HTML Injection

HTML Injection, bir uygulama kullanıcı tarafından sağlanan girdiyi tarayıcıda render etmeden önce uygun şekilde sanitize etmediğinde (temizlemediğinde) ortaya çıkar ve bir saldırganın web sayfasının Belge Nesne Modeline (DOM) rastgele HTML etiketleri enjekte etmesine olanak tanır. Cross-Site Scripting (XSS) ile benzerlik gösterse de, HTML Injection'ın temel amacı sayfanın görsel sunumunu manipüle etmek veya kötü niyetli formlar ve aldatıcı içerikler enjekte ederek oltalama (phishing) saldırılarını kolaylaştırmaktır. Saldırganlar; arama terimleri, kullanıcı profil alanları veya hata mesajları gibi kullanıcı arayüzünde (UI) yansıyan parametreleri hedef alarak mağdurları hassas kimlik bilgilerini ifşa etmeye veya yetkisiz üçüncü taraf bağlantılarla etkileşime girmeye zorlar. Bu güvenlik açığı, uygulamanın kullanıcı arayüzünün bütünlüğü için önemli bir risk teşkil eder ve daha karmaşık sosyal mühendislik veya oturumla ilgili saldırıların öncüsü olabilir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CLNT-03 |
| **CWE** | CWE-80 |
| **Test Durumu** | Yürütülmedi |
| **Önem Derecesi** | Düşük / Orta |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/03-Testing_for_HTML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite`, `OWASP ZAP`, `DOM Invader`, `Wfuzz`

### Kullanıcı kontrollü girdiler, uygun HTML kodlaması (HTML encoding) yapılmadan DOM'a yansıtılıyor mu?
- [ ] Hayır — yansıyan tüm girdiler, render edilmeden önce kesinlikle HTML kodlamasından geçirilmektedir  
- [ ] Evet — bazı karakterler kodlanmaktadır ancak belirli etiketler için atlatmalar (bypasses) mümkündür  
- [ ] Evet — girdi ham (raw) olarak yansıtılmaktadır ve HTML injection mümkündür  

### Sayfa düzenini değiştirmek için yapısal HTML öğeleri enjekte edilebiliyor mu?
- [ ] Hayır — uygulama `<div>`, `<table>` veya `<iframe>` gibi yapısal etiketleri filtrelemekte veya temizlemektedir  
- [ ] Evet — yapısal öğeler enjekte edilebilmektedir ancak kullanıcı arayüzü (UI) üzerinde önemli bir etkisi yoktur  
- [ ] Evet — enjekte edilen etiketler aracılığıyla önemli ölçüde kullanıcı arayüzü tahrifatı (UI defacement) mümkündür  

### Formlar veya kötü niyetli bağlantılar gibi işlevsel oltalama (phishing) öğeleri enjekte etmek mümkün mü?
- [ ] Hayır — `<form>`, `<input>` ve `<a>` etiketleri etkili bir şekilde engellenmekte veya sanitize edilmektedir  
- [ ] Evet — kullanıcıları harici sitelere yönlendirmek için kötü niyetli bağlantılar enjekte edilebilmektedir  
- [ ] Evet — kimlik bilgilerini toplamak için işlevsel `<form>` öğeleri enjekte edilebilmektedir *(Orta)*  

### Güvenlik başlıkları veya İçerik Güvenliği Politikaları (CSP), enjeksiyonun etkisini azaltıyor mu?
- [ ] Evet — kısıtlayıcı bir CSP yürürlüktedir ve `form-action` veya `frame-src` uygulanmaktadır  
- [ ] No — güvenlik başlıkları mevcuttur ancak CSP devre dışıdır veya unsafe-inline/wildcards (joker karakterler) içermektedir  
- [ ] No — hiçbir CSP veya ilgili güvenlik başlığı etkin değildir

---

## WSTG-CLNT-04 — İstemci Taraflı URL Yönlendirmesi Testi (Testing for Client-Side URL Redirect)

İstemci taraflı URL yönlendirmesi, bir web uygulamasının genellikle URL parametreleri veya hash fragmanları aracılığıyla kullanıcı kontrollü girdi kabul etmesi ve bu girdiyi `window.location` veya `document.location.href` gibi istemci taraflı bir yönlendirme havuzunda (client-side redirection sink) kullanmasıyla oluşur. Bu zafiyet, kurbanın başlangıçta meşru alana güvenmesi ve ardından sessizce kötü niyetli bir siteye yönlendirilmesi nedeniyle, saldırganlar tarafından karmaşık oltalama (phishing) kampanyaları yürütmek için öncelikli olarak kullanılır. Oltalama saldırılarının ötesinde, istemci taraflı yönlendirmeler; yönlendirme sırasında URL'de veya `Referer` başlığında OAuth erişim belirteçleri (OAuth access tokens) veya oturum kimlikleyicileri (session identifiers) gibi hassas veriler bulunuyorsa, bu verilerin sızdırılması için zincirlenebilir. Modern Tek Sayfalı Uygulamalarda (Single Page Applications - SPA), bu zafiyet sıklıkla yönlendirme mantığında (routing logic), kimlik doğrulama sonrası "geri dönüş" (return to) parametrelerinde veya dil değiştirme işlevlerinde görülür.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CLNT-04 |
| **CWE** | CWE-601 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta* |

> *Yönlendirme; OAuth belirteçlerini, oturum kimliklerini (session IDs) sızdırmak veya bir kimlik doğrulama akışındaki güvenlik kontrollerini atlamak için kullanılabiliyorsa önem derecesi Yüksek olur.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/04-Testing_for_Client-side_URL_Redirect  
* https://hacktricks.wiki/en/pentesting-web/open-redirect.html  

**Araçlar:** `Burp Suite (DOM Invader)`, `Browser DevTools`, `LinkFinder`, `Arjun`, `B-Config`

### Uygulama, kullanıcı girdisine dayalı yönlendirmeleri işlemek için istemci taraflı betikler kullanıyor mu?
- [ ] Hayır — yönlendirme, sunucu tarafında yalnızca `3xx` HTTP durum kodları aracılığıyla yönetilir  
- [ ] Evet — uygulama, URL parametrelerine dayalı gezinme için `window.location` gibi JavaScript havuzlarını (sinks) kullanır  
- [ ] Evet — uygulama, kullanıcı tarafından sağlanan yolları işleyen bir istemci taraflı uygulama çatısı yönlendiricisi (örneğin React Router, Vue Router) kullanır  

### Hedef URL için girdi doğrulama veya beyaz listeye alma (whitelisting) kontrolleri uygulanmış mı?
- [ ] Evet — izin verilen alan adları/yollardan oluşan katı bir **beyaz liste** (whitelist) zorunludur ve atlatılması **mümkün değildir**  
- [ ] Evet — doğrulama mevcuttur ancak zayıf regex veya kara listelere (blacklists) dayanmaktadır ve atlatılması **mümkündür**  
- [ ] Hayır — uygulama, yönlendirme hedefi olarak herhangi bir rastgele dizgiyi kabul eder  

### Yönlendirme mantığı yaygın gizleme (obfuscation) teknikleri kullanılarak atlatılabilir mi?
- [ ] Hayır — kontroller; kodlanmış karakterleri, protokolden bağımsız URL'leri (`//`) ve ters eğik çizgileri (backslashes) doğru şekilde işler  
- [ ] Evet — URL kodlama (URL encoding) veya çift kodlama (double-encoding) kullanılarak atlatma **mümkündür**  
- [ ] Evet — protokolden bağımsız URL'ler veya `@` karakterleri (örneğin `https://expected.com@attacker.com`) kullanılarak atlatma **mümkündür**  
- [ ] Evet — belirli tarayıcı tuhaflıkları (quirks) veya null byte enjeksiyonu kullanılarak atlatma **mümkündür**  

### Yönlendirme işlemi hedef siteye hassas veri ifşa ediyor mu?
- [ ] Hayır — URL'de hassas veri bulunmamaktadır veya yönlendirme sırasında başlıklar (headers) aracılığıyla iletilmemektedir  
- [ ] Evet — hassas veriler (örneğin oturum belirteçleri, API anahtarları) `Referer` başlığı aracılığıyla harici siteye **sızdırılmaktadır**  
- [ ] Evet — URL fragmanında (`#`) bulunan hassas verilere hedef sitenin betikleri tarafından **erişilebilmektedir**  

### Yönlendirme havuzu (sink) aracılığıyla JavaScript çalıştırmak mümkün mü (DOM tabanlı XSS)?
- [ ] Hayır — havuz yalnızca `http` veya `https` protokollerine geçişe izin verir  
- [ ] Evet — yönlendirme havuzu `javascript:` veya `data:` URI'larına izin vererek DOM tabanlı XSS'i (DOM-based XSS) **mümkün kılar**

---

## WSTG-CLNT-05 — CSS Enjeksiyonu Testi (CSS Injection)

CSS Enjeksiyonu (CSS Injection), bir uygulamanın kullanıcı tarafından sağlanan girdilerin, uygun sanitizasyon veya kaçış (escaping) işlemleri yapılmadan bir web sayfasının Basamaklı Stil Şablonlarını (Cascading Style Sheets - CSS) etkilemesine izin vermesiyle oluşur. Genellikle kozmetik bir sorun olarak algılansa da saldırganlar, CSS enjeksiyonunu; öznitelik seçicileri (attribute selectors) ve `background-image` özelliklerini kullanarak harici istekleri tetiklemek suretiyle CSRF token'ları veya oturum ID'leri (session IDs) gibi hassas verileri sızdırmak (exfiltrate) için kullanabilirler. Bu güvenlik zafiyeti genellikle kullanıcı tercihlerinin (örneğin; özel temalar, yazı tipi renkleri) `<style>` blokları veya satır içi (inline) `style` öznitelikleri içinde yansıtıldığı uç noktalarda (endpoints) meydana gelir. Bir saldırganın perspektifinden bakıldığında, başarılı bir istismar; kullanıcı arayüzü manipülasyonuna (UI redressing), yetkisiz düzen modifikasyonu yoluyla oltalama (phishing) saldırılarına veya katı İçerik Güvenlik Politikalarının (Content Security Policy - CSP) JavaScript tabanlı saldırıları engelleyebileceği ortamlarda gizli veri sızıntısına yol açabilir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CLNT-05 |
| **CWE** | CWE-74 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek* |

> *Enjeksiyon noktası CSRF token'ları gibi hassas özniteliklerin sızdırılmasına izin veriyorsa veya hassas iş akışlarında UI redressing için kullanılabiliyorsa önem derecesi Yüksek olur.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/05-Testing_for_CSS_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite`, `CSS-Injection-Payload-Generator`, `CyberChef`, `Postman`

### Uygulama, kullanıcı kontrollü girdileri CSS bağlamları içinde yansıtıyor mu?
- [ ] Hayır — girdi stil etiketlerinde, özniteliklerde veya CSS dosyalarında yansıtılmıyor  
- [ ] Evet — girdi yansıtılıyor ancak kesinlikle güvenli alfanümerik değerlerle sınırlı  
- [ ] Evet — girdi bir `style` özniteliği veya `<style>` bloğu içinde yansıtılıyor  

### CSS'e özgü meta karakterler ve fonksiyonlar uygun şekilde sanitize ediliyor mu?
- [ ] Evet — `{`, `}`, `:` gibi karakterler ve `url()` gibi fonksiyonlar uygun şekilde kaçırılıyor (properly escaped)  
- [ ] Evet — sanitizasyon mevcut ancak kodlama (encoding) veya CSS yorumları (comments) yoluyla atlatılması (bypass) mümkün  
- [ ] Hayır — CSS meta karakterlerine hiçbir sanitizasyon uygulanmıyor  

### CSS seçicileri (selectors) kullanılarak veri sızıntısı mümkün mü?
- [ ] Hayır — seçiciler harici istekleri tetikleyemez veya veri sızdıramaz  
- [ ] Evet — öznitelik seçicileri ve `background-image` aracılığıyla kısmi veri sızıntısı mümkün  
- [ ] Evet — otomatik Brute Force teknikleri kullanılarak hassas token'ların tam sızıntısı mümkün  

### Bir İçerik Güvenlik Politikası (CSP), CSS enjeksiyonu riskini azaltıyor mu?
- [ ] Evet — `style-src` 'self' ile sınırlandırılmış ve `img-src` veya `connect-src` kısıtlanmış  
- [ ] Evet — CSP mevcut ancak 'unsafe-inline' kullanıyor veya joker karakterli (wildcard) harici alan adlarına izin veriyor  
- [ ] Hayır — CSS aracılığıyla harici kaynak yüklenmesini önlemek için herhangi bir CSP uygulanmamış  

### Güvenlik zafiyeti UI redressing veya phishing gerçekleştirmek için kullanılabilir mi?
- [ ] Hayır — enjeksiyon kapsamı düzeni (layout) önemli ölçüde değiştirmek için çok sınırlı  
- [ ] Evet — kullanıcı arayüzü modifikasyonu mümkün, bu da kötü niyetli öğelerin üst üste bindirilmesine (overlay) olanak tanıyor  
- [ ] Evet — tam sayfa düzeni kontrolü mümkün, bu da oldukça ikna edici oltalama (phishing) saldırılarını kolaylaştırıyor

---

## WSTG-CLNT-06 — İstemci Tarafı Kaynak Manipülasyonu Testi (Client-Side Resource Manipulation)

İstemci tarafı kaynak manipülasyonu (Client-side resource manipulation), bir uygulama kullanıcı tarafından kontrol edilen girdinin; betikler (scripts), stil sayfaları (stylesheets) veya iframe'ler gibi tarayıcı tarafından yüklenen kaynakların kaynağını veya yolunu etkilemesine izin verdiğinde meydana gelir. Bir saldırgan; URI fragmanlarını, sorgu parametrelerini veya diğer DOM kaynaklarını manipüle ederek uygulamayı kötü amaçlı harici içerik yüklemeye veya yetkisiz kod çalıştırmaya zorlayabilir. Bu zafiyet genellikle, HTML öğelerinin `src` veya `href` özniteliklerini (attributes) güvenilir bir izin listesine (allow-list) karşı yeterli doğrulama yapmadan dinamik olarak dolduran JavaScript mantığında bulunur. Bir saldırgan açısından bakıldığında, bu durum kaynak yüklemesini saldırgan kontrolündeki bir sunucuya yönlendiren bir URL oluşturularak istismar edilir; bu da potansiyel olarak DOM tabanlı Cross-Site Scripting (XSS), hassas veri sızıntısı (exfiltration) veya kullanıcı arayüzü manipülasyonu (UI redressing) ile sonuçlanabilir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CLNT-06 |
| **CWE** | CWE-99 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/06-Testing_for_Client-side_Resource_Manipulation  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  

**Araçlar:** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `grep`, `Snyk`, `LinkFinder`

### Uygulama, kaynakları dinamik olarak yüklemek veya gömmek için istemci tarafı alıcılar (sinks) kullanıyor mu?
- [ ] Hayır — kaynaklar yalnızca statik HTML veya sunucu tarafı mantığı aracılığıyla yükleniyor  
- [ ] Evet — istemci tarafı JavaScript; `src`, `href` veya `data` gibi kaynak özniteliklerini dinamik olarak dolduruyor  

### Kaynak URI kaynakları için doğrulama kontrolleri veya izin listeleri (allow-lists) uygulanmış mı?
- [ ] Evet — tüm dinamik kaynaklara katı izin listeleri ve kaynak (origin) doğrulaması **uygulanmaktadır**  
- [ ] Evet — doğrulama mevcuttur ancak zayıf kara listelere (blacklists) veya kolayca atlatılabilen düzenli ifadelere (regex) dayanmaktadır  
- [ ] Hayır — kullanıcı kontrollü kaynak yollarına hiçbir doğrulama kontrolü uygulanmamaktadır  

### Alternatif protokoller veya kodlama (encoding) kullanarak URI doğrulamasını atlatmak mümkün mü?
- [ ] Hayır — kaynak filtrelerinin atlatılması **mümkün değildir**  
- [ ] Evet — `data:`, `vbscript:` veya `javascript:` gibi farklı protokoller kullanılarak atlatma **mümkündür**  
- [ ] Evet — basit dize eşleşmelerini atlatmak için kodlanmış karakterler veya null baytlar kullanılarak atlatma **mümkündür**  

### Bir saldırgan, kaynak yüklemesini harici ve güvenilmeyen bir alan adına yönlendirebilir mi?
- [ ] Hayır — kaynak yolları aynı kaynak (same origin) veya sabit bir alt dizinle sınırlandırılmıştır  
- [ ] Evet — uygulama, rastgele bir harici alan adından betikler veya stiller yüklemeye **zorlanabilir**  

### Kaynak manipülasyonunun maksimum etkisi nedir?
- [ ] Düşük — manipülasyon yalnızca görseller veya hassas olmayan metinler gibi çalıştırılamaz öğeleri etkiler  
- [ ] Orta — manipülasyon CSS Injection'a veya önemli ölçüde kullanıcı arayüzü manipülasyonuna (UI redressing) olanak tanır  
- [ ] Yüksek — manipülasyon DOM tabanlı XSS'e veya rastgele JavaScript çalıştırılmasına yol açar *(Kritik)*

---

## WSTG-CLNT-07 — Kaynaklar Arası Kaynak Paylaşımı (CORS)

Kaynaklar Arası Kaynak Paylaşımı (CORS), bir sunucunun farklı kökenlerden (origins) kendi kaynaklarına erişime açıkça izin vermesini sağlayan ve Aynı Köken Politikasını (SOP) esneten tarayıcı tabanlı bir mekanizmadır. Yanlış yapılandırmalar, bir uygulamanın rastgele kökenlere aşırı güvenmesi durumunda ortaya çıkar; bu durum genellikle `Access-Control-Allow-Origin` yanıt başlığında `Origin` başlığının yansıtılması (reflecting) veya kötü yapılandırılmış regex beyaz listelerinin (whitelists) kullanılmasıyla gerçekleşir. Saldırgan perspektifinden bakıldığında, gevşek bir CORS politikası, kontrol edilen bir alan adında (domain) barındırılan ve zafiyetli uygulamaya kimlik doğrulaması yapılmış (authenticated) istekler gönderen kötü niyetli bir betik (script) aracılığıyla istismar edilebilir. Bu durum, saldırganın CSRF tokenları veya Kişisel Veriler (PII) gibi hassas verileri doğrudan yanıt gövdesinden (response body) sızdırmasına (exfiltrate) olanak tanır. Bu zafiyet, kimlik doğrulaması yapılmış oturumları işleyen ve halka açık olmayan bilgileri döndüren API uç noktalarında (endpoints) en kritik seviyededir.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CLNT-07 |
| **CWE** | CWE-942 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek |

**Kaynaklar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/07-Testing_Cross_Origin_Resource_Sharing  
* https://hacktricks.wiki/en/pentesting-web/cors-bypass.html  
* https://portswigger.net/web-security/cors  

**Araçlar:** `Burp Suite (CORS Raider)`, `curl`, `Postman`, `Corsy`

### Uygulama, kimlik doğrulaması yapılmış uç noktalarında CORS başlıklarını uyguluyor mu?
- [ ] Hayır — CORS **etkin değil**; tarayıcı varsayılan olarak katı Aynı Köken Politikasına (SOP) döner  
- [ ] Evet — CORS başlıkları mevcut ve katı bir **beyaz liste** (whitelist) ile sınırlandırılmış  
- [ ] Evet — CORS başlıkları mevcut ancak **gevşek (permissive)** yapılandırılmış  

### Sunucu `Access-Control-Allow-Origin` (ACAO) başlığını nasıl işliyor?
- [ ] ACAO belirli, **güvenilir** ve statik bir alan adına ayarlanmış  
- [ ] ACAO `*` (wildcard) olarak ayarlanmış ve kimlik bilgileri (credentials) **gönderilemez**  
- [ ] ACAO `*` (wildcard) olarak ayarlanmış ve kimlik bilgileri **gönderilebilir** *(Not: Çoğu tarayıcı bunu engeller)*  
- [ ] ACAO, istekteki `Origin` başlığının değerini **dinamik olarak yansıtıyor** (reflect)  

### `Access-Control-Allow-Credentials` (ACAC) başlığı `true` olarak ayarlanmış mı?
- [ ] Hayır — ACAC **mevcut değil** veya `false` olarak ayarlanmış  
- [ ] Evet — ACAC `true` olarak ayarlanmış, ancak ACAO güvenilir kökenlerle **sınırlandırılmış**  
- [ ] Evet — ACAC `true` olarak ayarlanmış ve ACAO rastgele kökenleri **yansıtıyor** *(Kritik)*  

### Köken beyaz listesi, alt alan adı (subdomain) veya null origin manipülasyonu ile atlatılabilir mi?
- [ ] Hayır — beyaz liste katı bir şekilde uygulanıyor ve **atılatılamaz**  
- [ ] Evet — beyaz liste hedefin **herhangi bir** alt alan adını kabul ediyor (örneğin, `attacker.target.com`)  
- [ ] Evet — beyaz liste sandbox uygulanmış iframe'ler aracılığıyla `null` origin değerini kabul ediyor  
- [ ] Evet — beyaz liste regex atlatma (bypass) yöntemlerine karşı savunmasız (örneğin, `target.com.attacker.com` veya `target.com-safe.com`)  

### CORS yapılandırması hassas veri sızıntısına (exfiltration) izin veriyor mu?
- [ ] Hayır — dışa açık uç noktalar hassas veya oturuma özel veriler **içermiyor**  
- [ ] Evet — kimlik doğrulaması yapılmış uç noktalar, yetkisiz bir köken tarafından **okunabilen** PII, kimlik bilgileri veya CSRF tokenları döndürüyor

---

## WSTG-CLNT-08 — Cross-Site Flashing Testi

Cross-Site Flashing (XSF), bir Flash (SWF) dosyasının kullanıcı tarafından kontrol edilen girdileri uygun olmayan şekilde işlemesi sonucunda, saldırganın kötü amaçlı ActionScript kodu yürütmesine veya tarayıcının JavaScript ortamına köprü kurmasına olanak tanıyan istemci tarafı (client-side) bir zafiyettir. `ExternalInterface.call`, `getURL` veya `loadMovie` gibi sink noktalarına aktarılan `FlashVars` veya URL sorgu dizeleri (query strings) gibi parametrelerin manipüle edilmesiyle, bir saldırgan oturum ele geçirme (session hijacking) ve veri sızdırma (data exfiltration) dahil olmak üzere savunmasız alan adı (domain) bağlamında eylemler gerçekleştirebilir. Bu zafiyet, temel olarak hâlâ SWF dosyalarını barındıran eski kurumsal uygulamalarda bulunur; burada yetersiz girdi doğrulaması, Flash dosyasının Cross-Site Scripting (XSS) için bir vektör olarak yeniden kullanılmasına veya izin veren çapraz etki alanı (cross-domain) yapılandırmaları aracılığıyla Aynı Köken Politikası'nın (Same-Origin Policy - SOP) atlatılmasına izin verir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CLNT-08 |
| **CWE** | CWE-79 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek |

**Kaynaklar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/08-Testing_for_Cross_Site_Flashing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `JPEXS Free Flash Decompiler`, `FFDec`, `Burp Suite`, `Google Dorks`, `strings`

### Uygulamada eski Adobe Flash (.swf) dosyaları barındırılıyor mu?
- [ ] Hayır — uygulama kapsamında hiçbir Flash dosyası barındırılmıyor veya referans verilmiyor  
- [ ] Evet — SWF dosyaları mevcut ancak yalnızca statik içerik sağlıyor  
- [ ] Evet — SWF dosyaları mevcut ve `FlashVars` veya URL dizeleri aracılığıyla dinamik parametreleri kabul ediyor  

### ActionScript sink noktalarına aktarılan girdiler temizleniyor mu?
- [ ] Evet — tüm girdiler sıkı bir şekilde doğrulanıyor ve ActionScript sink noktaları manipüle edilemiyor  
- [ ] Evet — doğrulama uygulanıyor ancak belirli kodlama teknikleri ile atlatma (bypass) mümkün  
- [ ] Hayır — girdi doğrudan `ExternalInterface.call` veya `getURL` gibi hassas sink noktalarına aktarılıyor *(Kritik)*  

### Flash dosyası, rastgele JavaScript (XSS) yürütmek için kullanılabilir mi?
- [ ] Hayır — `ExternalInterface` devre dışı bırakılmış veya `allowScriptAccess` parametresi `never` olarak ayarlanmış  
- [ ] Evet — `allowScriptAccess` parametresi `sameDomain` olarak ayarlanmış ancak SWF hedef alan adında barındırılıyor  
- [ ] Evet — `allowScriptAccess` parametresi `always` olarak ayarlanmış ve herhangi bir etki alanından XSS yapılmasına izin veriyor  

### `crossdomain.xml` politikası, yetkisiz çapraz köken (cross-origin) isteklerini engelliyor mu?
- [ ] Evet — `crossdomain.xml` kısıtlayıcıdır ve yalnızca güvenilen, belirli kökenlere izin verir  
- [ ] Hayır — `crossdomain.xml` dosyası eksik  
- [ ] Evet — politika aşırı derecede izin vericidir (örneğin, `<allow-access-from domain="*" />`)  

### SWF dosyası, saldırgan kontrollü harici dosyaları yüklemeye zorlanabilir mi?
- [ ] Hayır — uygulama, harici SWF dosyalarını yüklemeye zorlanamaz  
- [ ] Evet — `loadMovie` veya `loadMovieNum` fonksiyonları doğrulanmamış harici URL'leri kabul ediyor

---

## WSTG-CLNT-09 — Clickjacking Testi

Clickjacking (Kullanıcı Arayüzü Düzenleme veya UI Redressing olarak da bilinir), bir saldırganın, kullanıcının üst seviye bir sayfaya tıkladığını sanmasını sağlayarak, şeffaf veya opak katmanlar aracılığıyla başka bir sayfadaki bir butona veya bağlantıya tıklamaya zorlaması durumunda ortaya çıkar. Bu güvenlik açığı, kullanıcı eylemlerinin bütünlüğünü bozarak, mağdur adına yetkisiz veri değişikliğine, hesap değişikliklerine veya finansal transferlere yol açabilir. Saldırganlar genellikle hedef web sitesini, kontrol ettikleri kötü niyetli bir sitedeki bir `<iframe>` içine yerleştirerek ve üzerini aldatıcı içeriklerle kapatarak bu açıktan faydalanırlar. Bu durum en sık "Hesabı Sil" butonları, parola sıfırlama formları veya yönetici yapılandırma panelleri gibi hassas durum değiştiren eylemlerin gerçekleştirildiği sayfalarda gözlemlenir.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CLNT-09 |
| **CWE** | CWE-1021 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek* |

> *Hassas eylemler (örneğin; para transferleri, hesap silme veya yetki yükseltme) Clickjacking yoluyla tetiklenebiliyorsa önem derecesi Yüksek olur.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/09-Testing_for_Clickjacking  
* https://hacktricks.wiki/en/pentesting-web/clickjacking.html  
* https://portswigger.net/web-security/clickjacking  

**Araçlar:** `Burp Suite Clickbandit`, `Browser Developer Tools`, `OWASP ZAP`, `Online Clickjack Test`

### Uygulama, yetkisiz çerçevelemeyi (framing) önlemek için sunucu tarafı başlıkları (headers) uyguluyor mu?
- [ ] Evet — `X-Frame-Options` veya `Content-Security-Policy` **uygulanmıştır** ve çerçevelemeyi engeller *(En güvenli)*  
- [ ] Evet — başlıklar mevcuttur ancak **yanlış yapılandırılmıştır** (örneğin, geniş tarayıcı desteği bulunmayan `X-Frame-Options: ALLOW-FROM`)  
- [ ] Hayır — herhangi bir çerçeveleme koruma başlığı **uygulanmamıştır**  

### `Content-Security-Policy` (CSP) `frame-ancestors` direktifi uygulanmış mı?
- [ ] Evet — `frame-ancestors 'none'` veya `'self'` **uygulanmıştır**  
- [ ] Evet — `frame-ancestors` mevcuttur ancak güvenilmeyen veya aşırı geniş kaynaklara (origins) **izin vermektedir**  
- [ ] Hayır — direktif **eksiktir** veya CSP uygulanmamıştır  

### `X-Frame-Options` (XFO) başlığı, eski tarayıcı desteği için doğru yapılandırılmış mı?
- [ ] Evet — `DENY` veya `SAMEORIGIN` **uygulanmıştır**  
- [ ] Evet — XFO mevcuttur ancak bir CSP `frame-ancestors` direktifi bulunduğu için modern tarayıcılar tarafından **yok sayılmaktadır**  
- [ ] Hayır — XFO başlığı **eksiktir** veya güvensiz bir değere ayarlanmıştır  

### Çerçeveleme (framing) korumaları bilinen tekniklerle atlatılabilir mi (bypass)?
- [ ] Hayır — standart tekniklerle (double-framing vb.) atlatma **mümkün değildir**  
- [ ] Evet — `frame-ancestors` beyaz listesindeki (white-list) zayıf düzenli ifade (regex) veya mantık hatası nedeniyle koruma **atlatılabilir**  
- [ ] Evet — uygulama yalnızca JavaScript tabanlı çerçeve kırıcıya (frame-busting) güvendiği için koruma **atlatılabilir**  

### Hassas bir eylem, bir Clickjacking kavram kanıtı (proof-of-concept) aracılığıyla istismar edilebilir mi?
- [ ] Hayır — çerçeveleme engellenmiştir veya hassas eylemlere ulaşılamamaktadır  
- [ ] Evet — kullanıcı arayüzü düzenleme (UI redressing) **mümkündür** ancak karmaşık kullanıcı etkileşimi gerektirir  
- [ ] Evet — kullanıcı arayüzü düzenleme (UI redressing) **mümkündür** ve anında durum değiştiren eylemlere izin verir *(Kritik)*

---

## WSTG-CLNT-10 — WebSocket Testi

WebSocket testi, geleneksel HTTP istek-yanıt modellerini devre dışı bırakan, istemci ile sunucu arasında kurulan tam çift yönlü (full-duplex) iletişim kanalına odaklanır. Sızma testi uzmanları (Pentesters); el sıkışma (Handshake) sürecinin güvenliğini, Cross-Site WebSocket Hijacking (CSWSH) korumalarının mevcudiyetini ve mesaj yüklerine (Payload) uygulanan girdi doğrulamanın (Input Validation) titizliğini değerlendirir. İstismar (Exploitation) süreci tipik olarak, verileri gerçek zamanlı olarak sızdırmak (exfiltrate) için aktif bir oturumun ele geçirilmesini veya kalıcı soket üzerinden Komut Enjeksiyonu (Command Injection) veya SQLi gibi sunucu tarafı zafiyetlerini tetikleyen kötü niyetli yüklerin (Payload) enjekte edilmesini içerir. Birçok geleneksel Web Uygulaması Güvenlik Duvarı (WAF), WebSocket trafiğini etkili bir şekilde denetlemediğinden, bu protokol genellikle çevre savunmalarını atlatmak (Bypass) için gizli bir vektör sağlar.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CLNT-10 |
| **CWE** | CWE-1385 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Orta |

**Kaynaklar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/10-Testing_WebSockets  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/websockets  

**Araçlar:** `Burp Suite (WebSockets tab)`, `OWASP ZAP`, `wscat`, `Socket.io-client`, `Wireshark`

### WebSocket bağlantısı güvenli bir kanal üzerinden mi kuruluyor?
- [ ] Evet — Tüm bağlantılar için `wss://` (WebSocket Secure) kullanımı zorunlu kılınmıştır.
- [ ] Hayır — `ws://` kullanılıyor; bu durum aradaki adam (person-in-the-middle - PITM) dinlemesine olanak tanır.

### Uygulama, Cross-Site WebSocket Hijacking (CSWSH) saldırılarına karşı korunuyor mu?
- [ ] Hayır — Sunucu, el sıkışma (Handshake) sırasında `Origin` başlığını (Header) sıkı bir şekilde doğrular.
- [ ] Evet — `Origin` başlığı doğrulaması zayıf veya null/sahte origin'ler aracılığıyla atlatılabilir (Bypass).
- [ ] Evet — `Origin` başlığı doğrulaması yapılmıyor; bu durum siteler arası saldırganların bağlantı başlatmasına olanak tanıyor *(Kritik)*.

### WebSocket mesaj yüklerine (Payload) girdi doğrulaması (Input Validation) ve temizlemesi (Sanitization) uygulanıyor mu?
- [ ] Evet — Gelen tüm mesajlar temizlenir ve bir şemaya karşı doğrulanır.
- [ ] Evet — Bazı doğrulamalar mevcut ancak kodlanmış veya standart dışı yükler (Payload) kullanılarak atlatılması (Bypass) mümkündür.
- [ ] Hayır — Mesajlar doğrudan işlenir; bu durum enjeksiyona (XSS, SQLi, RCE) veya mantık manipülasyonuna izin verir.

### Her WebSocket mesajında kimlik doğrulama (Authentication) ve yetkilendirme (Authorization) kontrolleri yapılıyor mu?
- [ ] Evet — Her mesaj için oturum doğrulanır veya protokol token'lar ile durumsuz (Stateless) çalışır.
- [ ] Evet — Kimlik doğrulama el sıkışma (Handshake) sırasında gerçekleşir ancak belirli eylemler için yetkilendirme (Authorization) mesaj başına zorunlu kılınmaz.
- [ ] Hayır — Kimlik doğrulama yalnızca el sıkışma (Handshake) sırasında kontrol edilir; bu durum oturumun ele geçirilmesine (Session Hijacking) ve tam erişim sağlanmasına olanak tanır.

### Sunucu, WebSocket üzerinde hız sınırlama (Rate Limiting) veya kaynak kısıtlamaları uyguluyor mu?
- [ ] Evet — Hız sınırlama (Rate Limiting) ve maksimum mesaj boyutları etkinleştirilmiş ve zorunlu kılınmıştır.
- [ ] Hayır — Hiçbir sınır mevcut değil; mesaj yağmuru (flooding) veya büyük mesaj boyutları aracılığıyla Hizmet Dışı Bırakma (DoS) saldırılarına olanak tanır.

---

## WSTG-CLNT-11 — Web Mesajlaşma Testi (Test Web Messaging)

Web Mesajlaşma veya `postMessage`, bir üst sayfa ile açılan bir pencere (spawned popup) veya gömülü bir iframe gibi pencere nesneleri (window objects) arasında kökenler arası iletişime (cross-origin communication) olanak tanır. Bu mekanizma, modern web işlevselliği için kritiktir; ancak alıcı uygulama gönderenin kimliğini doğrulamada başarısız olursa veya mesaj yükünü (payload) hatalı işlerse önemli riskler doğurur. Saldırganlar, hedef uygulamayı gömen kötü niyetli bir site barındırarak ve istenmeyen eylemleri tetiklemek, hassas verileri sızdırmak (exfiltrate) veya DOM tabanlı Siteler Arası Betik Çalıştırma (Cross-Site Scripting - XSS) saldırılarını gerçekleştirmek için özel yapılandırılmış mesajlar (crafted messages) göndererek bu güvenlik açıklarını istismar ederler. Başarılı bir istismar, mesaj dinleyicileri (message listeners) `origin` özelliğini sıkı bir şekilde doğrulamadığında veya `message.data` içeriğini doğrudan tehlikeli yürütme havuzlarına (execution sinks) aktardığında gerçekleşir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CLNT-11 |
| **CWE** | CWE-345 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek |

**Kaynaklar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/11-Testing_Web_Messaging  
* https://hacktricks.wiki/en/pentesting-web/postmessage-vulnerabilities/index.html  

**Araçlar:** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `PostMessage-Tracker`, `PMHook`

### Uygulama, dokümanlar arası iletişim için `postMessage` API'sini kullanıyor mu?
- [ ] Hayır — İstemci tarafı kodunda `postMessage` dinleyicileri mevcut **değildir**  
- [ ] Evet — `postMessage`, dahili bileşen iletişimi için kullanılmaktadır  
- [ ] Evet — `postMessage`, üçüncü taraf alan adları veya widget'larla iletişim kurmak için kullanılmaktadır  

### Gelen mesajların kaynağı (origin) sıkı bir şekilde doğrulanıyor mu?
- [ ] Evet — Kaynak, `===` veya tam eşleşme karşılaştırması kullanılarak sıkı bir beyaz listeye (whitelist) göre kontrol edilmektedir *(En güvenli yöntem)*  
- [ ] Evet — Kaynak, bir regex (düzenli ifade) kullanılarak doğrulanmaktadır ancak mantık atlatılmaya (bypass) müsaittir (örneğin, `^https://trusted.com`)  
- [ ] Hayır — `origin` özelliği kontrol edilmemekte ve herhangi bir alan adının mesaj göndermesine izin verilmektedir  
- [ ] Hayır — Gönderenin `targetOrigin` kısmında bir joker karakter `*` kullanılmaktadır ve bu durum verilerin kötü niyetli dinleyicilere sızmasına neden olabilir  

### Mesaj yükü (payload), uygulama tarafından işlenmeden önce temizleniyor (sanitized) mu?
- [ ] Evet — Veriler düz metin (plain text) olarak ele alınmaktadır ve hiçbir tehlikeli havuz (sink) kullanılmamaktadır  
- [ ] Evet — Veriler kullanılmadan önce ayrıştırılmakta (örneğin, `JSON.parse`) ve doğrulanmaktadır; atlatma (bypass) **mümkün değildir**  
- [ ] Hayır — Mesaj verileri doğrudan `innerHTML`, `eval()` veya `setTimeout()` gibi tehlikeli havuzlara aktarılmaktadır  
- [ ] Hayır — Mesaj verileri, doğrulama yapılmadan pencereyi yönlendirmek veya `location.href` değerini değiştirmek için kullanılmaktadır  

### Bir saldırgan, kötü niyetli bir mesaj yoluyla yetkisiz eylemleri tetikleyebilir mi veya veri sızdırabilir mi?
- [ ] Hayır — Sahte bir mesajla bile hassas eylemlere veya verilere erişilememektedir  
- [ ] Evet — Özel yapılandırılmış bir mesaj, hassas durum değişikliklerini tetikleyebilir (örneğin, parola değiştirme, profil güncelleme)  
- [ ] Evet — Özel yapılandırılmış bir mesaj, DOM tabanlı XSS ile sonuçlanabilir veya CSRF token'larının/oturum verilerinin sızdırılmasına yol açabilir

---

## WSTG-CLNT-12 — Tarayıcı Depolama Alanının Test Edilmesi

Tarayıcı depolamasını test etmek, bir uygulamanın verileri kalıcı hale getirmek için LocalStorage, SessionStorage, IndexedDB ve WebSQL gibi istemci tarafı mekanizmalarını nasıl kullandığının analiz edilmesini içerir. Güvensiz şekilde depolanan hassas bilgiler — Kişisel Veriler (Personally Identifiable Information (PII)), kimlik doğrulama belirteçleri (tokens) veya oturum tanımlayıcıları dahil — bir saldırganın Siteler Arası Betik Çalıştırma (Cross-Site Scripting (XSS)) gerçekleştirmesi veya kullanıcının cihazına fiziksel erişim sağlaması durumunda ele geçirilebilir. Sızma testi uzmanları (Pentesters), verilerin açık metin (cleartext) olarak saklanıp saklanmadığını, çıkış yapıldıktan sonra erişilebilir kalıp kalmadığını ve veri sızıntısını önlemek için depolama yaşam döngüsünün uygun şekilde yönetilip yönetilmediğini belirlemelidir. Bir saldırganın bakış açısından bu depolama mekanizmaları, oturum çalma (session hijacking) veya uzun süreli hesap kalıcılığı sağlayan durum koruma bilgilerini sızdırmak (exfiltrating) için kazançlı hedeflerdir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CLNT-12 |
| **CWE** | CWE-922 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek* |

> *Oturum belirteçleri (tokens) veya hassas PII verilerinin LocalStorage'da saklanması ve XSS yoluyla erişilebilir olması durumunda Önem Derecesi Yüksek (High) olarak kabul edilir.

**Kaynaklar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/12-Testing_Browser_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Browser DevTools`, `Storage Explorer`, `Burp Suite`, `OWASP ZAP`

### Uygulama, tarayıcı depolama alanında hassas bilgi saklıyor mu?
- [ ] Hayır — uygulama tarayıcı depolamasını kullanmıyor veya yalnızca hassas olmayan kullanıcı arayüzü (UI) durumlarını saklıyor  
- [ ] Evet — hassas veriler saklanıyor ancak güçlü istemci tarafı kriptografi kullanılarak şifreleniyor  
- [ ] Evet — hassas veriler (tokens, PII, sırlar) açık metin (cleartext) olarak saklanıyor *(Orta)*  

### Depolanan verilere yetkisiz betikler tarafından erişilebilir mi (XSS riski)?
- [ ] Hayır — veriler HttpOnly çerezlerinde saklanıyor (tarayıcı depolamasında değil) veya depolama alanı kullanılmıyor  
- [ ] Evet — LocalStorage/SessionStorage kullanılıyor, bu da verileri herhangi bir XSS Payload aracılığıyla tamamen erişilebilir kılıyor *(Yüksek)*  

### Kullanıcı çıkışı yapıldığında tarayıcı depolama alanı uygun şekilde temizleniyor mu?
- [ ] Evet — uygulamayla ilgili tüm depolama alanları çıkış işlemi sırasında açıkça temizleniyor  
- [ ] Hayır — depolama alanı çıkıştan sonra da kalıyor ancak hassas oturum verileri içermiyor  
- [ ] Hayır — kimlik doğrulama belirteçleri veya PII, oturum sonlandırıldıktan sonra depolama alanında kalmaya devam ediyor *(Orta)*  

### Uygulama, büyük/hassas veri setleri için IndexedDB veya WebSQL kullanıyor mu?
- [ ] Hayır — bu depolama mekanizmaları kullanılmıyor  
- [ ] Evet — veriler saklanıyor ve DOM içinde işlenmeden önce uygun şekilde sanitize ediliyor  
- [ ] Evet — hassas veri setleri IndexedDB/WebSQL yapılarında açık metin (cleartext) olarak saklanıyor  

### Depolanan verilerin bütünlüğü, uygulama mantığını etkileyecek şekilde manipüle edilebilir mi?
- [ ] Hayır — uygulama, depolama alanından gelen verileri işlemeden önce doğrular veya imzalar  
- [ ] Evet — depolama değerlerini (örneğin kullanıcı rolleri, bayraklar) değiştirmek mümkündür ve uygulama davranışının değişmesine neden olur

---

## WSTG-CLNT-13 — Siteler Arası Betik Dahil Etme (Cross-Site Script Inclusion) Testi

Siteler Arası Betik Dahil Etme (Cross-Site Script Inclusion - XSSI), bir web uygulamasının hassas verileri dinamik olarak oluşturulmuş bir JavaScript dosyası veya JSONP yanıtı içinde dışa aktarması ve bu dosyanın saldırgan kontrolündeki bir site tarafından bir `<script>` etiketi aracılığıyla dahil edilmesi durumunda meydana gelir. Betik dahil etme işlemi Aynı Köken Politikası'nı (Same-Origin Policy - SOP) atlattığı için, bir saldırgan betiği kendi bağlamında çalıştırabilir ve hassas bilgileri sızdırmak amacıyla global nesneleri veya değişkenleri geçersiz kılabilir (override). Bu güvenlik zafiyeti genellikle e-posta adresleri, API anahtarları veya oturum meta verileri gibi kullanıcıya özel verileri betik formatında döndüren kimlik doğrulaması yapılmış uç noktalarda (endpoints) kendini gösterir. Bir saldırganın perspektifinden bakıldığında, istismar süreci; zafiyet barındıran betiğe atıfta bulunan ve sonuçta ortaya çıkan global değişken değişikliklerini veya fonksiyon çağrılarını yakalayan kötü niyetli bir sayfa hazırlanmasını içerir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CLNT-13 |
| **CWE** | CWE-200 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek |

> *Sızan veriler CSRF token'ları, oturum tanımlayıcıları veya yüksek derecede hassas PII (Kişisel Veri) içeriyorsa önem derecesi Yüksek olur.

**Kaynaklar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/13-Testing_for_Cross_Site_Script_Inclusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite`, `JSParser`, `LinkFinder`, `Browser Developer Tools`

### Kimlik doğrulaması yapılmış herhangi bir uç nokta, hassas bilgiler içeren dinamik JavaScript veya JSONP döndürüyor mu?
- [ ] Hayır — tüm betikler statiktir ve kullanıcıya özel veriler **içeremez**  
- [ ] Evet — dinamik betikler mevcuttur ancak hassas bilgi **içermemektedir**  
- [ ] Evet — dinamik betikler PII veya token'lar içerir ve kökenler arası (cross-origin) **erişilebilirdir**  

### Hassas betik yanıtlarında `X-Content-Type-Options: nosniff` başlığı uygulanmış mı?
- [ ] Evet — başlık **uygulanmıştır** ve MIME türü koklamayı (MIME-type sniffing) engeller  
- [ ] Hayır — başlık **eksiktir**, bu da potansiyel olarak betik olmayan içeriğin betik olarak çalıştırılmasına izin verebilir  

### Hassas betikler, çalıştırılamayan ön ekler (non-executable prefixes) veya özel başlıklar gibi anti-XSSI mekanizmalarıyla korunuyor mu?
- [ ] Evet — betikler, standart bir `<script>` etiketi aracılığıyla **gönderilemeyen** özel bir başlık veya token gerektirir  
- [ ] Evet — betikler, kolayca **atılatılamayan** çalıştırılamayan ön ekler (örneğin `)]}'`) kullanır  
- [ ] Hayır — betiklere, ortam kimlik bilgileri (ambient credentials) kullanılarak GET istekleri aracılığıyla doğrudan erişilebilir *(Kritik)*  

### Global değişkenleri veya prototipleri (prototypes) geçersiz kılarak hassas verilerin sızdırılması mümkün mü?
- [ ] Hayır — hassas veriler yerel kapsamdadır (scoped locally) veya modern tarayıcı özellikleri aracılığıyla korunmaktadır  
- [ ] Evet — hassas veriler global değişkenlere atanmıştır ve **yakalanabilir**  
- [ ] Evet — veriler, **kesilebilen** (intercept) JSONP geri çağırmaları (callbacks) aracılığıyla sızdırılmaktadır

---

## WSTG-CLNT-14 — Testing for Reverse Tabnabbing

Reverse Tabnabbing, `target="_blank"` kullanılarak açılan bir sayfanın, `window.opener` nesnesi aracılığıyla üst sayfaya (parent page) ait işlevsel bir referansı koruduğu istemci taraflı bir zafiyettir. Bu durum, saldırganın kontrolündeki bir hedef sitenin, orijinal ve güvenilir sekmeyi programatik olarak genellikle kimlik bilgilerini ele geçirmek (credential harvesting) için tasarlanmış bir Kimlik Avı (Phishing) sayfası olan zararlı bir URL'ye yönlendirmesine olanak tanır. Saldırı, kullanıcının orijinal sekmeye duyduğu güveni istismar eder; zira kullanıcılar, az önce ayrıldıkları sayfanın farklı bir alan adına (domain) yönlendiğini genellikle fark etmezler. Modern güvenlik uygulamaları, bu pencereler arası iletişimi önlemek için çapa (anchor) etiketlerinde `rel="noopener"` veya `rel="noreferrer"` özniteliklerinin kullanılmasını ya da JavaScript'te `opener` özelliğinin null olarak ayarlanmasını gerektirir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CLNT-14 |
| **CWE** | CWE-1022 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Düşük / Orta* |

> *Modern tarayıcıların çoğu artık `target="_blank"` kullanımını varsayılan olarak `rel="noopener"` şeklinde işlediğinden, önem derecesi çoğu durumda Düşüktür. Uygulama eski nesil tarayıcıları hedefliyorsa veya `opener` özelliğini null olarak ayarlamadan `window.open()` kullanıyorsa önem derecesi Orta olur.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/14-Testing_for_Reverse_Tabnabbing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Browser DevTools (Inspector)`, `Burp Suite (Repeater)`, `ZAP`

### Yeni bir pencerede veya sekmede açılan bağlantılar mevcut mu?
- [ ] Hayır — `target="_blank"` veya eşdeğer JavaScript tabanlı yönlendirme kullanan bağlantı bulunmamaktadır.
- [ ] Evet — `target="_blank"` yalnızca dahili ve güvenilir bağlantılarda kullanılmaktadır.
- [ ] Evet — `target="_blank"` harici bağlantılarda veya kullanıcı tarafından sağlanan URL'lerde kullanılmaktadır.

### Çapa (anchor) etiketlerinde `window.opener` ilişkisi kısıtlanmış mı?
- [ ] Evet — `rel="noopener"` veya `rel="noreferrer"`, `target="_blank"` içeren tüm bağlantılara **tutarlı bir şekilde uygulanmıştır** *(En güvenli)*.
- [ ] Evet — öznitelikler uygulanmıştır ancak yüksek riskli veya kullanıcı tarafından oluşturulan bağlantılarda **eksiktir**.
- [ ] No — öznitelikler **uygulanmamıştır**, bu durum alt sayfanın `window.opener` nesnesine erişmesine izin vermektedir.

### Uygulama, JavaScript `window.open()` üzerinden Reverse Tabnabbing'e karşı savunmasız mı?
- [ ] Hayır — `window.open()` kullanılmamaktadır veya `opener` özelliği açıkça null olarak ayarlanmıştır.
- [ ] Evet — `window.open()` kullanılmaktadır ve `opener` referansı alt pencere için **erişilebilir durumdadır**.

### Bir saldırgan, yeni sekmede açılan bir bağlantının hedefini kontrol edebilir mi?
- [ ] Hayır — tüm bağlantı hedefleri sabit kodlanmıştır (hardcoded) ve güvenilir alan adlarını işaret etmektedir.
- [ ] Evet — bağlantı hedefleri kullanıcı tarafından sağlanmaktadır (örneğin sosyal medya profilleri, biyografi bağlantıları) ve tabnabbing korumaları için **sanitize edilmemiştir**.

---

## WSTG-APIT-01 — API Keşfi (API Reconnaissance)

API keşfi (API Reconnaissance), uç noktaları (endpoints), desteklenen yöntemleri ve altta yatan veri yapılarını ortaya çıkarmak amacıyla API saldırı yüzeyinin sistematik olarak tanımlanması ve haritalanması sürecidir. Saldırganlar, belgelenmemiş (gölge - shadow) API'ları, eski güvenlik açıklarına sahip artık kullanılmayan sürümleri ve dahili iş mantığını ortaya çıkaran Swagger veya OpenAPI tanımları gibi açıkta kalan dokümantasyon dosyalarını keşfetmek için bu aşamayı gerçekleştirirler. Öngörülebilir URL yapılarını, istemci tarafı JavaScript dosyalarını ve dizin kaba kuvvet (brute-force) sonuçlarını analiz ederek, bir saldırgan Kırık Nesne Seviyesinde Yetkilendirme (Broken Object Level Authorization - BOLA) veya Toplu Atama (Mass Assignment) saldırıları için hedef belirlemek üzere API'nın kapsamlı bir haritasını oluşturabilir. Bu keşif çalışması, uygulama arka ucunun işlevselliğine dair bir yol haritası elde etmek için genellikle `/api/v1/`, `/swagger.json` veya `/graphql` gibi yaygın yolları hedefler.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-APIT-01 |
| **CWE** | CWE-200 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Bilgilendirme / Düşük |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/01-API_Reconnaissance  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/api-testing  

**Araçlar:** `Kiterunner`, `ffuf`, `Arjun`, `Burp Suite (Logger++)`, `Postman`, `Swagger-ez`

### API dokümantasyon dosyaları (Swagger, OpenAPI, WSDL) herkese açık olarak erişilebilir mi?
- [ ] Hayır — API dokümantasyonu erişilebilir **değildir** veya kimlik doğrulaması ile sınırlandırılmıştır  
- [ ] Evet — Dokümantasyon erişilebilirdir ancak geçerli kimlik bilgileri gerektirir  
- [ ] Evet — API dokümantasyonu (örneğin `/swagger-ui.html`), kimlik doğrulaması olmaksızın **herkese açık olarak erişilebilirdir**  

### Belgelenmemiş veya "gölge" (shadow) API uç noktaları fuzzing yoluyla keşfedilebiliyor mu?
- [ ] Hayır — Dizin ve uç nokta fuzzing işlemi sonucunda belgelenmemiş bir yol bulunamamıştır  
- [ ] Evet — Belgelenmemiş uç noktalar bulunmuştur ancak bunlara yetkilendirme olmaksızın **erişilememektedir**  
- [ ] Evet — Belgelenmemiş uç noktalar bulunmuştur ve geçerli bir yetkilendirme olmaksızın **erişilebilmektedir**  

### Uygulama birden fazla API sürümünü (örneğin /v1/, /v2/, /beta/) ifşa ediyor mu?
- [ ] Hayır — Yalnızca güncel ve sıkılaştırılmış API sürümü erişilebilirdir  
- [ ] Evet — Eski sürümler mevcuttur ancak güncel sürümle **aynı** güvenlik kontrollerini uygulamaktadır  
- [ ] Evet — Eski sürümler (örneğin `/v1/`) erişilebilirdir ve daha yeni sürümlerdeki güvenlik kontrollerinden **yoksundur**  

### İstemci tarafı kaynakları (JavaScript/Mobil Uygulamalar), API uç nokta yapılarını veya anahtarlarını sızdırıyor mu?
- [ ] Hayır — İstemci tarafı kodu, kodun içine gömülmüş (hardcoded) API yolları veya hassas anahtarlar **içermemektedir**  
- [ ] Evet — İstemci tarafı kodu API uç nokta haritalarını içerir ancak hassas anahtar **içermez**  
- [ ] Evet — API anahtarları, gizli bilgiler (secrets) veya yalnızca dahili kullanıma özel uç nokta URL'leri, istemci tarafı kaynaklarında **kodun içine gömülmüş (hardcoded)** haldedir  

### Bilinen uç noktalarda gizli parametreler veya üstbilgiler (headers) keşfedilebiliyor mu?
- [ ] Hayır — Parametre fuzzing işlemi (örneğin `Arjun` ile), gizli girdiler **ortaya çıkarmamıştır**  
- [ ] Evet — Gizli parametreler (örneğin `debug=true`, `admin=1`) keşfedilmiştir ancak **işlevsel değildir**  
- [ ] Evet — Gizli parametreler keşfedilmiştir ve uygulama davranışını değiştirmek için **kullanılabilmektedir**

---

## WSTG-APIT-02 — API Nesne Seviyesinde Bozuk Yetkilendirme (BOLA)

Nesne Seviyesinde Bozuk Yetkilendirme (BOLA), aynı zamanda Güvensiz Doğrudan Nesne Referansı (IDOR) olarak da bilinir; bir API'nin, bir kullanıcının bir ID ile tanımlanan belirli bir kaynağa erişmek veya kaynağı manipüle etmek için uygun izinlere sahip olup olmadığını doğrulamada başarısız olması durumunda meydana gelir. Saldırganlar, diğer kullanıcılara ait verilere erişmek için istek yollarındaki (request paths), sorgu parametrelerindeki (query parameters) veya JSON gövdelerindeki (JSON bodies) tanımlayıcıları —sayısal ID'ler veya UUID'ler gibi— sistematik olarak numaralandırarak veya tahmin ederek bu durumdan faydalanırlar. Bu zafiyet, modern API güvenliğindeki en yaygın ve etkili sorun olup; kitlesel veri sızdırmasına, yetkisiz değişikliğe veya tam hesap ele geçirmeye (account takeover) yol açabilir. Bir saldırganın bakış açısıyla amaç, nesne tanımlayıcılarını kabul eden uç noktaları (endpoints) belirlemek ve sunucu tarafındaki yetkilendirme mantığının eksik olup olmadığını veya bu ID'leri manipüle ederek atlatılıp atlatılamayacağını test etmektir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-APIT-02 |
| **CWE** | CWE-285 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Kritik |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/02-API_Broken_Object_Level_Authorization  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/api-testing  

**Araçlar:** `Burp Suite (Intruder/Repeater)`, `Autorize`, `Postman`, `ffuf`, `Arjun`

### API istekleri içindeki nesne tanımlayıcıları tahmin edilebilir veya numaralandırılabilir mi?
- [ ] Hayır — tanımlayıcılar uzun, rastgele ve kriptografik olarak güvenlidir (örneğin, UUIDv4)  
- [ ] Evet — tanımlayıcılar ardışık tam sayılardır (örneğin, `101`, `102`)  
- [ ] Evet — tanımlayıcılar tahmin edilebilir bir kalıbı izler veya halka açık bilgilerden türetilmiştir  

### API, her istek için nesne sahipliğinin sunucu tarafında doğrulamasını yapıyor mu?
- [ ] Evet — yetkilendirme kontrolleri her istek için uygulanır ve atlatılması **mümkün değildir**  
- [ ] Evet — kontroller uygulanır ancak parametre kirliliği (parameter pollution) veya metot tünelleme (method tunneling) yoluyla atlatılması **mümkündür**  
- [ ] Hayır — uygulama, belirli bir kaynak sahipliğini kontrol etmeden yalnızca kullanıcının kimliğinin doğrulanmış olmasına dayanır  

### Tanımlayıcıyı değiştirerek başka bir kullanıcının kaynağına erişmek veya kaynağı değiştirmek mümkün müdür?
- [ ] Hayır — diğer kullanıcıların kaynaklarına yetkisiz erişim **mümkün değildir**  
- [ ] Evet — yetkisiz **okuma** erişimi (Yatay BOLA) **mümkündür**  
- [ ] Evet — yetkisiz **değişiklik** veya **silme** (Yatay BOLA) **mümkündür**  

### API, ID'leri değiştirerek yönetici veya sistem düzeyindeki nesnelere erişime izin veriyor mu?
- [ ] Hayır — yönetici kaynakları ikincil yetkilendirme katmanları ile korunmaktadır  
- [ ] Evet — sistem düzeyindeki kaynaklara erişim veya bunları değiştirme (Dikey BOLA) **mümkündür**  

### Tanımlayıcıyı isteğin farklı bir bölümüne taşıyarak yetkilendirme kontrolü atlatılabilir mi?
- [ ] Hayır — yetkilendirme mantığı, parametre konumundan bağımsız olarak tutarlıdır  
- [ ] Evet — ID, URL yolundan JSON gövdesine veya başlıklara (headers) taşındığında atlatma **mümkündür**  
- [ ] Evet — birden fazla ID sağlandığında ve sunucu yetkisiz olanı işlediğinde atlatma **mümkündür**

---

## WSTG-APIT-99 — GraphQL Testi

GraphQL, istemcilerin belirli veri yapılarını talep etmesine olanak tanıyan API'lar için bir sorgu dilidir; ancak hatalı yapılandırmalar genellikle bilgi ifşası (information disclosure) ve kaynak tükenmesi (resource exhaustion) dahil olmak üzere önemli güvenlik risklerine yol açar. Zafiyetler genellikle etkinleştirilmiş introspection, sorgu derinliği/karmaşıklığı (query depth/complexity) sınırlarının eksikliği ve çözümleyici fonksiyonlar (resolver functions) içindeki yetersiz nesne seviyesinde yetkilendirmeden (Broken Object-Level Authorization - BOLA) kaynaklanır. Saldırganlar, tüm şemayı haritalamak için introspection özelliğini kullanarak, Hizmet Dışı Bırakma (Denial of Service - DoS) saldırılarını tetiklemek için dairesel parçalar (circular fragments) kullanarak veya iç içe geçmiş sorgular (nested queries) aracılığıyla yetkisiz alanlara erişerek bu uç noktaları (endpoints) istismar ederler. Testler, uygulamanın katı erişim kontrolleri ve kaynak yönetimi uyguladığından emin olmak için `/graphql`, `/v1/graphql` veya `/graphiql` uç noktalarına odaklanır.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-APIT-99 |
| **CWE** | CWE-200 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Kritik* |

> *Yetkisiz nesne seviyesinde yetkilendirme (BOLA), PII (Kişisel Olarak Tanımlanabilir Bilgiler) veya idari mutasyonlara (administrative mutations) yetkisiz erişime izin veriyorsa önem derecesi Kritik seviyesine çıkar.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/99-Testing_GraphQL  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/graphql  

**Araçlar:** `InQL`, `Graphw00f`, `GraphQL Voyager`, `Burp Suite`, `Altair GraphQL Client`, `Clairvoyance`

### GraphQL introspection sistemi etkin mi?
- [ ] Hayır — introspection **devre dışıdır** ve şema haritalanamaz  
- [ ] Evet — introspection **etkindir** ancak idari kimlik doğrulaması gerektirir  
- [ ] Evet — introspection **etkindir** ve herkese açıktır *(Yüksek)*  

### Sorgularda kaynak sınırları (derinlik ve karmaşıklık) uygulanıyor mu?
- [ ] Evet — katı sorgu derinliği ve karmaşıklığı sınırları **uygulanmaktadır** ve zorunlu kılınmıştır  
- [ ] Evet — sınırlar **uygulanmaktadır** ancak takma adlar (aliases) veya parçalar (fragments) kullanılarak atlatılabilir  
- [ ] Hayır — hiçbir sınır **uygulanmamaktadır**; bu durum özyinelemeli (recursive) sorgulara ve DoS saldırılarına izin verir  

### Çözümleyicilerde (resolvers) Nesne Seviyesinde Yetkilendirme Hatası (BOLA) var mı?
- [ ] Hayır — her sorgu için alan (field) ve nesne seviyesinde yetkilendirme **uygulanmaktadır**  
- [ ] Evet — bazı alanlara yetkilendirme **uygulanmaktadır** ancak hassas nesneler açığa çıkmaktadır  
- [ ] Evet — yetkilendirme **uygulanmamaktadır**; ID manipülasyonu yoluyla herhangi bir nesneye erişime izin verilmektedir  

### GraphQL mutasyonları (mutations) yetkisiz kullanıma karşı uygun şekilde korunuyor mu?
- [ ] Hayır — şemada mutasyonlar **bulunmamaktadır**  
- [ ] Evet — mutasyonlar geçerli kimlik doğrulaması ve katı yetkilendirme kontrolleri gerektirir  
- [ ] Evet — kimliği doğrulanmamış kullanıcılar için mutasyonlar **mümkündür** veya yetkilendirme eksiktir  

### Hata mesajları hassas uygulama veya şema ayrıntılarını sızdırıyor mu?
- [ ] Hayır — hata mesajları geneldir ve dahili mantığı **ifşa etmez**  
- [ ] Evet — hata mesajları temel veritabanı türlerini veya "Bunu mu demek istediniz...?" (Did you mean...?) aracılığıyla önerileri ifşa eder  
- [ ] Evet — yanıtlarda tam yığın izleme (full stack traces) ve hassas yapılandırma verileri **ifşa edilmektedir**