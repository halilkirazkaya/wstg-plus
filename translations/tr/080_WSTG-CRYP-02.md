## WSTG-CRYP-02 — Padding Oracle Testi

Padding Oracle zafiyetleri, bir uygulamanın deşifreleme (decryption) sürecinin, deşifre edilen bir şifreli metnin (ciphertext) dolgulamasının (padding) geçerli olup olmadığına dair bilgi sızdırdığı durumlarda ortaya çıkar. Farklı HTTP durum kodları, belirli hata mesajları veya yanıt süresi farklılıkları gibi bu yan kanal (side-channel) yanıtlarını gözlemleyerek; bir saldırgan, şifreleme anahtarını bilmeden verileri deşifre edebilir ve potansiyel olarak rastgele açık metinleri (plaintext) yeniden şifreleyebilir. Bu zafiyet genellikle PKCS#7 dolgulamalı Cipher Block Chaining (CBC) modunda blok şifreleyiciler (block ciphers) kullanan şifrelenmiş oturum çerezlerinde, kimlik doğrulama belirteçlerinde veya URL parametrelerinde görülür. İstismar edilmesi, gizliliğin tamamen kaybolmasına neden olur ve sahte şifrelenmiş blokların oluşturulması yoluyla bütünlüğün (integrity) bozulmasına yol açabilir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CRYP-02 |
| **CWE** | CWE-209 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Kritik* |

> *Zafiyet; oturum yönetimi, kimlik belirteçleri veya PII (Kişisel Veri) şifrelemesinde bulunuyorsa Önem Derecesi Kritiktir.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/02-Testing_for_Padding_Oracle  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `PadBuster`, `Burp Suite (Intruder/Padding Oracle Solver)`, `pkcs7-padbuster`, `CyberChef`

### Hassas veriler için CBC modunda bir blok şifreleyici kullanılıyor mu?
- [ ] Hayır — uygulama kimliği doğrulanmış şifreleme (AES-GCM/ChaCha20-Poly1305) kullanıyor  
- [ ] Evet — uygulama blok şifreleyiciler kullanıyor ancak dolgulama (padding) **gerekmiyor** (örn. Akış şifreleyiciler veya CTR modu)  
- [ ] Evet — uygulama CBC modu kullanıyor ve dolgulama (padding) **uygulanıyor**  

### Sunucu, dolgulama geçerliliğini yan kanallar aracılığıyla ifşa ediyor mu?
- [ ] Hayır — sunucu tek tip hata mesajları ve tutarlı yanıt süreleri döndürüyor  
- [ ] Evet — sunucu, dolgulama hataları için farklı HTTP durum kodları (örn. 200'e karşı 500) döndürüyor  
- [ ] Evet — sunucu belirli hata mesajları döndürüyor (örn. "Invalid Padding", "Decryption failed")  
- [ ] Evet — sunucu deşifreleme hatası sırasında ölçülebilir zamanlama farkları sergiliyor  

### Şifreli metnin otomatik olarak deşifre edilmesi mümkün mü?
- [ ] Hayır — yan kanallar (side channels) istismar için yeterince **kararlı değil**  
- [ ] Evet — şifreli metin (ciphertext) **kısmen** deşifre edilebiliyor (son birkaç blok)  
- [ ] Evet — `PadBuster` gibi araçlar kullanılarak **tam açık metin (plaintext) kurtarma mümkündür**  

### Saldırgan bit-flipping gerçekleştirebilir mi veya geçerli şifreli metinler üretebilir mi?
- [ ] Hayır — bütünlük kontrolleri (HMAC), deşifreleme işleminden **önce** doğrulanıyor  
- [ ] Evet — herhangi bir bütünlük kontrolü mevcut değil, ancak Payload oluşturma **uygulanabilir değil**  
- [ ] Evet — rastgele şifreli metin oluşturma **mümkündür**; bu da yetki yükseltmeye (privilege escalation) veya veri enjeksiyonuna olanak tanır  

---