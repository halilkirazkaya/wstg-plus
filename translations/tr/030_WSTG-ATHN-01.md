## WSTG-ATHN-01 — Kimlik Bilgilerinin Şifrelenmiş Bir Kanal Üzerinden Aktarılmasının Test Edilmesi

Kimlik bilgilerinin şifrelenmiş bir kanal üzerinden aktarılmasının test edilmesi; kullanıcı adları, parolalar ve oturum belirteçleri (session tokens) gibi hassas kimlik doğrulama verilerinin iletim sırasında ele geçirilmeye karşı korunmasını sağlar. Ağ üzerinde konumlanmış saldırganlar —örneğin halka açık Wi-Fi ağlarındaki Aradaki Adam (Man-in-the-Middle - MitM) saldırıları veya ele geçirilmiş dahili ağlar aracılığıyla— TLS/SSL düzgün bir şekilde zorunlu kılınmamışsa, paket koklama (packet sniffing) araçlarını kullanarak açık metin halindeki kimlik bilgilerini ele geçirebilirler. Bu zafiyet genellikle uygulamanın HTTPS'i zorunlu kılmadığı veya HTTP'den hatalı yönlendirme yaptığı oturum açma uç noktalarında, parola sıfırlama formlarında ve API kimlik doğrulama başlıklarında (headers) görülür. Başarılı bir istismar (exploitation), saldırgana kullanıcı hesaplarına tam erişim sağlayarak kullanıcı verilerinin tamamen tehlikeye girmesine ve uygulama içinde potansiyel yanal harekete (lateral movement) yol açar.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-ATHN-01 |
| **CWE** | CWE-319 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Şiddet** | Yüksek |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/01-Testing_for_Credentials_Transported_over_an_Encrypted_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Araçlar:** `Wireshark`, `tcpdump`, `Burp Suite`, `OWASP ZAP`, `testssl.sh`, `sslyze`, `nmap`

### Tüm kimlik doğrulama süreci için HTTPS zorunlu kılınıyor mu?
- [ ] Evet — HSTS **etkindir** ve tüm trafik kesin olarak HTTPS üzerinden zorlanır  
- [ ] Evet — HTTPS'e yönlendirme gerçekleşiyor, ancak HSTS **etkin değil**  
- [ ] Hayır — kimlik bilgileri şifrelenmemiş HTTP üzerinden **gönderilebiliyor**  

### Oturum açma sayfaları ve formlar şifrelenmiş bir kanal üzerinden mi sunuluyor?
- [ ] Evet — oturum açma formunu içeren sayfa HTTPS aracılığıyla iletiliyor  
- [ ] Hayır — oturum açma sayfası HTTP üzerinden sunuluyor; bu da form eylemi gaspına (form-action hijacking) veya kimlik bilgisi koklamaya (credential sniffing) olanak tanıyor  

### Tüm hassas oturum çerezlerine `Secure` bayrağı uygulanmış mı?
- [ ] Evet — tüm kimlik doğrulama ve oturum çerezlerine `Secure` bayrağı **uygulanmış**  
- [ ] Evet — `Secure` bayrağı sadece bazı çerezlere **uygulanmış**  
- [ ] Hayır — `Secure` bayrağı **uygulanmamış**; bu da çerezlerin şifrelenmemiş istekler aracılığıyla sızdırılmasına (exfiltrated) izin veriyor  

### Sunucu zayıf TLS sürümlerini veya güvensiz şifreleme paketlerini (cipher suites) destekliyor mu?
- [ ] Hayır — sadece modern TLS (1.2+) ve güçlü şifrelemeler **etkin**  
- [ ] Evet — eski sürümler (TLS 1.0/1.1) veya zayıf şifrelemeler (RC4, DES, CBC) **destekleniyor**  

### Uygulama, kimlik bilgilerinin üçüncü taraf alan adlarına (domains) şifrelenmemiş kanallar üzerinden iletilmesini engelliyor mu?
- [ ] Evet — harici alan adlarına hassas veri gönderilmiyor veya tüm harici çağrılar HTTPS üzerinden zorlanıyor  
- [ ] Hayır — kimlik doğrulama başlıkları veya kimlik bilgileri, HTTP üzerinden üçüncü taraf uç noktalarına (örneğin; analitik veya SSO) gönderiliyor  

---