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