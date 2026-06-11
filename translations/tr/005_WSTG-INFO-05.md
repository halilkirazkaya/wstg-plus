## WSTG-INFO-05 — Bilgi Sızıntısı için Web Sayfası İçeriğinin İncelenmesi

Web sayfası içeriğinin incelenmesi; kullanıcılara kasıtsız olarak ifşa edilen hassas bilgileri tespit etmek amacıyla HTML, JavaScript ve CSS dosyaları dahil olmak üzere uygulama yanıtlarının manuel ve otomatik olarak denetlenmesini kapsar. Saldırganlar; geliştirici yorumlarını, gizli form alanlarını, dahili sunucu yollarını, sabit kodlanmış (hardcoded) API anahtarlarını ve sonraki sömürü aşamalarına yardımcı olabilecek hata ayıklama (debugging) bilgilerini bulmak için istemci tarafı kaynak kodunu titizlikle inceler. Bu sızıntı genellikle, geliştiricilerin uygulamayı canlı ortama (production) almadan önce meta verileri veya hata ayıklama kodlarını temizlemeyi unuttuğu durumlarda meydana gelir ve potansiyel olarak mantıksal hataları veya arka uç altyapı detaylarını ortaya çıkarır. Saldırganın bakış açısından bu durum, güvenlik uyarılarını tetiklemeden veya sunucunun arka uç mantığıyla etkileşime girmeden uygulamanın dahili yapısını haritalandırmak ve gözden kaçan giriş noktalarını keşfetmek için düşük çaba gerektiren bir yöntem sağlar.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INFO-05 |
| **CWE** | CWE-200 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Düşük / Orta* |

> *Sabit kodlanmış hassas kimlik bilgileri, özel anahtarlar (private keys) veya dahili ortam değişkenleri keşfedilirse Önem Derecesi "Yüksek" olur.

**Kaynaklar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/05-Review_Web_Page_Content_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**Araçlar:** `Burp Suite (Target/Proxy)`, `Browser Developer Tools`, `SecretFinder`, `grep`, `truffleHog`, `LinkFinder`

### Kaynak kodda hassas bilgiler içeren geliştirici yorumları bulunuyor mu?
- [ ] Hayır — yorum bulunamadı veya yalnızca genel, hassas olmayan yorumlar bulundu  
- [ ] Evet — yorumlar mevcut ancak hassas bir mantık veya veri içermiyor  
- [ ] Evet — yorumlar dahili yolları, SQL sorgularını veya idari talimatları ifşa ediyor  
- [ ] Evet — yorumlar kimlik bilgilerini veya hassas geliştirici notlarını ifşa ediyor *(Yüksek)*  

### Gizli HTML girdi (input) alanları hassas veri veya durum (state) bilgisi içeriyor mu?
- [ ] Hayır — gizli alan bulunamadı veya bunlar yalnızca hassas olmayan jetonlar (tokens) içeriyor  
- [ ] Evet — gizli alanlar durum bilgisi için kullanılıyor ancak şifrelenmiş (encrypted) veya imzalanmış (signed)  
- [ ] Evet — gizli alanlar düz metin (plaintext) parolalar, kullanıcı rolleri veya dahili kimlikler (IDs) içeriyor  

### JavaScript dosyalarında sabit kodlanmış sırlar, API anahtarları veya dahili IP adresleri mevcut mu?
- [ ] Hayır — JS dosyalarında hassas dizgiler (strings) veya sırlar bulunamadı  
- [ ] Evet — JS dosyaları uygun kısıtlamalara sahip genel (public) API anahtarları içeriyor  
- [ ] Evet — JS dosyaları korumasız API anahtarları, dahili IP'ler veya özel uç noktalar (endpoints) içeriyor  
- [ ] Evet — JS dosyaları sabit kodlanmış idari kimlik bilgileri veya özel anahtarlar (private keys) içeriyor *(Kritik)*  

### Uygulama, kaynak kodda uygulama iskeleti (framework) sürümlerini veya dahili dosya yollarını ifşa ediyor mu?
- [ ] Hayır — framework bilgileri ve dosya yolları küçültülmüş (minified) veya karartılmış (obfuscated)  
- [ ] Evet — framework isimleri ve sürümleri görünüyor ancak yamalanmış  
- [ ] Evet — dahili dosya yolları veya mutlak sunucu yolları kaynak kodda ifşa edilmiş  

### Kaynak kod, küçültme (minification) veya karartma (obfuscation) eksikliği nedeniyle sızıntıya eğilimli mi?
- [ ] Hayır — canlı (production) kod tamamen küçültülmüş ve yorumlardan arındırılmış  
- [ ] Evet — kod küçültülmüş ancak yorumlar kaynakta kalmış  
- [ ] Evet — kaynak haritaları (`.map` dosyaları) herkese açık olarak erişilebilir durumda ve kaynak kodun tamamen yeniden oluşturulmasına (reconstruction) olanak tanıyor  

---