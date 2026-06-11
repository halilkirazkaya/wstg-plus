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