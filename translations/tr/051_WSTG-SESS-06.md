## WSTG-SESS-06 — Oturumu Kapatma İşlevinin Test Edilmesi

Oturumu kapatma (Logout) işlevi, bir kullanıcının oturumunu sonlandırmak ve hem istemci hem de sunucu tarafındaki ilgili oturum tanımlayıcılarını (Session Identifiers) geçersiz kılmak için tasarlanmış kritik bir güvenlik kontrolüdür. Oturumu kapatma işleminin düzgün bir şekilde uygulanmaması, oturum sabitleme (Session Fixation) veya oturum ele geçirme (Hijacking) saldırılarına olanak tanır; çünkü bir kullanıcı "oturumu kapattıktan" sonra makineye erişim sağlayan bir saldırgan, hala aktif olan oturum belirtecini (Session Token) potansiyel olarak yeniden kullanabilir. Sızma testi uzmanları (Pentesters), bu durumu oturum çerezinin tarayıcıdan temizlenip temizlenmediğini, sunucu tarafındaki oturum durumunun açıkça yok edilip edilmediğini ve geri butonu ile gezinmenin önbelleğe alınmış hassas verilere erişime izin verip vermediğini kontrol ederek değerlendirir. Güvenli bir oturum kapatma uygulaması, eylem tetiklendikten sonra eski oturum belirteci ile yapılan sonraki hiçbir isteğin uygulama tarafından yetkilendirilmemesini sağlar.

| Alan | Değer |
|---|---|
| **WSTG Kimliği** | WSTG-SESS-06 |
| **CWE** | CWE-613 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/06-Testing_for_Logout_Functionality  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Araçlar:** `Burp Suite (Repeater/Proxy)`, `Browser Developer Tools`, `OWASP ZAP`

### İşlevsel bir oturumu kapatma mekanizması mevcut mu ve erişilebilir mi?
- [ ] Evet — oturumu kapatma butonu mevcut ve bir sonlandırma isteğini tetikliyor  
- [ ] Hayır — oturumu kapatma butonu **eksik** veya bir sonlandırma olayını **tetiklemiyor**  

### Oturum tanımlayıcı (Session Identifier) sunucu tarafında geçersiz kılınıyor mu?
- [ ] Evet — sunucu, eski oturum belirtecini kullanan sonraki tüm istekleri reddediyor  
- [ ] Hayır — sunucu, oturumu kapattıktan sonra eski oturum belirtecini **kabul etmeye devam ediyor**  

### Uygulama, oturumu kapattıktan sonra tarayıcıdaki oturum çerezlerini (Cookies) temizliyor mu?
- [ ] Evet — çerezlerin üzerine sona erme tarihi geçmiş veya boş bir değer yazılıyor  
- [ ] Evet — çerezler kalıyor ancak sunucuda **artık geçerli değil**  
- [ ] Hayır — çerezler tarayıcıda kalıyor ve orijinal değerlerini **koruyor**  

### Oturumu kapattıktan sonra tarayıcının "Geri" butonu aracılığıyla kimliği doğrulanmış hassas içeriğe erişilebiliyor mu?
- [ ] Hayır — `Cache-Control: no-store` veya benzeri başlıklar (Headers), oturum kapatıldıktan sonra hassas sayfaların görüntülenmesini engelliyor  
- [ ] Evet — hassas sayfalar önbelleğe alınıyor ve oturum sonlandırıldıktan sonra navigasyon yoluyla **görüntülenebiliyor**  

---