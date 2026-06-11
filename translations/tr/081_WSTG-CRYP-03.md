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