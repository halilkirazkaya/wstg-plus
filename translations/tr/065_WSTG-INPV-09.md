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