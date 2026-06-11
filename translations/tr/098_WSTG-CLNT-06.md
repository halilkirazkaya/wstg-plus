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