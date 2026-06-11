## WSTG-CRYP-01 — Zayıf Taşıma Katmanı Güvenliğinin Test Edilmesi (Testing for Weak Transport Layer Security)

Zayıf taşıma katmanı güvenliğinin (transport layer security) test edilmesi, verilerin iletimi sırasında gizliliğini ve bütünlüğünü sağlamak amacıyla SSL/TLS uygulamalarının ve konfigürasyonlarının analiz edilmesini kapsar. Saldırganlar; Ortadaki Adam (Man-in-the-Middle - MitM) saldırılarını gerçekleştirmek için güncelliğini yitirmiş protokoller (SSLv2, TLS 1.0), zayıf şifreleme paketleri (cipher suites) (NULL, RC4 veya anonim) ve geçersiz veya zayıf imzalanmış sertifikalar gibi yanlış yapılandırmaları hedeflerler. Bu test genellikle uygulamanın web sunucusunu veya şifreli iletişimin kurulduğu yük dengeleyici (load balancer) uç noktalarını hedefler. Başarılı bir istismar (exploit), bir saldırganın hassas verileri ele geçirmesine, kullanıcı oturumlarını çalmasına (hijack) veya protokol düşürme (protocol downgrade) ya da trafik deşifreleme (traffic decryption) yoluyla bağlantıları güvensiz durumlara indirmesine olanak tanır.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CRYP-01 |
| **CWE** | CWE-326 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Orta* |

> *Hassas veriler (PII, kimlik bilgileri, oturum belirteçleri) iletiliyorsa Önem Derecesi Yüksek; genel bilgilendirme amaçlı trafik için Ortadır.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/01-Testing_for_Weak_Transport_Layer_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `testssl.sh`, `sslyze`, `nmap`, `OpenSSL`, `Qualys SSL Labs`

### Güncelliğini yitirmiş veya güvensiz TLS/SSL protokolleri destekleniyor mu?
- [ ] Hayır — yalnızca TLS 1.2 ve TLS 1.3 **etkinleştirilmiştir**  
- [ ] Evet — TLS 1.0 veya TLS 1.1 **etkinleştirilmiştir**  
- [ ] Evet — SSLv2 veya SSLv3 **etkinleştirilmiştir** *(Kritik)*  

### Sunucu tarafından zayıf veya güvensiz şifreleme paketleri (cipher suites) kabul ediliyor mu?
- [ ] Hayır — yalnızca güçlü ve modern şifrelemeler (örn. AES-GCM, ChaCha20) **desteklenmektedir**  
- [ ] Evet — zayıf şifrelemeye sahip (örn. 3DES, RC4) veya düşük bit uzunluklu şifrelemeler **desteklenmektedir**  
- [ ] Evet — NULL, anonim (ADH) veya dışa aktarım (export) şifrelemeleri **desteklenmektedir** *(Kritik)*  

### Sunucu sertifikası geçerli ve güvenilir mi?
- [ ] Evet — sertifika geçerlidir, alan adı (domain) ile eşleşmektedir ve **güvenilir bir CA** tarafından imzalanmıştır  
- [ ] Hayır — sertifikanın **süresi dolmuştur** veya **zayıf bir imza algoritması** (örn. SHA-1) kullanmaktadır  
- [ ] Hayır — sertifika **kendi kendine imzalanmıştır (self-signed)** veya alan adı **uyumsuzluğu (mismatch)** mevcuttur  

### Sunucu, Güvenli Yeniden Anlaşmayı (Secure Renegotiation) destekliyor mu ve Sürüm Düşürme (Downgrade) saldırılarını engelliyor mu?
- [ ] Evet — Secure Renegotiation **desteklenmektedir** ve TLS Fallback SCSV **etkinleştirilmiştir**  
- [ ] Hayır — Secure Renegotiation **desteklenmemektedir**, bu durum potansiyel olarak MitM enjeksiyonuna izin verebilir  
- [ ] Hayır — Sunucu **POODLE** veya **ROBOT** saldırılarına karşı savunmasızdır  

### HTTP Sıkı Taşıma Güvenliği (HSTS) düzgün bir şekilde uygulanmış mı?

| Üstbilgi (Header) | Mevcut | Eksik |
|--------|:-------:|:-------:|
| `Strict-Transport-Security` |  |  |

- [ ] Evet — HSTS üstbilgisi mevcuttur, uzun bir `max-age` değerine sahiptir ve `includeSubDomains` **etkinleştirilmiştir**  
- [ ] Evet — HSTS üstbilgisi mevcuttur ancak `includeSubDomains` eksiktir veya **kısa bir max-age** değerine sahiptir  
- [ ] Hayır — HSTS üstbilgisi **mevcut değildir**  

---