## WSTG-INPV-11 — Kod Enjeksiyonu (Code Injection)

Kod enjeksiyonu (Code Injection), bir uygulamanın kullanıcı tarafından sağlanan kod parçacıklarını, genellikle `eval()`, `exec()` veya `system()` gibi güvenli olmayan dile özgü fonksiyonlar aracılığıyla kendi çalışma zamanı ortamında (runtime environment) yürütmesiyle oluşur. Bu zafiyet, işletim sistemi kabuğunu değil, programlama dilinin yürütme bağlamını (örneğin PHP, Python, Node.js) hedeflediği için komut enjeksiyonundan (command injection) farklıdır. Saldırganlar, rastgele mantık yürütmek, ortam değişkenlerini (environment variables) sızdırmak veya sunucu üzerinde tam uzaktan kod yürütme (Remote Code Execution - RCE) elde etmek için bu noktaları istismar ederler. Sızma testi uzmanları (pentesters); matematiksel ifadeleri, sunucu tarafı şablonlarını (server-side templates) veya girdinin yürütülebilir kod olarak yorumlanabileceği dinamik yapılandırma parametrelerini işleyen özellikleri incelemelidir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INPV-11 |
| **CWE** | CWE-94 |
| **Test Durumu** | Yürütülmedi |
| **Önem Derecesi** | Kritik |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11-Testing_for_Code_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/deserialization  
* https://portswigger.net/web-security/llm-attacks  

**Araçlar:** `Burp Suite (Repeater/Intruder)`, `FFUF`, `PayloadsAllTheThings`, `Commix`

### Herhangi bir uygulama özelliği, dile özgü değerlendirme fonksiyonları aracılığıyla dinamik girdileri yürütüyor mu?
- [ ] Hayır — kod tabanında dinamik değerlendirme fonksiyonları **bulunmamaktadır**  
- [ ] Evet — `eval()`, `exec()` veya `include()` gibi fonksiyonlar kullanılmaktadır ancak kullanıcı girdisi **kabul etmemektedir**  
- [ ] Evet — fonksiyonlar kullanılmaktadır ve kullanıcı tarafından sağlanan girdileri **işlemektedir**  

### Değerlendirme öncesinde girdiye girdi doğrulama (validation) veya temizleme (sanitization) mekanizmaları uygulanıyor mu?
- [ ] Evet — girdi, **sabit kodlanmış bir beyaz listeye (whitelist)** karşı sıkı bir şekilde doğrulanmaktadır  
- [ ] Evet — girdi, kara liste (blacklisting) yoluyla temizlenmektedir ancak atlatma (bypass) **mümkündür**  
- [ ] Hayır — girdi doğrudan değerlendirme fonksiyonuna aktarılmaktadır ve **hiçbir kontrol** uygulanmamaktadır  

### Yürütme ortamı, sistem düzeyinde erişimi engellemek için izole edilmiş (sandboxed) veya kısıtlanmış mı?
- [ ] Evet — yürütme, sistem çağrılarının **yapılamadığı** oldukça kısıtlı bir korumalı alanda (sandbox) gerçekleşmektedir  
- [ ] Evet — bazı kısıtlamalar mevcuttur ancak korumalı alandan kaçış (sandbox escape) **mümkündür**  
- [ ] Hayır — ortam, temel dil uygulama programlama arayüzlerine (API) ve işletim sistemi fonksiyonlarına tam erişim sağlamaktadır *(Kritik)*  

### Kod enjeksiyonu (code injection) hassas bilgileri sızdırmak için kullanılabilir mi?
- [ ] Hayır — yürütme kördür (blind) ve bant dışı (out-of-band) iletişim **mümkün değildir**  
- [ ] Evet — ortam değişkenleri veya yerel dosyalar HTTP/DNS istekleri aracılığıyla **sızdırılabilir**  
- [ ] Evet — yürütülen kodun sonuçları doğrudan uygulama yanıtına **yansıtılmaktadır**  

### Uygulama, uzaktan dosya dahil etmeye (Remote File Inclusion - RFI) veya dinamik kütüphane yüklemeye izin veriyor mu?
- [ ] Hayır — yalnızca yerel ve önceden tanımlanmış kod yolları yürütülebilir  
- [ ] Evet — uygulama, uzak veya saldırgan kontrolündeki bir kaynaktan kod yüklemeye ve yürütmeye **zorlanabilir**  

---