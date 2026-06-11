## WSTG-CLNT-08 — Cross-Site Flashing Testi

Cross-Site Flashing (XSF), bir Flash (SWF) dosyasının kullanıcı tarafından kontrol edilen girdileri uygun olmayan şekilde işlemesi sonucunda, saldırganın kötü amaçlı ActionScript kodu yürütmesine veya tarayıcının JavaScript ortamına köprü kurmasına olanak tanıyan istemci tarafı (client-side) bir zafiyettir. `ExternalInterface.call`, `getURL` veya `loadMovie` gibi sink noktalarına aktarılan `FlashVars` veya URL sorgu dizeleri (query strings) gibi parametrelerin manipüle edilmesiyle, bir saldırgan oturum ele geçirme (session hijacking) ve veri sızdırma (data exfiltration) dahil olmak üzere savunmasız alan adı (domain) bağlamında eylemler gerçekleştirebilir. Bu zafiyet, temel olarak hâlâ SWF dosyalarını barındıran eski kurumsal uygulamalarda bulunur; burada yetersiz girdi doğrulaması, Flash dosyasının Cross-Site Scripting (XSS) için bir vektör olarak yeniden kullanılmasına veya izin veren çapraz etki alanı (cross-domain) yapılandırmaları aracılığıyla Aynı Köken Politikası'nın (Same-Origin Policy - SOP) atlatılmasına izin verir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CLNT-08 |
| **CWE** | CWE-79 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek |

**Kaynaklar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/08-Testing_for_Cross_Site_Flashing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `JPEXS Free Flash Decompiler`, `FFDec`, `Burp Suite`, `Google Dorks`, `strings`

### Uygulamada eski Adobe Flash (.swf) dosyaları barındırılıyor mu?
- [ ] Hayır — uygulama kapsamında hiçbir Flash dosyası barındırılmıyor veya referans verilmiyor  
- [ ] Evet — SWF dosyaları mevcut ancak yalnızca statik içerik sağlıyor  
- [ ] Evet — SWF dosyaları mevcut ve `FlashVars` veya URL dizeleri aracılığıyla dinamik parametreleri kabul ediyor  

### ActionScript sink noktalarına aktarılan girdiler temizleniyor mu?
- [ ] Evet — tüm girdiler sıkı bir şekilde doğrulanıyor ve ActionScript sink noktaları manipüle edilemiyor  
- [ ] Evet — doğrulama uygulanıyor ancak belirli kodlama teknikleri ile atlatma (bypass) mümkün  
- [ ] Hayır — girdi doğrudan `ExternalInterface.call` veya `getURL` gibi hassas sink noktalarına aktarılıyor *(Kritik)*  

### Flash dosyası, rastgele JavaScript (XSS) yürütmek için kullanılabilir mi?
- [ ] Hayır — `ExternalInterface` devre dışı bırakılmış veya `allowScriptAccess` parametresi `never` olarak ayarlanmış  
- [ ] Evet — `allowScriptAccess` parametresi `sameDomain` olarak ayarlanmış ancak SWF hedef alan adında barındırılıyor  
- [ ] Evet — `allowScriptAccess` parametresi `always` olarak ayarlanmış ve herhangi bir etki alanından XSS yapılmasına izin veriyor  

### `crossdomain.xml` politikası, yetkisiz çapraz köken (cross-origin) isteklerini engelliyor mu?
- [ ] Evet — `crossdomain.xml` kısıtlayıcıdır ve yalnızca güvenilen, belirli kökenlere izin verir  
- [ ] Hayır — `crossdomain.xml` dosyası eksik  
- [ ] Evet — politika aşırı derecede izin vericidir (örneğin, `<allow-access-from domain="*" />`)  

### SWF dosyası, saldırgan kontrollü harici dosyaları yüklemeye zorlanabilir mi?
- [ ] Hayır — uygulama, harici SWF dosyalarını yüklemeye zorlanamaz  
- [ ] Evet — `loadMovie` veya `loadMovieNum` fonksiyonları doğrulanmamış harici URL'leri kabul ediyor  

---