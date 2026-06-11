## WSTG-INPV-17 — Host Header Enjeksiyonu Testi (Testing for Host Header Injection)

Host Header Enjeksiyonu (Host Header Injection), bir uygulamanın kullanıcı tarafından sağlanan `Host` başlığına güvenmesi ve bu başlığı yeterli doğrulama yapmadan bağlantı oluşturma veya yönlendirme gibi sunucu tarafı mantığına dahil etmesi durumunda meydana gelir. Saldırganlar; web önbellek zehirlenmesini (web cache poisoning) kolaylaştırmak, hassas belirteçleri (sensitive tokens) harici bir alana yönlendirerek parola sıfırlama zehirlenmesine (password reset poisoning) olanak tanımak veya başlık tabanlı yönlendirmeye dayanan güvenlik kontrollerini atlatmak amacıyla bu başlığı manipüle eder. Başarılı bir istismar, saldırganın oturum verilerini sızdırmasına, hesap ele geçirme (account takeover) işlemlerini gerçekleştirmesine veya şüphelenmeyen kullanıcıları kötü niyetli altyapılara yönlendirmesine izin verir. Bu güvenlik açığı, en yaygın olarak yük dengelemeli ortamlarda veya gelen istek bağlamına göre dinamik olarak mutlak URL'ler (absolute URLs) oluşturan uygulamalarda bulunur.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INPV-17 |
| **CWE** | CWE-20 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek* |

> *Host başlığı, parola sıfırlama gibi e-postalarda hassas bağlantılar oluşturmak için kullanılıyorsa Önem Derecesi Yüksek (High) olur.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/17-Testing_for_Host_Header_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/host-header  

**Araçlar:** `Burp Suite (Repeater/Collaborator)`, `curl`, `Param Miner`, `nmap (http-vhosts)`

### Uygulama `Host` başlığı değerini bağlantılarda, yönlendirmelerde veya betiklerde yansıtıyor mu?
- [ ] Hayır — uygulama sabit kodlanmış (hardcoded) temel URL'ler veya sıkı bir şekilde doğrulanmış yapılandırma kullanıyor  
- [ ] Evet — `Host` başlığı yansıtılıyor ancak bir beyaz listeye (whitelist) göre sıkı bir şekilde doğrulanıyor  
- [ ] Evet — `Host` başlığı yansıtılıyor ve hatalı yapılandırılmış değerler (örneğin, `beklenen.com:saldirgan.com`) aracılığıyla atlatma (bypass) **mümkün**  
- [ ] Evet — `Host` başlığı herhangi bir doğrulama **olmadan** yansıtılıyor  

### Uygulama veya altyapı tarafından alternatif ana makine (host) ile ilgili başlıklar işleniyor mu?
- [ ] Hayır — `X-Forwarded-Host`, `X-Host` veya `Forwarded` gibi başlıklar yoksayılıyor  
- [ ] Evet — alternatif başlıklar işleniyor ancak dahili mantığı (internal logic) **geçersiz kılmıyor**  
- [ ] Evet — alternatif başlıklar, birincil `Host` başlığı değerini geçersiz kılmak için **kullanılabiliyor**  

### `Host` başlığı, sistem tarafından oluşturulan e-postaları (örneğin parola sıfırlama) zehirlemek için manipüle edilebilir mi?
- [ ] Hayır — e-posta bağlantıları, sunucu yapılandırmasındaki statik ve güvenilir bir alanı kullanıyor  
- [ ] Evet — `Host` başlığı e-posta bağlantılarını etkiliyor ancak doğrulama **atılatılamıyor**  
- [ ] Evet — `Host` başlığı, parola sıfırlama belirteçlerini saldırgan kontrolündeki bir alana yönlendirmek için manipüle **edilebiliyor** *(Kritik)*  

### Uygulama, `Host` başlığı aracılığıyla Web Önbellek Zehirlenmesine (Web Cache Poisoning) karşı savunmasız mı?
- [ ] Hayır — herhangi bir önbellekleme mekanizması mevcut değil veya `Host` başlığı önbellek anahtarının (cache key) bir parçası  
- [ ] Evet — bir önbellek mevcut ancak anahtarlanmamış girdiler (unkeyed inputs) için `Host` başlığını sıkı bir şekilde doğruluyor veya yoksayıyor  
- [ ] Evet — bir önbellek mevcut ve diğer kullanıcılara kötü niyetli içerik sunmak için enjekte edilmiş bir `Host` başlığı ile **zehirlenebiliyor**  

### Sunucu, sanal ana makine yönlendirmesi (virtual host routing) için rastgele `Host` başlıklarını kabul ediyor mu?
- [ ] Hayır — sunucu, tanınmayan `Host` başlıkları için bir hata veya varsayılan bir sayfa döndürüyor  
- [ ] Evet — sunucu her türlü `Host` başlığını kabul ediyor; bu durum keşif (reconnaissance) veya dahili servis numaralandırma (internal service enumeration) için **kullanışlıdır**  

---