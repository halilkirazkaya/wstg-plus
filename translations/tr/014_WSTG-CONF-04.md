## WSTG-CONF-04 — Hassas Bilgiler İçin Eski Yedek ve Referans Verilmemiş Dosyaların İncelenmesi

Eski yedeklerin ve referans verilmemiş dosyaların incelenmesi; bir web sunucusu üzerinde genel erişime açık olması amaçlanmayan unutulmuş, geçici veya gizli dosyaların tespit edilmesini kapsar. Bu dosyalar genellikle kaynak kodu yedeklerini (`.zip`, `.bak`), metin düzenleyici swap dosyalarını (`.swp`, `~`) veya hassas bilgi sızdırabilecek kaynak kontrol meta verilerini (`.git`, `.svn`) içerir. Saldırganlar; veritabanı kimlik bilgilerini, kod içine gömülmüş (hardcoded) API anahtarlarını veya daha ileri seviye istismarları (exploitation) kolaylaştıran mantıksal kurguları ele geçirmek amacıyla, yaygın isimlendirme kurallarına karşı otomatik kelime listeleri ve keşif araçları kullanarak Brute Force saldırıları düzenler. Bu zafiyet tipik olarak manuel sunucu bakımlarından veya canlı (production) ortamdaki web kök dizinini (web root) temizlemeyi başaramayan kusurlu CI/CD süreçlerinden kaynaklanır.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CONF-04 |
| **CWE** | CWE-530 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek* |

> *Konfigürasyon dosyaları, kaynak kod arşivleri veya kimlik bilgileri tespit edilirse önem derecesi Yüksek (High) olarak kabul edilir.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/04-Review_Old_Backup_and_Unreferenced_Files_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `ffuf`, `dirsearch`, `gobuster`, `GitTools`, `Burp Suite (Engagement Tools)`, `Wfuzz`

### Web sunucusunda dizin listeleme (directory listing) özelliği etkin mi?
- [ ] Hayır — dizin listeleme **devre dışıdır** ve 403 Forbidden veya özel bir hata sayfası döndürülmektedir  
- [ ] Evet — dizin listeleme hassas olmayan dizinlerde **etkindir**  
- [ ] Evet — dizin listeleme kaynak kodu veya hassas dosyalar içeren dizinlerde **etkindir** *(Kritik)*  

### Yaygın uzantılara sahip (.bak, .old, .save gibi) yedek dosyaları keşfedilebiliyor mu?
- [ ] Hayır — yaygın yedekleme uzantıları **bulunamadı** veya sunucu yapılandırması tarafından engellenmiş  
- [ ] Evet — yedek dosyaları mevcut ancak hassas bilgi **içermiyor**  
- [ ] Evet — yapılandırma veya kaynak koduna ait yedek dosyaları erişilebilir **durumdadır** *(Yüksek)*  

### Kaynak kontrol dizinleri (.git, .svn gibi) veya meta veriler açıkta mı?
- [ ] Hayır — kaynak kontrol meta verileri **mevcut değil** veya doğru şekilde engellenmiş  
- [ ] Evet — meta veriler mevcut ancak tüm depo (repository) yeniden **oluşturulamıyor**  
- [ ] Evet — tüm kaynak kodu deposu, açıkta kalan meta veriler aracılığıyla **sızdırılabilir**  

### Web kök dizininde (web root) uygulamanın sıkıştırılmış arşivleri (.zip, .tar.gz, .rar gibi) bulunuyor mu?
- [ ] Hayır — Brute Force veya numaralandırma (enumeration) yoluyla hassas arşiv dosyası tespit edilmedi  
- [ ] Evet — arşivler bulundu ancak parola korumalılar veya yalnızca halka açık varlıkları içeriyorlar  
- [ ] Evet — uygulama kaynağını veya verilerini içeren şifrelenmemiş arşivler halka açık şekilde **erişilebilirdir**  

### Uygulama, metin düzenleyiciler veya IDE'ler tarafından oluşturulan geçici dosyaları sızdırıyor mu?
- [ ] Hayır — `.swp`, `~` veya `.DS_Store` gibi geçici dosyalar **mevcut değil**  
- [ ] Evet — hassas olmayan geçici dosyalar **mevcut**  
- [ ] Evet — geçici dosyalar kaynak kodu parçacıklarını veya dahili yolları (internal paths) ifşa ediyor  

---