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