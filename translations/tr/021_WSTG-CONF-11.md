## WSTG-CONF-11 — Bulut Depolama Testi

Bulut depolama hatalı yapılandırmalarına (misconfigurations) yönelik testler; Amazon S3, Azure Blobs veya Google Cloud Storage gibi genel erişime açık nesne depolama servislerinin (object storage services) tanımlanmasını ve denetlenmesini içerir. Bu kaynaklar genellikle aşırı izinli Erişim Kontrol Listeleri (ACLs) veya Kimlik ve Erişim Yönetimi (IAM) politikaları nedeniyle sızdırılan; yedeklemeler, kaynak kodlar, Kişisel Veriler (PII) veya yapılandırma dosyaları gibi hassas veriler içerir. Saldırganlar, veri sızdırma (data exfiltration) veya yetkisiz dosya değişikliği için giriş noktaları bulmak amacıyla JavaScript dosyaları, HTML kaynak kodları ve Brute Force teknikleri aracılığıyla keşif yaparak bucket isimlerini numaralandırır (enumerate). İstismar (Exploit) süreci, tam veri ihlalleri veya güvenilir bir alan adından zararlı dosyaların servis edilmesi dahil olmak üzere önemli etkilere yol açabilir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CONF-11 |
| **CWE** | CWE-922 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Kritik* |

> *PII, kimlik bilgileri veya üretim yedekleri erişilebilirse ya da kimlik doğrulaması gerektirmeyen yazma erişimi etkinse önem derecesi Kritik olur.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/11-Test_Cloud_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `aws-cli`, `gsutil`, `azure-cli`, `S3Scanner`, `CloudEnum`, `FFUF`, `Burp Suite`

### Bulut depolama uç noktaları (buckets/containers) tanımlanmış ve numaralandırılmış mı?
- [ ] Hayır — herhangi bir bulut depolama uç noktası kullanılmıyor veya keşfedilemiyor  
- [ ] Evet — depolama uç noktaları kod analizi veya trafik izleme yoluyla tanımlandı  
- [ ] Evet — depolama uç noktaları adlandırma kuralı (naming convention) brute-force yöntemiyle keşfedildi  

### Kimlik doğrulaması yapılmamış kullanıcılar bulut depolama içeriğini listeleyebiliyor mu?
- [ ] Hayır — bucket listeleme **devre dışı** ve 403 Forbidden veya 404 Not Found döndürüyor  
- [ ] Evet — listeleme **etkin** ancak hassas dosya bulunmuyor  
- [ ] Evet — listeleme **etkin** ve hassas dosya isimleri/metadataları görünüyor  

### Tanımlanan bucket'lardan hassas dosyaların indirilmesi mümkün mü?
- [ ] Hayır — dosyalar IAM politikaları ile korunuyor veya geçerli bir kimlik doğrulaması gerekiyor  
- [ ] Evet — dosyalara erişilebiliyor ancak kolayca tahmin edilemeyen imzalı bir URL (signed URL) gerekiyor  
- [ ] Evet — hassas dosyalar (yedeklemeler, `.env`, PII) indirme için **genel erişime açık** *(Kritik)*  

### Kimlik doğrulaması yapılmamış dosya yükleme veya değişikliğine izin veriliyor mu?
- [ ] Hayır — kimlik doğrulaması yapılmamış yüklemeler veya değişiklikler **mümkün değil**  
- [ ] Evet — kimlik doğrulamalı yükleme gerekiyor ancak zayıf politika koşulları nedeniyle **atlatma (bypass) mümkün**  
- [ ] Evet — dosyaların kimlik doğrulaması olmadan yazılması, üzerine yazılması veya silinmesi **mümkün** *(Kritik)*  

### Depolama uç noktasındaki Cross-Origin Resource Sharing (CORS) politikaları kısıtlayıcı mı?
- [ ] Evet — CORS **devre dışı** veya belirli güvenilir kaynaklarla (origins) kısıtlanmış  
- [ ] Hayır — CORS, bir joker karakter `*` kaynağı ile **etkinleştirilmiş**, bu da yetkisiz web tabanlı erişime izin veriyor  

---