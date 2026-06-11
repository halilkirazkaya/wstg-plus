## WSTG-BUSL-05 — Testowanie limitów liczby użyć danej funkcji

Ten test ocenia, czy aplikacja wymusza ograniczenia logiki biznesowej dotyczące częstotliwości oraz całkowitej liczby wykonań określonej akcji lub funkcji przez pojedynczego użytkownika, sesję lub adres IP. Brak wdrożenia tych limitów pozwala atakującym na nadużywanie istotnych funkcji — takich jak wysyłanie kodów SMS OTP, generowanie e-maili do resetowania hasła, stosowanie kodów rabatowych czy wykonywanie obciążających eksportów danych — co prowadzi do strat finansowych, wyczerpania zasobów lub utraty reputacji. Podatności te zazwyczaj występują w końcówkach API (endpoints) lub funkcjach komunikacyjnych i są wykorzystywane za pomocą zautomatyzowanych skryptów powtarzających akcję znacznie powyżej zamierzonych progów biznesowych. Z perspektywy atakującego jest to główny cel dla ataków typu SMS pumping, mail bombing lub workflowów typu Brute Force, które nie posiadają jawnych ograniczeń Rate Limiting lub liczników użyć.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-BUSL-05 |
| **CWE** | CWE-799 |
| **Status testu** | Nie przeprowadzono |
| **Poziom istotności** | Średni / Wysoki* |

> *Poziom istotności staje się Wysoki, jeśli funkcja obejmuje transakcje finansowe, koszty SMS lub wpływa na dostępność całego systemu.

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/05-Test_Number_of_Times_a_Function_Can_Be_Used_Limits  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite (Intruder/Repeater)`, `Turbo Intruder`, `Python (requests)`, `OWASP ZAP`

### Czy aplikacja definiuje i wymusza limity dla wrażliwych funkcji biznesowych?
- [ ] Tak — limity są rygorystycznie wymuszane, a ich obejście jest **niemożliwe**  
- [ ] Tak — limity są wymuszane, ale są zbyt wysokie, aby zapobiec nadużyciom  
- [ ] Nie — nie stosuje się żadnych limitów dla wykonania funkcji  

### Czy limity Rate Limiting oraz liczniki użycia są wymuszane po stronie serwera?
- [ ] Tak — wymuszanie odbywa się wyłącznie po stronie serwera i **nie może** być manipulowane  
- [ ] Nie — wymuszanie opiera się na logice po stronie klienta (np. JavaScript lub ukryte pola)  
- [ ] Nie — nie istnieje żadne wymuszanie na żadnym poziomie  

### Czy limity użycia mogą zostać pominięte poprzez manipulację nagłówkami lub sesją?
- [ ] Nie — obejście poprzez nagłówki `X-Forwarded-For`, `Origin` lub rotację sesji jest **niemożliwe**  
- [ ] Tak — limity **mogą** zostać pominięte poprzez podszywanie się pod nagłówki IP lub usuwanie plików cookie  
- [ ] Tak — limity **mogą** zostać pominięte poprzez tworzenie wielu kont lub sesji  

### Czy przekroczenie limitu wyzwala odpowiednią reakcję obronną?
- [ ] Tak — aplikacja zwraca kod `429 Too Many Requests` i loguje zdarzenie *(Najbezpieczniejsze)*  
- [ ] Tak — aplikacja zwraca błąd, ale **nie** loguje potencjalnego nadużycia  
- [ ] Nie — aplikacja kontynuuje przetwarzanie żądań, ale ignoruje wynik  
- [ ] Nie — aplikacja nie udziela odpowiedzi lub ulega awarii pod obciążeniem  

### Czy wpływ nadużycia funkcji jest istotny dla biznesu?
- [ ] Nie — nadużycie funkcji ma znikomy wpływ na koszty lub zasoby  
- [ ] Tak — nadużycie **może** prowadzić do niewielkiego zużycia zasobów lub uciążliwości dla użytkownika  
- [ ] Tak — nadużycie **może** prowadzić do znacznych kosztów finansowych (np. opłaty za SMS) lub odmowy usługi *(Krytyczne)*  

---