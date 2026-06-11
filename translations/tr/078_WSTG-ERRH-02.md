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