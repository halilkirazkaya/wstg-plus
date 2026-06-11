## WSTG-INPV-19 — Sunucu Taraflı İstek Sahteciliği (Server-Side Request Forgery - SSRF) Testi

Sunucu Taraflı İstek Sahteciliği (Server-Side Request Forgery - SSRF), bir saldırganın web uygulamasını, sunucudan dahili veya harici kaynaklara yetkisiz istekler yapması için etkileyebildiği durumlarda ortaya çıkar. URL, ana makine adları (hostnames) veya IP adresleri bekleyen parametreleri manipüle ederek, saldırganlar güvenlik duvarları (firewalls) ve ACL'ler gibi ağ düzeyi kontrolleri atlayarak dahili ağ segmentlerini tarayabilir, bulut meta veri servislerine (IMDS) erişebilir veya savunmasız dahili API'ler ve veritabanları ile etkileşime girebilir. Bu zafiyet genellikle görsel getirme, PDF oluşturma, webhook'lar veya URL üzerinden dosya yükleme içeren özelliklerde kendini gösterir. Saldırgan perspektifinden SSRF; izole ortamlara sızmak (pivot), hassas yapılandırma verilerini dışarı sızdırmak (exfiltrate) veya Redis ya da Memcached gibi dahili servislerle etkileşime girerek potansiyel olarak uzaktan kod çalıştırma (Remote Code Execution - RCE) elde etmek için kullanılan güçlü bir temel öğedir (primitive).


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INPV-19 |
| **CWE** | CWE-918 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Kritik |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/19-Testing_for_Server-Side_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/ssrf-server-side-request-forgery/index.html  
* https://portswigger.net/web-security/ssrf  

**Araçlar:** `Burp Suite (Collaborator/Interactsh)`, `Interact.sh`, `Gopherus`, `ffuf`, `cURL`, `DNSRebind Tool`

### Uygulama kullanıcı tarafından sağlanan URL'leri veya ana makine adlarını işliyor mu?
- [ ] Hayır — harici URL veya ana makine adlarını kabul eden bir özellik bulunmamaktadır  
- [ ] Evet — özellik mevcuttur ancak girdi bir izin listesine (allow-list) göre sıkı bir şekilde doğrulanmaktadır  
- [ ] Evet — özellik mevcuttur ve kullanıcı tarafından sağlanan URL'leri kabul etmektedir  

### İstenen hedef için doğrulama kontrolleri veya kara listeler uygulanıyor mu?
- [ ] Evet — güvenilir alan adlarından/IP'lerden oluşan sıkı bir izin listesi (allow-list) zorunlu tutulmaktadır  
- [ ] Evet — bir engelleme listesi (deny-list) kullanılmaktadır (örneğin; `127.0.0.1`, `169.254.169.254` adreslerini engellemek gibi)  
- [ ] Hayır — girdiye herhangi bir doğrulama veya filtreleme uygulanmamaktadır  

### Kodlama, yönlendirmeler veya DNS yeniden bağlama yoluyla filtreleri atlamak mümkün müdür?
- [ ] Hayır — filtreler güçlüdür ve yönlendirmeleri/DNS çözümlemesini güvenli bir şekilde işlemektedir  
- [ ] Evet — URL kodlaması (encoding) veya alternatif IP formatları (hex, octal) kullanılarak atlatma mümkündür  
- [ ] Evet — dahili hedeflere yönelik 3xx HTTP yönlendirmeleri üzerinden atlatma mümkündür  
- [ ] Evet — dahili IP'leri çözümlemek için DNS yeniden bağlama (DNS rebinding) saldırılarıyla atlatma mümkündür  

### Uygulama dahili meta veri servislerine veya yerel arayüzlere ulaşabiliyor mu?
- [ ] Hayır — yerel ağ ve bulut meta veri uç noktalarına (endpoints) ulaşılamamaktadır  
- [ ] Evet — localhost (`127.0.0.1`) veya geri döngü (loopback) servislerine erişim mümkündür  
- [ ] Evet — bulut meta veri servislerine (örneğin; AWS/GCP/Azure IMDS) erişim mümkündür *(Kritik)*  

### SSRF yanıtının niteliği nedir?
- [ ] Blind (Kör) — kullanıcıya herhangi bir yanıt veya meta veri döndürülmez  
- [ ] Partial (Kısmi) — kullanıcıya yanıt meta verileri (başlıklar, boyut, zamanlama) döndürülür  
- [ ] Full (Tam) — dahili kaynaktan gelen tam yanıt gövdesi (response body) kullanıcıya yansıtılır  

---