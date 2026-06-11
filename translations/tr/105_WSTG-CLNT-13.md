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