## WSTG-ATHN-06 — Tarayıcı Önbelleği Zafiyeti Testi

Tarayıcı önbelleği zafiyeti (Browser cache weakness), hassas bilgiler yerel tarayıcı önbelleğinde saklandığında ve aynı fiziksel makineye erişimi olan yetkisiz bir kullanıcı tarafından geri alınabildiğinde ortaya çıkar. Uygun önbellek kontrol başlıklarının (cache control headers) eksikliği; kişisel bilgiler, hesap detayları veya oturum tanımlayıcıları (session identifiers) gibi potansiyel olarak hassas verilerin, kullanıcı oturumu kapattıktan veya oturumu kapattıktan sonra da kalıcı olmasına izin verir. Saldırganlar veya paylaşılan ya da genel terminallerdeki sonraki kullanıcılar; tarayıcı geçmişinde gezinerek, "Geri" butonunu kullanarak veya önbelleğe alınmış yanıtları dışarı sızdırmak (exfiltrate) için yerel önbellek dosyalarını inceleyerek bu durumdan faydalanabilirler. Hassas verilerin asla diske yazılmamasını sağlamak amacıyla, kimlik doğrulaması yapılmış içeriği işleyen her uygulama için `Cache-Control: no-store` ve `Pragma: no-cache` gibi katı direktiflerin uygulanması kritik önem taşır.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-ATHN-06 |
| **CWE** | CWE-525 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Şiddet** | Düşük / Orta* |

> *Uygulamaya sık sık paylaşılan/genel kiosklar üzerinden erişiliyorsa veya uygulama yüksek derecede hassas PII/PHI verileri içeriyorsa şiddet Orta (Medium) seviyesine çıkar.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/06-Testing_for_Browser_Cache_Weaknesses  

**Araçlar:** `Burp Suite (Proxy/Repeater)`, `Browser Developer Tools (Network Tab)`, `Zed Attack Proxy (ZAP)`

### Kimlik doğrulaması yapılmış veya hassas sayfalarda uygun önbellek kontrol (cache-control) başlıkları mevcut mu?
- [ ] Evet — `Cache-Control: no-store, no-cache` ve `Pragma: no-cache` başlıkları **mevcut** ve **doğru yapılandırılmış**  
- [ ] Evet — bazı başlıklar mevcut ancak `no-store` **eksik**, bu da disk önbelleğine izin veriyor  
- [ ] Hayır — varsayılan tarayıcı davranışına güveniliyor, önbellekle ilgili hiçbir başlık **uygulanmamış**  

### Uygulama, kullanıcıya özel veriler için `private` direktifini kullanıyor mu?
- [ ] Evet — ara sunucu (proxy) önbelleğe alımını önlemek için tüm kişiselleştirilmiş içeriklerde `Cache-Control: private` **etkinleştirilmiş**  
- [ ] Hayır — içerik `public` olarak işaretlenmiş veya `private` direktifi eksik, bu da içeriği ara proxy'ler tarafından **önbelleğe alınabilir** kılıyor  

### Başarılı bir oturum kapatma işleminden sonra tarayıcının "Geri" butonu ile hassas bilgilere erişilebiliyor mu?
- [ ] Hayır — tarayıcı yeniden kimlik doğrulaması istiyor veya sayfa yerel önbellekten **yüklenmiyor**  
- [ ] Evet — oturum sonlandırıldıktan sonra bile "Geri" butonu aracılığıyla hassas bilgiler **hala görüntülenebiliyor**  

### Hassas HTML dışı dosyalar (örneğin PDF'ler, CSV dışa aktarımları, JSON API yanıtları) önbelleğe almaya karşı korunuyor mu?
- [ ] Hayır — uygulama tarafından hassas hiçbir HTML dışı dosya oluşturulmuyor veya işlenmiyor  
- [ ] Evet — tüm hassas dosya indirmelerine ve API uç noktalarına (endpoints) katı önbellek kontrol başlıkları **uygulanmış**  
- [ ] Hayır — hassas indirmeler veya API yanıtları yerel tarayıcı önbelleğinde veya geçici dizinlerde **saklanıyor**  

### Uygulama, güncelliğini yitirmiş veri kullanımını önlemek için `Expires: 0` veya geçmiş bir tarih kullanıyor mu?
- [ ] Evet — `Expires` başlığı `0` veya geçmiş bir tarihe ayarlanmış, tarayıcının içeriği anında güncelliğini yitirmiş (stale) olarak değerlendirmesi **sağlanmış**  
- [ ] Hayır — hassas sayfalar için `Expires` başlığı **eksik** veya gelecekteki bir tarihe ayarlanmış  

---