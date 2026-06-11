## WSTG-SESS-02 — Çerez Özniteliklerinin Test Edilmesi

Oturum yönetimi (Session management), durumu korumak için genellikle HTTP çerezlerine (cookies) dayanır; bu da söz konusu çerezlerin güvenlik özniteliklerini (attributes) saldırganlar için birincil hedef haline getirir. Uygulamalar; `HttpOnly`, `Secure` ve `SameSite` gibi öznitelikleri atlayarak veya yanlış yapılandırarak; oturum belirteçlerini Cross-Site Scripting (XSS) yoluyla çalınmaya, şifrelenmemiş kanallar üzerinden ele geçirilmeye veya Cross-Site Request Forgery (CSRF) saldırılarına maruz bırakır. Sızma testi uzmanları (Pentesters), bir saldırganın tarayıcının doküman nesne modelinden (DOM) oturum tanımlayıcılarını sızdırıp sızdıramayacağını veya tarayıcıyı bunları çapraz kaynaklı (cross-origin) isteklerde göndermeye zorlayıp zorlayamayacağını belirlemek için bu bayrakları inceler. Uygun öznitelik yapılandırması, hassas belirteçlerin yetkili ve güvenli bağlamlarla sınırlı kalmasını sağlamak için temel bir derinlemesine savunma (defense-in-depth) önlemidir.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-SESS-02 |
| **CWE** | CWE-614 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Orta |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/02-Testing_for_Cookies_Attributes  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Araçlar:** `Burp Suite (Proxy/Scanner)`, `OWASP ZAP`, `Browser Developer Tools (Storage Tab)`, `EditThisCookie`, `curl`

### Çerez Bayrağı Uygulamasının Analizi
| Öznitelik | Mevcut | Eksik |
|-----------|:-------:|:-------:|
| `HttpOnly` |  |  |
| `Secure` |  |  |
| `SameSite` |  |  |

### Hassas oturum çerezlerine `HttpOnly` bayrağı uygulanmış mı?
- [ ] Evet — **tüm** hassas oturum çerezlerine uygulanmış *(En güvenli)*  
- [ ] Evet — yalnızca bazı oturum çerezlerine uygulanmış  
- [ ] Hayır — bayrak **uygulanmamış**, istemci tarafı betikler (scripts) üzerinden erişime izin veriliyor *(Kritik)*  

### HTTPS üzerinden iletilen çerezler için `Secure` bayrağı zorunlu tutuluyor mu?
- [ ] Evet — TLS üzerinden iletilen **tüm** çerezlere uygulanmış  
- [ ] Hayır — bayrak **uygulanmamış**, çerezlerin düz metin HTTP üzerinden gönderilmesine izin veriliyor  

### CSRF risklerini azaltmak için `SameSite` özniteliği kullanılıyor mu?
- [ ] Evet — oturum çerezleri için `Strict` veya `Lax` olarak ayarlanmış  
- [ ] Evet — `None` olarak ayarlanmış ancak `Secure` bayrağı mevcut  
- [ ] Hayır — öznitelik **eksik** veya `Secure` bayrağı **olmadan** `None` olarak ayarlanmış  

### `Domain` ve `Path` öznitelikleri gerekli olan minimum kapsamla sınırlandırılmış mı?
- [ ] Evet — **belirli** alt alan adı (subdomain) ve uygulama yolu ile sınırlandırılmış  
- [ ] Hayır — kapsam çok **geniş** (örneğin, bir üst alan adına veya kök dizin `/` değerine ayarlanmış), bu da saldırı yüzeyini artırıyor  

### Hassas oturum çerezleri `Expires` veya `Max-Age` öznitelikleri aracılığıyla düzgün bir şekilde sonlandırılıyor mu?
- [ ] Evet — uygun son kullanma tarihleri ayarlanmış  
- [ ] Hayır — çerezler aşırı **uzun** ömürlü ve kalıcı (persistent)  
- [ ] Hayır — çerezler yalnızca oturum bazlı (session-only) ancak sunucu tarafında zaman aşımı (timeout) zorunluluğu **bulunmuyor**  

---