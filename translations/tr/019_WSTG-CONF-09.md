## WSTG-CONF-09 — Dosya İzinlerinin Test Edilmesi

Dosya izinlerinin test edilmesi, hassas kaynakların yetkisiz kullanıcılara veya süreçlere aşırı derecede ifşa edilmediğinden emin olmak amacıyla web sunucusundaki dosya ve dizinlere atanan erişim kontrol seviyelerinin denetlenmesini içerir. Yanlış yapılandırılmış izinler; saldırganların veritabanı kimlik bilgilerini içeren hassas yapılandırma dosyalarını okumasına, tescilli kaynak kodlarını görüntülemesine veya Remote Code Execution (Uzaktan Kod Çalıştırma) gerçekleştirmek için sunucu tarafı betikleri değiştirmesine olanak tanıyabilir. Bu zafiyet tipik olarak; yapılandırma yolları, günlük (log) klasörleri veya yükleme depoları gibi kritik uygulama dizinlerinde varsayılan "world-readable" (herkes tarafından okunabilir) veya "world-writable" (herkes tarafından yazılabilir) bayraklarının bırakıldığı dağıtım aşamasında ortaya çıkar. Bir saldırganın perspektifinden, hatalı şekilde güvenliği sağlanmış bir dosyanın keşfedilmesi, genellikle Privilege Escalation (Yetki Yükseltme) veya tam sistem ele geçirme için birincil tetikleyici görevi görür.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CONF-09 |
| **CWE** | CWE-732 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek* |

> *Hassas yapılandırma dosyaları (örneğin `.env`, `web.config`) okunabilir durumdaysa veya web kök dizinine (web root) yazma erişimi mümkünse Önem Derecesi "Yüksek" olarak değerlendirilir.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/09-Test_File_Permission  
* https://hacktricks.wiki/en/linux-hardening/privilege-escalation/index.html  

**Araçlar:** `ls`, `find`, `LinPeas`, `WinPeas`, `ffuf`, `dirsearch`, `Nmap`

### Hassas uygulama yapılandırma dosyaları yetkisiz erişime karşı korunuyor mu?
- [ ] Evet — yapılandırma dosyalarına (örneğin `.env`, `config.php`) web istekleri aracılığıyla **erişilemiyor**  
- [ ] Evet — dosyalar mevcut ancak erişim yalnızca yetkili yerel kullanıcılarla **sınırlandırılmış**  
- [ ] Hayır — hassas yapılandırma dosyalarına **erişilebiliyor** ve kimlik bilgileri veya sırlar ifşa ediliyor  

### Bir saldırgan web kök dizini (web root) içindeki dosya veya dizinleri değiştirebilir mi?
- [ ] Hayır — tüm web kök dizinleri web sunucusu kullanıcısı için **salt okunurdur (read-only)**  
- [ ] Evet — yazılabilir dizinler mevcut ancak betik yürütme (script execution) **devre dışı bırakılmış**  
- [ ] Evet — "world-writable" dizinler mevcut ve betiklerin yüklenmesine ve yürütülmesine **izin veriyor** *(Kritik)*  

### Gevşek izinler nedeniyle sürüm kontrol meta verileri veya yedek dosyaları ifşa ediliyor mu?
- [ ] Hayır — `.git`, `.svn` ve yedek dosyaları (örneğin `.bak`, `~`) **mevcut değil** veya korunuyor  
- [ ] Evet — meta veri dizinleri mevcut ancak dizin listeleme (directory listing) **devre dışı**  
- [ ] Evet — hassas meta veriler veya yedekler **tamamen erişilebilir** durumda, bu da kaynak kodun elde edilmesine olanak tanıyor  

### Web sunucusu kullanıcısı en düşük yetki prensibi (principle of least privilege) ile mi çalışıyor?
- [ ] Evet — web sunucusu süreci **özel bir düşük yetkili** kullanıcı olarak çalışıyor  
- [ ] Hayır — web sunucusu süreci **gereksiz yetkilerle** (örneğin `root` veya `Administrator`) çalışıyor  

### Günlük (log) dosyaları yetkisiz okuma veya değiştirmeye karşı güvenli mi?
- [ ] Evet — günlük dosyaları idari kullanıcılarla **sınırlandırılmış**  
- [ ] Evet — günlük dosyaları okunabilir ancak oturum belirteçleri (session tokens) veya Kişisel Veriler (PII) **içermiyor**  
- [ ] Hayır — günlük dosyaları **herkese açık şekilde okunabiliyor** ve hassas işlem verilerini veya kullanıcı bilgilerini ifşa ediyor  

---