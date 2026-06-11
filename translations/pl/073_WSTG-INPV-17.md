## WSTG-INPV-17 — Testowanie pod kątem Host Header Injection

Host Header Injection występuje, gdy aplikacja ufa dostarczonemu przez użytkownika nagłówkowi `Host` i włącza go do logiki po stronie serwera, takiej jak generowanie linków lub przekierowania, bez wystarczającej walidacji. Atakujący manipulują tym nagłówkiem, aby ułatwić przeprowadzenie Web Cache Poisoning, zatrucie mechanizmu resetowania hasła (Password Reset Poisoning) poprzez przekierowanie wrażliwych tokenów do zewnętrznej domeny lub obejście mechanizmów kontroli bezpieczeństwa opartych na routingu nagłówkowym. Skuteczna eksploatacja pozwala atakującemu na eksfiltrację danych sesyjnych, przejęcie konta (Account Takeover) lub przekierowanie niczego niepodejrzewających użytkowników do złośliwej infrastruktury. Podatność ta jest najczęściej spotykana w środowiskach z systemami równoważenia obciążenia (Load Balancing) lub w aplikacjach, które dynamicznie generują bezwzględne adresy URL na podstawie kontekstu przychodzącego żądania.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INPV-17 |
| **CWE** | CWE-20 |
| **Status testu** | Nieprzeprowadzono |
| **Poziom zagrożenia** | Średni / Wysoki* |

> *Poziom zagrożenia staje się Wysoki, jeśli nagłówek Host jest używany do generowania wrażliwych linków w wiadomościach e-mail, takich jak linki do resetowania hasła.

**Odniesienia:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/17-Testing_for_Host_Header_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/host-header  

**Narzędzia:** `Burp Suite (Repeater/Collaborator)`, `curl`, `Param Miner`, `nmap (http-vhosts)`

### Czy aplikacja odzwierciedla wartość nagłówka `Host` w linkach, przekierowaniach lub skryptach?
- [ ] Nie — aplikacja używa sztywno zakodowanych (hardcoded) bazowych adresów URL lub rygorystycznie walidowanej konfiguracji  
- [ ] Tak — nagłówek `Host` jest odzwierciedlany, ale rygorystycznie walidowany względem białej listy (whitelist)  
- [ ] Tak — nagłówek `Host` jest odzwierciedlany, a obejście (bypass) jest możliwe poprzez nieprawidłowo sformatowane wartości (np. `expected.com:attacker.com`)  
- [ ] Tak — nagłówek `Host` jest odzwierciedlany **bez** żadnej walidacji  

### Czy alternatywne nagłówki związane z hostem są przetwarzane przez aplikację lub infrastrukturę?
- [ ] Nie — nagłówki takie jak `X-Forwarded-Host`, `X-Host` lub `Forwarded` są ignorowane  
- [ ] Tak — alternatywne nagłówki są przetwarzane, ale **nie** nadpisują logiki wewnętrznej  
- [ ] Tak — alternatywne nagłówki **mogą** być użyte do nadpisania podstawowej wartości nagłówka `Host`  

### Czy nagłówek `Host` może zostać zmanipulowany w celu zatrucia wiadomości e-mail generowanych przez system (np. resetowania hasła)?
- [ ] Nie — linki w wiadomościach e-mail używają statycznej, zaufanej domeny z konfiguracji serwera  
- [ ] Tak — nagłówek `Host` wpływa na linki w e-mailach, ale walidacji **nie da się** obejść  
- [ ] Tak — nagłówek `Host` **może** zostać zmanipulowany w celu przekierowania tokenów resetowania hasła do domeny kontrolowanej przez atakującego *(Krytyczne)*  

### Czy aplikacja jest podatna na Web Cache Poisoning poprzez nagłówek `Host`?
- [ ] Nie — brak mechanizmu buforowania (caching) lub nagłówek `Host` jest częścią klucza pamięci podręcznej (cache key)  
- [ ] Tak — pamięć podręczna istnieje, ale rygorystycznie waliduje lub ignoruje nagłówek `Host` dla wejść niebędących kluczem (unkeyed inputs)  
- [ ] Tak — pamięć podręczna istnieje i **może** zostać zatruta przez wstrzyknięty nagłówek `Host`, aby serwować złośliwą treść innym użytkownikom  

### Czy serwer akceptuje dowolne nagłówki `Host` dla routingu wirtualnych hostów (virtual host routing)?
- [ ] Nie — serwer zwraca błąd lub stronę domyślną dla nierozpoznanych nagłówków `Host`  
- [ ] Tak — serwer akceptuje dowolny nagłówek `Host`, co **jest** użyteczne dla rekonesansu lub enumeracji usług wewnętrznych  

---