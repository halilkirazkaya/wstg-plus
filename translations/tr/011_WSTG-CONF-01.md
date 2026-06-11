## WSTG-CONF-01 — Ağ Altyapı Yapılandırmasının Test Edilmesi

Ağ altyapı yapılandırma testi, web uygulamasını destekleyen temel sunucu ve ağ bileşenlerindeki zayıflıkların tanımlanmasını içerir. Saldırganlar; yer edinmek, yatayda hareket etmek veya keşif yapmak için yanlış yapılandırılmış servisleri, güncelliğini yitirmiş protokolleri ve gereksiz açık portları hedef alırlar. Yaygın bulgular arasında güvensiz TLS sürümleri, ayrıntılı servis başlıkları (verbose service banners) ve dahili ağlarla sınırlandırılması gereken açık yönetim arayüzleri yer alır. Bu altyapı düzeyi kusurların istismar edilmesi (Exploit) yoluyla, bir saldırgan iletim halindeki verilerin gizliliğini tehlikeye atabilir veya ana işletim sistemine yetkisiz erişim sağlayabilir.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-CONF-01 |
| **CWE** | CWE-16 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Düşük / Orta |

**Kaynaklar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/01-Test_Network_Infrastructure_Configuration  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/pentesting-methodology.html  

**Araçlar:** `nmap`, `sslyze`, `testssl.sh`, `Nikto`, `OpenVAS`, `Nessus`

### Uygulama sunucusunda gereksiz açık portlar veya dışarıya açık servisler bulunuyor mu?
- [ ] Hayır — yalnızca gerekli portlara (örneğin, 443) **erişilebilir**  
- [ ] Evet — ek servisler dışarıya açıktır ancak **güvenli** bir yapılandırmaya sahiptir  
- [ ] Evet — temel olmayan servisler (örneğin FTP, Telnet, SMB) halka açık şekilde **maruz bırakılmıştır**  

### Servis başlıkları (banners) veya header bilgileri hassas sürüm bilgisi sızdırıyor mu?
- [ ] Hayır — başlıklar kaldırılmış veya geneldir  
- [ ] Evet — başlıklar sunucu yazılımını (örneğin Apache/nginx) gösteriyor ancak sürüm numaralarını **göstermiyor**  
- [ ] Evet — ayrıntılı başlıklar belirli yazılım sürümlerini ifşa ederek **istismarı (exploitation)** kolaylaştırıyor  

### TLS yapılandırması modern güvenlik en iyi uygulamalarını takip ediyor mu?
- [ ] Evet — güçlü şifreleme paketleri (cipher suites) ile yalnızca TLS 1.2/1.3 **etkindir**  
- [ ] Evet — eski protokoller (TLS 1.0/1.1) **etkindir**  
- [ ] Hayır — zayıf şifrelemeler veya güvensiz protokoller (SSLv2/v3) **etkindir**  

### Yönetim veya kontrol arayüzleri yetkili ağlarla sınırlandırılmış mı?
- [ ] Evet — arayüzlere (örneğin SSH, RDP, kontrol panelleri) halka açık internetten **erişilemez**  
- [ ] Hayır — arayüzlere halka açık şekilde **erişilebilir** ancak çok faktörlü kimlik doğrulama (MFA) gerektirir  
- [ ] Hayır — arayüzlere tek faktörlü kimlik doğrulama veya varsayılan kimlik bilgileri (default credentials) ile halka açık şekilde **erişilebilir**  

---