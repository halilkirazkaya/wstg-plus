## WSTG-INFO-04 — Web Sunucusundaki Uygulamaların Numaralandırılması

Uygulama numaralandırma (Application Enumeration), tek bir web sunucusu veya IP adresi üzerinde barındırılan tüm web uygulamalarını ve servislerini tanımlama sürecidir. Modern web sunucuları genellikle birden fazla sanal ana makine (Virtual Host), eski hazırlık ortamları (Staging Environments) veya standart dışı portlarda ve yollarda yönetim arayüzleri barındırdığından, bu gizli varlıkların tanımlanamaması saldırı yüzeyinin (Attack Surface) önemli bir kısmının test edilmeden kalmasına neden olur. Saldırganlar, birincil üretim uygulamasından genellikle daha az güvenli olan bu unutulmuş veya gizlenmiş giriş noktalarını keşfetmek için DNS bölge transferleri (DNS zone transfers), sanal ana makine kaba kuvvet saldırıları (Virtual Host Brute-Forcing) ve ters IP aramaları (Reverse IP Lookups) gibi teknikleri kullanırlar.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INFO-04 |
| **CWE** | CWE-200 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Bilgi Düzeyi / Düşük |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/04-Attack_Surface_Identification  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/external-recon-methodology/index.html  

**Araçlar:** `nmap`, `ffuf`, `gobuster`, `theHarvester`, `Dig`, `DNSrecon`, `Reverse IP Lookups`, `Shodan`

### Hedef altyapı ile ilişkili birden fazla IP adresi veya DNS takma adı (Alias) mevcut mu?
- [ ] Hayır — yalnızca tek bir IP ve A-kaydı (A-record) mevcut  
- [ ] Evet — birden fazla IP adresi veya takma ad tespit edildi, kapsam genişletildi  

### Web sunucusu aynı IP üzerinde birden fazla Sanal Ana Makine (vhosts) barındırıyor mu?
- [ ] Hayır — sunucu tarafından yalnızca birincil alan adı (domain) sunulmaktadır  
- [ ] Evet — ek sanal ana makineler tespit edildi ancak erişim **mümkün değil** (ör. 403 Forbidden)  
- [ ] Evet — gizli, geliştirme veya hazırlık (staging) sanal ana makineleri tespit edildi ve erişilebilir durumda  

### Uygulamalar standart dışı portlarda mı barındırılıyor?
- [ ] Hayır — sadece standart portlar (80/443) açık  
- [ ] Evet — standart dışı portlar açık ancak web uygulaması **barındırmıyor**  
- [ ] Evet — standart dışı portlarda ek web uygulamaları tespit edildi (ör. 8080, 8443, 9000)  

### Farklı uygulamalar belirli URI yollarına (URI paths) mı atanmış?
- [ ] Hayır — tek bir uygulama tüm kök dizine (root directory) hizmet vermektedir  
- [ ] Evet — dizin numaralandırma (Directory Enumeration) aracılığıyla birden fazla uygulama keşfedildi (ör. `/api`, `/portal`, `/backup`)  

### Üçüncü taraf keşif (Reconnaissance) servisleri geçmişe ait veya ikincil uygulamaları ortaya çıkarıyor mu?
- [ ] Hayır — Shodan, Censys veya Wayback Machine üzerinden anlamlı bir bulgu elde edilmedi  
- [ ] Evet — geçmişe dönük anlık görüntüler (snapshots) veya servis taramaları, şu anda aktif olan ancak bağlantısı bulunmayan uygulamaları ortaya çıkarıyor  

---