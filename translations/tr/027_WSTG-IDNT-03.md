## WSTG-IDNT-03 — Hesap Hazırlama Sürecinin Test Edilmesi (Account Provisioning Process)

Hesap hazırlama (Account Provisioning) süreci; yeni kimliklerin nasıl oluşturulduğunu, nasıl doğrulandığını ve bir uygulama ortamında yetkilerin nasıl atandığını yönetir. Saldırganlar; spam veya hizmet dışı bırakma (denial-of-service) saldırıları için otomatik kitlesel kayıtlar gerçekleştirmek, sahte hesaplar oluşturmak amacıyla kimlik doğrulama adımlarını atlatmak veya kayıt aşamasında kendilerine yüksek yetkiler tanımlamak için parametreleri manipüle etmek amacıyla bu iş akışını hedeflerler. Zafiyetler genellikle; iş mantığı (business logic) hatalarının yetkisiz hesap oluşturulmasına veya yetki yükseltmeye (privilege escalation) izin verdiği kendi kendine kayıt formlarında, yalnızca davetle girilen sistemlerde veya yönetici hazırlama konsollarında ortaya çıkar. Bir saldırganın bakış açısından, hazırlama akışının ele geçirilmesi; daha derin sömürü, veri sızdırma (data exfiltration) veya sosyal mühendislik saldırıları için bir basamak olarak kullanılabilecek meşru bir dayanak noktası sağlar.


| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-IDNT-03 |
| **CWE** | CWE-285 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta / Yüksek* |

> *Bir saldırganın yönetici hesapları oluşturabilmesi veya genel kayıt kısıtlamalarını atlatabilmesi durumunda Önem Derecesi Kritik (Critical) seviyesine yükselir.

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/03-Test_Account_Provisioning_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite`, `FFUF`, `Postman`, `Python`, `MailHog`

### Kendi kendine kayıt işlemi sıkı bir şekilde kontrol ediliyor mu veya yetkili kullanıcılarla sınırlandırılmış mı?
- [ ] Evet — kayıt işlemi **devre dışı** bırakılmış veya manuel yönetici onayı gerektiriyor  
- [ ] Evet — kayıt işlemi açık ancak **yetkili** e-posta alan adları veya davetiye belirteçleri (tokens) ile sınırlandırılmış  
- [ ] Hayır — kayıt işlemi herhangi bir alan adı veya belirteç kısıtlaması olmaksızın tüm kullanıcılara **açık**  

### Hesap aktivasyonundan önce güçlü bir kimlik doğrulama mekanizması (örneğin, e-posta/SMS) gerekiyor mu?
- [ ] Evet — doğrulama **zorunlu** ve **atılatılamaz**  
- [ ] Evet — doğrulama **zorunlu** ancak parametre kurcalama (parameter tampering) veya tahmin edilebilir belirteçler (predictable tokens) aracılığıyla **atlatılabilir**  
- [ ] Hayır — hesaplar, herhangi bir doğrulama olmaksızın gönderim anında **hemen aktif** hale geliyor  

### Kullanıcı, kayıt isteği sırasında rol veya izin parametrelerini manipüle edebiliyor mu?
- [ ] Hayır — roller **sunucu tarafında atanır** ve istemci tarafında herhangi bir parametre bulunmaz  
- [ ] Evet — rol parametreleri mevcut ancak sunucu taraflı doğrulama nedeniyle manipülasyon **mümkün değil**  
- [ ] Evet — rol/izin parametreleri (örneğin, `role=admin`, `is_staff=true`) yüksek yetkili erişim elde etmek için **değiştirilebilir** *(Kritik)*  

### Kitlesel hesap oluşturmayı önlemek için hız sınırlaması (Rate Limiting) veya otomasyon karşıtı kontroller uygulanmış mı?
- [ ] Evet — hız sınırları ve CAPTCHA'lar **etkin** ve başarılı  
- [ ] Evet — kontroller mevcut ancak başlık sahteciliği (header spoofing) veya CAPTCHA çözme servisleri ile atlatılması **mümkün**  
- [ ] Hayır — herhangi bir hız sınırlaması veya otomasyon karşıtı kontrol **uygulanmamış**  

### Hazırlama süreci mevcut hesaplar hakkında bilgi sızdırıyor mu (Kullanıcı Tespiti - User Enumeration)?
- [ ] Hayır — yanıt mesajları mevcut olan ve olmayan kullanıcılar için **aynıdır**  
- [ ] Evet — farklı yanıtlar veya zamanlama farklılıkları mevcut kullanıcıların **tespit edilmesine** (enumeration) olanak tanır  

---