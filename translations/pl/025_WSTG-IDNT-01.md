## WSTG-IDNT-01 — Testowanie definicji ról

Testowanie definicji ról polega na identyfikacji i dokumentowaniu różnych ról użytkowników oraz poziomów uprawnień zdefiniowanych w aplikacji, aby upewnić się, że są one logicznie odseparowane i zgodne z zasadą najniższych uprawnień (principle of least privilege). Atakujący analizuje aplikację w celu zmapowania ról o wysokich uprawnieniach w zestawieniu ze standardowymi rolami użytkowników, szukając wszelkich niejasności w granicach uprawnień lub nieudokumentowanych funkcji administracyjnych. Identyfikacja tych ról jest pierwszym krokiem do odkrycia ścieżek eskalacji uprawnień (privilege escalation), ponieważ niespójności w definicjach ról często prowadzą do nieautoryzowanego dostępu do wrażliwych danych lub funkcji administracyjnych. Faza ta zazwyczaj odbywa się podczas wstępnego rekonesansu i mapowania logiki biznesowej, koncentrując się na profilach użytkowników, panelach administracyjnych oraz strukturach uprawnień API.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-IDNT-01 |
| **CWE** | CWE-285 |
| **Status testu** | Nieprzeprowadzony |
| **Dotkliwość** | Niski / Średni |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/01-Test_Role_Definitions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite (Compare Site Maps)`, `Postman`, `AuthMatrix`, `Authz`, `Browser Developer Tools`

### Czy role są wyraźnie zdefiniowane i udokumentowane w aplikacji lub jej dokumentacji?
- [ ] Tak — role są w pełni udokumentowane i są zgodne z zasadą **najniższych uprawnień (least privilege)**  
- [ ] Tak — role są udokumentowane, ale zawierają **nakładające się uprawnienia**  
- [ ] Nie — nie istnieje formalna dokumentacja ani definicje ról  

### Czy istnieje wyraźny rozdział między funkcjami administracyjnymi a funkcjami standardowego użytkownika?
- [ ] Tak — funkcje administracyjne są **całkowicie odizolowane** od widoków standardowego użytkownika  
- [ ] Tak — rozdział istnieje, ale punkty końcowe (endpoints) administracji są **przewidywalne lub możliwe do odgadnięcia**  
- [ ] Nie — funkcje administracyjne i standardowe są **wymieszane** w ramach tych samych interfejsów  

### Czy aplikacja wykorzystuje scentralizowany mechanizm kontroli dostępu opartej na rolach (RBAC)?
- [ ] Tak — scentralizowany mechanizm RBAC lub ABAC jest **zaimplementowany** i konsekwentnie egzekwowany  
- [ ] Tak — istnieją zdecentralizowane mechanizmy kontrolne, ale są **niekonsekwentnie stosowane** w różnych modułach  
- [ ] Nie — logika kontroli dostępu jest **zakodowana na sztywno (hardcoded)** w poszczególnych stronach lub komponentach  

### Czy w systemie występują nieudokumentowane lub ukryte role?
- [ ] Nie — wszystkie wykryte role odpowiadają udokumentowanej funkcjonalności  
- [ ] Tak — ukryte role lub role typu "shadow" (np. `super_admin`, `support`, `test`) **są obecne**  

### Czy użytkownik o niskich uprawnieniach może wyliczyć (enumerować) uprawnienia lub role innych użytkowników?
- [ ] Nie — użytkownicy **nie mogą** przeglądać ani wyliczać ról/uprawnień innych podmiotów  
- [ ] Tak — użytkownicy **mogą** wyliczać role poprzez strony profilowe, metadane lub odpowiedzi API *(Średni)*  

---