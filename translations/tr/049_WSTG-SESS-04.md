## WSTG-SESS-04 — Açığa Çıkan Oturum Değişkenlerinin Test Edilmesi

Açığa çıkan oturum değişkenleri (Exposed session variables), hassas oturum tanımlayıcılarının veya durumla ilgili verilerin; URL sorgu dizileri (URL query strings), Referer başlıkları veya sistem günlükleri (system logs) gibi güvenli olmayan kanallar üzerinden iletildiği durumlarda meydana gelir. Bu durum, tanımlayıcıların tarayıcı geçmişinde önbelleğe alınabilmesi, ara proxy'ler tarafından günlüğe kaydedilebilmesi veya Referer başlığı aracılığıyla üçüncü taraf sitelere sızabilmesi nedeniyle oturum çalma (session hijacking) riskini önemli ölçüde artırır. Penetrasyon test uzmanları (Pentesters), tüm giden istekleri izleyerek ve uygulama günlüklerini veya sunucu yapılandırmalarını kasıtsız veri sızıntılarına karşı inceleyerek bu açıkları tespit ederler. Gerçek dünya senaryolarında, saldırganlar bu sızıntıları paylaşılan makinelerden geçerli oturum belirteçlerini (session tokens) toplayarak veya kimlik doğrulamayı atlatmak ve kullanıcı hesaplarına yetkisiz erişim sağlamak amacıyla trafik modellerini analiz ederek istismar (exploit) ederler.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-SESS-04 |
| **CWE** | CWE-598 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek* |

> *Açığa çıkan değişkenin, doğrudan hesap ele geçirmeye (account takeover) izin veren bir oturum belirteci (session token) olması durumunda önem derecesi Yüksek (High) olarak değerlendirilir.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/04-Testing_for_Exposed_Session_Variables  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Araçlar:** `Burp Suite (Proxy/Logger)`, `OWASP ZAP`, `Browser DevTools`, `grep`

### Oturum tanımlayıcı (session identifier) URL sorgu dizisi (query string) içinde iletiliyor mu?
- [ ] Hayır — oturum tanımlayıcıları URL sorgu dizisinde bulunmamaktadır.
- [ ] Evet — tanımlayıcılar mevcut ancak oturum kısa süreli veya düşük risklidir.
- [ ] Evet — benzersiz oturum ID'leri (unique session IDs) URL sorgu dizisinde bulunmaktadır *(Kritik)*.

### Uygulama, Referer başlığı üzerinden oturum belirteçlerini (session tokens) sızdırıyor mu?
- [ ] Hayır — `Referrer-Policy` sızıntıyı önlüyor veya üçüncü taraf bağlantısı bulunmamaktadır.
- [ ] Evet — oturum belirteçleri Referer başlığı aracılığıyla üçüncü taraf alan adlarına iletilmektedir.

### Oturum değişkenleri sunucu tarafı veya uygulama günlüklerinde (logs) saklanıyor mu?
- [ ] Hayır — oturum değişkenleri maskelenmiş veya günlüklerden hariç tutulmuştur.
- [ ] Evet — oturum değişkenleri uygulama veya web sunucusu günlüklerine düz metin (plain text) olarak kaydedilmektedir.

### Uygulama, durum değiştiren işlemler için GET isteklerini kullanıyor mu?
- [ ] Hayır — uygulama, hassas işlemler için `POST` veya diğer idempotent olmayan yöntemleri kullanmaktadır.
- [ ] Evet — `GET` kullanılmaktadır; bu durum oturum verilerinin tarayıcı geçmişinde ve ara proxy'lerde önbelleğe alınmasına neden olmaktadır.

### `Cache-Control` başlığı, oturumla ilgili verilerin önbelleğe alınmasını önleyecek şekilde yapılandırılmış mı?
- [ ] Evet — hassas yanıtlara `Cache-Control: no-store` (veya benzeri) uygulanmıştır.
- [ ] Evet — kontroller mevcuttur ancak tarayıcı uç durumları (edge cases) üzerinden atlatma (bypass) mümkündür.
- [ ] Hayır — oturum verilerini içeren hassas yanıtlar tarayıcı tarafından önbelleğe alınabilmektedir.

---