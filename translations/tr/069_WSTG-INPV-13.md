## WSTG-INPV-13 — Format String Injection

Format string injection (format dizgisi enjeksiyonu), bir web uygulamasının kullanıcı tarafından kontrol edilen girdiyi doğrudan C'nin `printf` ailesi veya düşük seviyeli günlükleme (logging) sarmalayıcıları gibi değişken argümanlı (variadic) bir fonksiyonun format dizgisi argümanına iletmesiyle oluşur. Modern yönetilen kod (managed-code) web framework'lerinde daha az yaygın olsa da, bu zafiyet eski nesil CGI uygulamalarında, yerel (native) eklentilerde veya düşük seviyeli sistem kütüphaneleriyle etkileşime giren uç durum günlükleme sistemlerinde kritikliğini korumaktadır. Saldırganlar, yığın (stack) üzerindeki hassas bellek adreslerini ve verileri sızdırmak için `%x` veya `%p` gibi format belirleyicilerini (format specifiers) ya da rastgele bellek yazma işlemleri gerçekleştirmek için `%n` belirleyicisini sağlayarak bu durumdan faydalanırlar. Başarılı bir istismar (exploit), genellikle Uzaktan Kod Yürütme (Remote Code Execution - RCE) yoluyla sistemin tamamen ele geçirilmesiyle veya en azından süreç çökmeleri aracılığıyla etkili bir hizmet dışı bırakma (Denial-of-Service - DoS) durumuyla sonuçlanır.

| Alan | Değer |
|---|---|
| **WSTG ID** | WSTG-INPV-13 |
| **CWE** | CWE-134 |
| **Test Durumu** | Gerçekleştirilmedi |
| **Önem Derecesi** | Yüksek / Kritik |

**Referanslar:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/13-Testing_for_Format_String_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Araçlar:** `Burp Suite`, `GDB`, `strings`, `radare2`, `Checksec`, `AFL++`

### Uygulama, kullanıcı girdisini yerel kod (native code) veya eski nesil CGI arayüzleri üzerinden mi işliyor?
- [ ] Hayır — uygulama tamamen yönetilen kod framework'leri (örneğin JVM, CLR) üzerine inşa edilmiştir  
- [ ] Evet — uygulama, format string zafiyetlerinin **bulunabileceği** JNI, yerel C/C++ eklentileri veya eski nesil ikili (binary) CGI'lar kullanmaktadır  

### Kullanıcı tarafından sağlanan girdiler, format farkındalığı olan fonksiyonlara birincil argüman olarak mı iletiliyor?
- [ ] Hayır — girdiler veri argümanı olarak iletilir ve format dizgileri **statik** ve **sabit kodludur (hardcoded)**  
- [ ] Evet — kullanıcı girdisi doğrudan format dizgisi argümanına eklenir ancak temizleme (sanitization) **uygulanmaktadır**  
- [ ] Evet — kullanıcı girdisi doğrudan format dizgisi olarak kullanılır ve hiçbir temizleme **uygulanmamaktadır**  

### Format belirleyicileri kullanılarak bellek içeriğini sızdırmak mümkün mü?
- [ ] Hayır — girdi doğrulanır ve `%p`, `%x` veya `%s` gibi belirleyiciler **işlenmez**  
- [ ] Evet — `%p` veya `%x` belirleyicilerinin sağlanması, yığın (stack) veya öbek (heap) bellek adreslerinin yansımasıyla sonuçlanır  
- [ ] Evet — hassas veriler (örneğin işaretçiler, stack cookies) format belirleyicileri aracılığıyla **sızdırılabilir**  

### Uygulama `%n` belirleyicisi kullanılarak çökertilebilir mi veya manipüle edilebilir mi?
- [ ] Hayır — bellek yazma işlemlerine izin veren belirleyiciler **devre dışı bırakılmıştır** veya derleyici/ortam tarafından engellenmiştir  
- [ ] Evet — `%s%s%s%s` veya benzeri dizgiler sağlandığında uygulama çöker (DoS)  
- [ ] Evet — `%n` üzerinden rastgele bellek yazma işlemleri **mümkündür** ve potansiyel olarak Uzaktan Kod Yürütme (RCE) ile sonuçlanabilir  

### Modern ikili korumalar (ASLR, DEP, Stack Canaries), bu enjeksiyon yoluyla atlatılabilir mi?
- [ ] Hayır — ortam sıkılaştırılmıştır (hardened) ve bellek sızıntısı korumaları atlatmak için **kullanılamaz**  
- [ ] Evet — format string injection, temel adresleri sızdırmak için **kullanılabilir** ve ASLR/DEP atlatmayı **mümkün** kılar  

---