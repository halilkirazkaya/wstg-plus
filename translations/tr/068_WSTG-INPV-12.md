## WSTG-INPV-12 — Command Injection

Command Injection (Komut Enjeksiyonu), bir uygulamanın form girişleri, HTTP başlıkları veya çerezler gibi güvenli olmayan kullanıcı verilerini bir sistem kabuğuna (shell) iletmesi durumunda ortaya çıkar. Bu zafiyet, bir saldırganın sunucu üzerinde, genellikle web uygulaması sürecinin yetkileriyle rastgele işletim sistemi komutları yürütmesine olanak tanır. Saldırgan açısından bu durum; noktalı virgül, pipe veya backtick gibi shell meta karakterleri kullanarak zararlı komutları hedeflenen yürütme akışına zincirleme yoluyla istismar edilir (Exploit). Etkisi genellikle yıkıcıdır; tam sistem ele geçirme, yetkisiz veri sızdırma veya iç ağda yanal hareket (Lateral Movement) ile sonuçlanabilir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INPV-12 |
| **CWE** | CWE-78 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Kritik |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/12-Testing_for_Command_Injection  
* https://hacktricks.wiki/en/pentesting-web/command-injection.html  
* https://portswigger.net/web-security/os-command-injection  

**Araçlar:** `Commix`, `Burp Suite (Intruder/Repeater)`, `dnsbin`, `Interactsh`, `Collaborator`

### Uygulama, sistem düzeyinde komutlar veya shell yürütülebilir dosyaları çağırıyor mu?
- [ ] Hayır — uygulama işletim sistemi kabuğu (shell) ile etkileşime **girmiyor**  
- [ ] Evet — uygulama, sistem görevleri için dahili API'ler veya üst düzey kütüphaneler kullanıyor  
- [ ] Evet — uygulama; `system()`, `exec()` veya `popen()` gibi sistem çağrıları (system calls) aracılığıyla shell komutlarını doğrudan çağırıyor  

### Kullanıcı girdisi, sistem çağrılarına (system calls) iletilmeden önce doğrulanıyor mu veya temizleniyor mu?
- [ ] Evet — sıkı izin listesi (allow-listing) ve girdi doğrulaması (Input Validation) **uygulanıyor**  
- [ ] Evet — temizleme (sanitization) veya kaçış karakteri (escaping) kullanımı mevcut ancak bypass **mümkün**  
- [ ] Hayır — kullanıcı girdisi doğrulama **olmadan** doğrudan shell'e iletiliyor  

### Komut yürütme akışını manipüle etmek için shell meta karakterleri kullanılabilir mi?
- [ ] Hayır — meta karakterler doğru bir şekilde filtreleniyor veya kaçış karakteri ile etkisiz hale getiriliyor  
- [ ] Evet — komut çıktısının yanıta yansıdığı bant içi (in-band) yürütme **mümkündür**  
- [ ] Evet — hiçbir çıktının yansımadığı kör (blind) yürütme **mümkündür**  

### Ana makineden bant dışı (OOB) veri sızdırma (exfiltration) mümkün mü?
- [ ] Hayır — sunucunun dışa giden (egress) bağlantısı yok veya OOB sinyalleri (DNS/HTTP) başarısız oluyor  
- [ ] Evet — DNS veya HTTP tabanlı OOB sinyalleri aracılığıyla veri sızdırma **mümkündür**  

### Uygulama süreci yüksek sistem yetkileriyle mi çalışıyor?
- [ ] Hayır — uygulama özel, düşük yetkili bir servis hesabı ile çalışıyor  
- [ ] Evet — uygulama `root`, `Administrator` veya yüksek yetkili bir kullanıcı olarak çalışıyor *(Kritik)*  

---