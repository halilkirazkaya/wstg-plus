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