## WSTG-INPV-10 — IMAP SMTP Injection

IMAP ve SMTP enjeksiyonu (IMAP SMTP Injection) zafiyetleri, bir web uygulaması kullanıcı tarafından sağlanan verileri mail sunucusuna gönderilen komutlara dahil etmeden önce düzgün bir şekilde filtrelemediğinde ortaya çıkar. Sıfır başı ve satır besleme (Carriage Return and Line Feed - CRLF) karakterleri enjekte edilerek, saldırgan amaçlanan komutu sonlandırabilir ve ek alıcılar (CC/BCC) eklemek veya mesaj gövdesini değiştirmek gibi rastgele posta talimatları ekleyebilir. Bu kusurlar en sık web-to-mail ağ geçitlerinde, iletişim formlarında ve IMAP4 veya SMTP gibi protokoller aracılığıyla mail sunucularıyla doğrudan iletişim kuran e-posta istemcilerinde bulunur. Başılı bir istismar; spam'in yetkisiz bir şekilde iletilmesine (relay), hassas e-posta içeriklerinin sızdırılmasına ve bazı durumlarda posta sunucusunun bütünlüğünün tamamen tehlikeye atılmasına olanak tanır.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INPV-10 |
| **CWE** | CWE-93 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/10-Testing_for_IMAP_SMTP_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite (Repeater/Intruder)`, `netcat`, `telnet`, `nmap`

### Uygulama, IMAP veya SMTP komutları oluşturmak için kullanıcı tarafından sağlanan girdileri işliyor mu?
- [ ] Hayır — uygulama kullanıcı girdisi aracılığıyla mail sunucularıyla etkileşime girmez  
- [ ] Evet — uygulama mail sunucularıyla etkileşime girer ancak güvenli bir API veya kütüphane kullanır  
- [ ] Evet — uygulama kullanıcı kontrollü parametreleri kullanarak ham (raw) mail komutları oluşturur  

### Girdi, Carriage Return (`\r`) ve Line Feed (`\n`) dizilerinin enjeksiyonunu önlemek için doğrulanıyor mu?
- [ ] Evet — CRLF karakterleri **sıkı** bir şekilde filtrelenir, kodlanır veya reddedilir  
- [ ] Evet — girdi, karakterlerden oluşan sıkı bir izin verme listesine (allowlist) göre doğrulanır  
- [ ] Hayır — CRLF karakterleri filtrelenmez, bu da komut sonlandırma ve enjeksiyona izin verir  

### Bir saldırgan `Bcc:` veya `Cc:` gibi ek mail başlıkları (headers) enjekte edebilir mi?
- [ ] Hayır — sıkı girdi kısıtlamaları nedeniyle başlık (header) değişikliği **mümkün değildir**  
- [ ] Evet — CRLF dizileri aracılığıyla ek başlıkların enjeksiyonu **mümkündür**  
- [ ] Evet — harici alan adlarına mail iletimi (mail relaying) **etkindir** ve istismar edilebilir *(Kritik)*  

### Uygulama, yetkisiz posta kutularına erişmek için IMAP komutlarının manipüle edilmesine izin veriyor mu?
- [ ] Hayır — uygulama IMAP **kullanmaz** veya posta kutusu erişimini kimliği doğrulanmış kullanıcıyla sıkı bir şekilde sınırlar  
- [ ] Evet — IMAP komut enjeksiyonu **mümkündür** ancak meta veri listeleme (enumeration) ile sınırlıdır  
- [ ] Evet — IMAP komut enjeksiyonu, diğer kullanıcıların e-postalarının okunmasına veya silinmesine olanak tanır  

### Arka uç (backend) mail sunucusu komut ardışık düzenine (command pipelining) karşı savunmasız mı?
- [ ] Hayır — mail sunucusu toplu komutları veya tek bir oturumdaki birden fazla komutu reddeder  
- [ ] Evet — mail sunucusu tek bir girdi alanı üzerinden enjekte edilen birden fazla komutu işler  

---