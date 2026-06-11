## WSTG-ATHZ-01 — Dizin Gezginliği ve Dosya Dahil Etme Testi (Testing Directory Traversal File Include)

Dizin gezginliği (Directory Traversal) ve dosya dahil etme (File Inclusion) zafiyetleri, bir uygulama kullanıcı tarafından kontrol edilebilen girdileri, yeterli doğrulama veya temizleme (sanitization) yapmadan dosya veya dizin yollarını oluşturmak için kullandığında ortaya çıkar. Saldırganlar, hedeflenen dizinin dışına çıkmak için `../` gibi dizileri enjekte ederek bu açıklardan yararlanır ve hassas sistem dosyalarına, yapılandırma verilerine veya uygulama kaynak koduna erişebilirler. Yerel Dosya Dahil Etme (Local File Inclusion - LFI) veya Uzaktan Dosya Dahil Etme (Remote File Inclusion - RFI) içeren daha ciddi durumlarda, bir saldırgan kötü amaçlı betikler dahil ederek veya günlük zehirlemesi (log poisoning) ve PHP sarmalayıcılarından (wrappers) yararlanarak uzaktan kod yürütme (Exploit) gerçekleştirebilir. Bu zafiyetler genellikle dinamik içerik yükleme, şablon motorları veya sunucu tarafı mantığının dosya sistemi yollarını işlediği görüntü getirme uç noktalarında (endpoints) kullanılan parametrelerde bulunur.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-ATHZ-01 |
| **CWE** | CWE-22 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Kritik |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/01-Testing_Directory_Traversal_File_Include  
* https://hacktricks.wiki/en/pentesting-web/file-inclusion/index.html  
* https://portswigger.net/web-security/file-path-traversal  

**Araçlar:** `Burp Suite`, `FFUF`, `DotDotPwn`, `LFI Suite`, `Wfuzz`

### Parametreler sunucu tarafı işleme için dosya adlarını veya yollarını kabul ediyor mu?
- [ ] Hayır — hiçbir parametrenin dosya sistemiyle etkileşime girdiği görülmemektedir  
- [ ] Evet — parametreler mevcuttur ancak dosya tanımlayıcıları için katı bir izin verilenler listesi (allowlist) kullanmaktadır  
- [ ] Evet — parametreler doğrudan dosya adlarını veya yollarını kabul etmektedir  

### Girdi, dizin gezginliği dizilerini önlemek için temizleniyor (sanitized) mu?
- [ ] Evet — girdi katı bir izin verilenler listesine göre doğrulanmaktadır ve gezginlik dizileri **mümkün değildir**  
- [ ] Evet — girdi `../` dizileri kaldırılarak temizlenmektedir, ancak özyinelemeli (recursive) atlatma **mümkündür**  
- [ ] Hayır — yol ile ilgili girdilere hiçbir temizleme veya doğrulama **uygulanmamaktadır**  

### Gezginlik dizileri aracılığıyla kısıtlanmış dizinin dışındaki dosyalara erişmek mümkün mü?
- [ ] Hayır — uygulama veya işletim sistemi, tanımlanan kapsam dışındaki dosyalara erişimi engellemektedir  
- [ ] Evet — web kök dizini (web root) içindeki dosyalara erişim **mümkündür**  
- [ ] Evet — hassas sistem dosyalarına (örneğin `/etc/passwd`, `C:\Windows\win.ini`) erişim **mümkündür** *(Kritik)*  

### Sunucu, uzak URL'lerin dahil edilmesine (RFI) izin veriyor mu?
- [ ] Hayır — uzaktan dosya dahil etme, sunucu/uygulama düzeyinde **devre dışıdır**  
- [ ] Evet — uzak dosyalar dahil **edilebilir** ancak yürütme (execution) **mümkün değildir**  
- [ ] Evet — uzak dosyalar dahil **edilebilir** ve sunucuda yürütülebilir *(Kritik)*  

### Filtreler, kodlama (encoding) veya özel karakterler kullanılarak atlatılabilir mi?
- [ ] Hayır — filtreler çeşitli kodlamaları ve boş baytları (null bytes) etkili bir şekilde işlemektedir  
- [ ] Evet — atlatma; URL encoding, double URL encoding veya 16-bit Unicode kullanılarak **mümkündür**  
- [ ] Evet — atlatma; boş bayt enjeksiyonu (`%00`) veya dosya sistemi sarmalayıcıları (örneğin `php://filter`) kullanılarak **mümkündür**  

---