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