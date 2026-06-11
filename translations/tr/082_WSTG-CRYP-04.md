## WSTG-CRYP-04 — Zayıf Kriptografik Primitiflerin Test Edilmesi

Zayıf kriptografik primitifler (Weak Cryptographic Primitives), çakışma saldırılarına (collision attacks), frekans analizine veya kaba kuvvet (Brute Force) şifre çözümüne karşı duyarlı olan MD5, SHA-1, DES veya RC4 gibi modası geçmiş veya güvenli olmayan algoritmaların kullanılmasını içerir. Bu zayıflıklar genellikle parola özetleme (password hashing), durağan veri şifreleme (data encryption at rest), oturum belirteci oluşturma (session token generation) veya uygulama arka ucundaki dijital imza doğrulama işlemlerinde ortaya çıkar. Bir saldırgan açısından bu primitiflerin belirlenmesi, açık metin parolaları elde etmek için yakalanan özetlerin (hashes) kırılması veya kimlik doğrulama belirteçlerinin (authentication tokens) sahtesinin oluşturulması gibi veri bütünlüğünün ve gizliliğinin tehlikeye atılmasına olanak tanır. Modern uygulamalar bunun yerine, kripto analize karşı sağlam koruma sağlamak amacıyla Argon2, Bcrypt veya AES-GCM gibi endüstri standardı olan, çakışmaya dayanıklı ve yüksek entropili primitifler kullanmalıdır.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CRYP-04 |
| **CWE** | CWE-327 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/04-Testing_for_Weak_Cryptographic_Primitives  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `hashcat`, `John the Ripper`, `Burp Suite`, `CyberChef`, `OpenSSL`, `nmap (ssl-enum-ciphers)`

### Hassas veri depolama için MD5 veya SHA-1 gibi desteği sonlandırılmış (deprecated) özetleme algoritmaları kullanılıyor mu?
- [ ] Hayır — uygulama Argon2 veya Bcrypt gibi modern, çakışmaya dayanıklı algoritmalar kullanıyor  
- [ ] Evet — desteği sonlandırılmış algoritmalar kullanılıyor ancak **yalnızca** güvenlik açısından hassas olmayan bütünlük kontrolleri için  
- [ ] Evet — parolalar veya hassas belirteçler (tokens) için desteği sonlandırılmış algoritmalar **kullanılıyor** *(Yüksek)*  

### DES, 3DES veya RC4 gibi zayıf simetrik şifreleme algoritmaları (ciphers) şu anda kullanılıyor mu?
- [ ] Hayır — GCM veya CBC gibi güvenli bir modda AES (128-bit veya daha yüksek) kullanılıyor  
- [ ] Evet — eski şifreleme algoritmaları geriye dönük uyumluluk için **etkinleştirilmiş** durumda ancak varsayılan değil  
- [ ] Evet — eski şifreleme algoritmaları, durağan veya iletim halindeki verileri korumak için birincil yöntemdir  

### Uygulama "kendi yapımı" (roll-your-own) veya standart dışı bir kriptografik mantık uyguluyor mu?
- [ ] Hayır — uygulama kesinlikle standart, hakemli incelemeden geçmiş (peer-reviewed) kriptografik kütüphanelere bağlı kalıyor  
- [ ] Evet — özel sarmalayıcılar (wrappers) mevcut ancak temel primitifler endüstri standardıdır  
- [ ] Evet — hassas verilere özel mülkiyetli (proprietary) kriptografik algoritmalar veya özel mantık **uygulanıyor** *(Kritik)*  

### Kriptografik tuzlar (Salts) ve başlatma vektörleri (IVs) en iyi uygulamalara göre mi yönetiliyor?
- [ ] Evet — tuzlar kullanıcı başına benzersizdir ve IV'ler her işlem için tahmin edilemezdir  
- [ ] Evet — tuzlar kullanılıyor ancak tüm kayıtlarda benzersiz **değil**  
- [ ] Hayır — tuzlar statik, sabit kodlanmış (hardcoded) veya mevcut değil ve IV'ler birden fazla oturumda **yeniden kullanılıyor**  

### Asimetrik anahtarların (RSA, Diffie-Hellman) bit uzunluğu modern standartlar için yeterli mi?
- [ ] Evet — RSA/DH anahtarları 2048 bit veya daha yüksektir ya da Eliptik Eğri (ECC) kullanılıyor  
- [ ] Hayır — anahtarlar 2048 bitten azdır ancak atlatılması (bypass) kolay değildir  
- [ ] Hayır — anahtarlar 1024 bit veya daha küçüktür, bu da onları çarpanlara ayırma veya hesaplamalı saldırılara karşı **savunmasız** hale getirir  

---