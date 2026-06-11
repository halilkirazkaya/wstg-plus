## WSTG-INFO-01 — Bilgi Sızıntısı İçin Arama Motoru Keşfi ve Keşif Çalışması Yürütülmesi

Arama motoru keşfi (Search engine discovery), hedef kuruluşun yanlışlıkla ifşa ettiği bilgileri tanımlamak için halka açık arama motorlarından, önbelleğe alınmış sayfalardan ve indeksleme hizmetlerinden yararlanmayı içerir. Saldırganlar, halka açık olmaması gereken hassas dosyaları, dizinleri, giriş portallarını, hata mesajlarını ve meta verileri keşfetmek için gelişmiş arama operatörlerini (`Google Dorks`) ve üçüncü taraf indeksleme hizmetlerini kullanırlar. Bu keşif (reconnaissance) aşaması kritiktir; çünkü hedefle doğrudan etkileşim gerektirmez, bu da onu neredeyse tespit edilemez kılarken açıkta kalan yönetim panelleri, yapılandırma dosyaları, veritabanı dökümleri (database dumps) ve dahili belgeler gibi yüksek değerli giriş noktalarını potansiyel olarak ortaya çıkarabilir.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INFO-01 |
| **CWE** | CWE-200 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/01-Conduct_Search_Engine_Discovery_Reconnaissance_for_Information_Leakage  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/external-recon-methodology/index.html  
* https://portswigger.net/web-security/information-disclosure  

**Araçlar:** `Google Dorks`, `theHarvester`, `Shodan`, `Censys`, `Wayback Machine`, `Recon-ng`, `SearchDiggity`

### Hassas dosyalar veya dizinler arama motorları tarafından indekslenmiş mi?
- [ ] Hayır — arama motoru sonuçlarında hassas içerik bulunmadı  
- [ ] Evet — yapılandırma dosyaları, yedekler veya dahili belgeler indekslenmiş *(Kritik)*  

### Google Dorking çalışması açıkta kalan yönetim veya giriş arayüzlerini ortaya çıkarıyor mu?
- [ ] Hayır — yönetici panelleri ve giriş sayfaları indekslenmemiş  
- [ ] Evet — yönetici panelleri indekslenmiş ancak güçlü çok faktörlü kimlik doğrulama gerektiriyor  
- [ ] Evet — yönetici panelleri indekslenmiş ve yeterli kontroller olmaksızın halka açık durumda  

### Sayfaların önbelleğe alınmış sürümleri, o zamandan beri kaldırılmış olan hassas verileri ifşa ediyor mu?
- [ ] Hayır — önbelleğe alınmış anlık görüntülerde hassas veri bulunmadı  
- [ ] Evet — önbelleğe alınmış sayfalar kimlik bilgileri, dahili IP adresleri veya hassas bilgiler içeriyor  

### `robots.txt` dosyası istenmeden hassas yolları ifşa ediyor mu?
- [ ] Hayır — `robots.txt` hassas dizin yapılarını ortaya çıkarmıyor  
- [ ] Evet — `robots.txt` saldırgan keşif çalışmasına yardımcı olan hassas yolları listeliyor  
- [ ] `robots.txt` dosyası mevcut değil  

### Üçüncü taraf hizmetler (`Shodan`, `Censys`, `Wayback Machine`) geçmişteki veya mevcut maruziyeti ortaya çıkarıyor mu?
- [ ] Hayır — üçüncü taraf indeksleme hizmetlerinden önemli bir bulgu elde edilmedi  
- [ ] Evet — geçmişe dönük anlık görüntüler veya servis taramaları hassas meta verileri veya servis banner bilgilerini (service banners) ortaya çıkarıyor  

---