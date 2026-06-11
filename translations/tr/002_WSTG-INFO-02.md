## WSTG-INFO-02 — Web Sunucusu Parmak İzi Belirleme (Fingerprint Web Server)

Web sunucusu parmak izi belirleme (Fingerprinting), hedef bir web sunucusunun belirli yazılım türünü, versiyonunu ve temel işletim sistemini tanımlama sürecidir. Saldırganlar, Apache, Nginx, IIS veya diğer sunucu teknolojilerinin belirli sürümlerine özgü yamalanmamış CVE'ler veya yapılandırma zayıflıkları gibi bilinen güvenlik açıklarını belirlemek için bu keşif (Reconnaissance) işlemini gerçekleştirirler. Bu işlem genellikle HTTP yanıt başlıklarının (örneğin; `Server`, `X-Powered-By`) analiz edilmesi, benzersiz protokol davranışlarının incelenmesi veya varsayılan hata sayfaları ve dosyalar aracılığıyla sızan bilgilerin gözlemlenmesiyle gerçekleştirilir. Başarılı bir parmak izi belirleme, saldırganın istismar (Exploit) stratejisini uyarlamasına, genel taramalardan bilinen mimari kusurlara yönelik yüksek olasılıklı saldırılara geçmesine olanak tanır.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INFO-02 |
| **CWE** | CWE-200 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Bilgi Düzeyi / Düşük* |

> *Parmak izi belirleme sonucunda bilinen kritik güvenlik açıklarına sahip, güncel olmayan veya kullanım ömrü dolmuş (EOL) bir sunucu sürümü ortaya çıkarsa önem derecesi "Yüksek" olur.

**Kaynaklar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/02-Fingerprint_Web_Server  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**Araçlar:** `curl`, `Nmap`, `WhatWeb`, `Wappalyzer`, `Nikto`, `Netcat`

### HTTP yanıt başlıklarında tanımlayıcı dizeler mevcut mu?
- [ ] Hayır — `Server` ve `X-Powered-By` başlıkları **mevcut değil** veya genel değerler içeriyor  
- [ ] Evet — başlıklar sunucu yazılımını açığa çıkarıyor ancak spesifik sürüm numaralarını **içermiyor**  
- [ ] Evet — başlıklar belirli sunucu yazılımını ve **tam** sürüm numaralarını açığa çıkarıyor  

### Varsayılan hata sayfaları veya sistem tarafından oluşturulan yanıtlar sunucu ayrıntılarını sızdırıyor mu?
- [ ] Hayır — özel hata sayfaları kullanılıyor ve sunucu bilgilerini **ifşa etmiyor**  
- [ ] Evet — varsayılan hata sayfaları sunucu yazılımı adını ve/veya sürümünü sızdırıyor  
- [ ] Evet — hata sayfaları sunucu ayrıntılarını, sürümünü ve **temel işletim sistemini** sızdırıyor  

### Sunucu sürümü varsayılan dosyalar veya dizin yapıları aracılığıyla ifşa ediliyor mu?
- [ ] Hayır — varsayılan kurulum dosyaları, kılavuzlar ve test betikleri **kaldırılmış**  
- [ ] Evet — varsayılan dosyalar (örneğin; `info.php`, `manual/`, `test.html`) **mevcut** ve sürüm verilerini ifşa ediyor  

### Sunucu türü benzersiz davranış analizi yoluyla doğru bir şekilde tahmin edilebiliyor mu?
- [ ] Hayır — sunucu yanıtları normalize edilmiş ve davranış tabanlı parmak izi belirleme (Fingerprinting) **mümkün değil**  
- [ ] Evet — benzersiz davranışlar (örneğin; başlık sıralaması, belirli hata kodları veya TCP/IP yığını tuhaflıkları) sunucu tanımlamasına **izin veriyor**  

### Tanımlanan sunucu sürümü şu anda destekleniyor ve yamalanmış mı?
- [ ] Evet — tanımlanan sürüm güncel ve satıcı tarafından **destekleniyor**  
- [ ] Hayır — tanımlanan sürüm **güncel değil** veya bilinen güvenlik açıklarına sahip **kullanım ömrü dolmuş (EOL)** bir sürüm  

---