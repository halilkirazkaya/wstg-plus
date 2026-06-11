## WSTG-ATHN-08 — Testowanie pod kątem słabych odpowiedzi na pytania pomocnicze

Testowanie pod kątem słabych odpowiedzi na pytania pomocnicze (security questions) polega na ocenie mechanizmów uwierzytelniania opartego na wiedzy (KBA — Knowledge-Based Authentication) stosowanych do weryfikacji tożsamości użytkownika, zazwyczaj podczas procesów odzyskiwania hasła lub w ramach przepływów uwierzytelniania wieloskładnikowego. Ma to istotne znaczenie, ponieważ pytania pomocnicze często opierają się na statycznych, niejawnych informacjach, które można łatwo odkryć za pomocą białego wywiadu (OSINT) lub socjotechniki (Social Engineering), co prowadzi do nieautoryzowanego przejęcia konta. Podatności te występują zazwyczaj w modułach „Zapomniałem hasła” lub „Odblokowanie konta”, gdzie użytkownicy udzielają odpowiedzi na wcześniej ustalone pytania. Z perspektywy atakującego jest to doskonały cel dla zautomatyzowanych ataków typu Brute Force lub celowanego rozpoznania, ponieważ wielu użytkowników udziela przewidywalnych lub publicznie weryfikowalnych odpowiedzi na powszechne pytania, takie jak „Jakie jest nazwisko panieńskie matki?” lub „Do jakiej szkoły średniej uczęszczałeś?”.


| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-ATHN-08 |
| **CWE** | CWE-640 |
| **Status testu** | Nie wykonano |
| **Poziom krytyczności** | Średni / Wysoki* |

> *Poziom krytyczności staje się wysoki (High), jeśli pytanie pomocnicze jest jedynym wymogiem do pełnego zresetowania hasła i przejęcia konta bez dalszej weryfikacji.

**Źródła:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/08-Testing_for_Weak_Security_Question_Answer  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**Narzędzia:** `Burp Suite (Intruder)`, `ffuf`, `Maltego`, `Sherlock`, `Social Engineering Toolkit (SET)`

### Czy aplikacja implementuje pytania pomocnicze do weryfikacji tożsamości lub odzyskiwania hasła?
- [ ] Nie — pytania pomocnicze **nie są używane** do uwierzytelniania ani odzyskiwania konta  
- [ ] Tak — pytania pomocnicze są **używane** jako opcjonalny drugi składnik  
- [ ] Tak — pytania pomocnicze są **używane** jako główny/jedyny mechanizm odzyskiwania *(Wysokie ryzyko)*  

### Czy pytania pomocnicze są wybierane z ustalonej listy, czy są definiowane przez użytkownika?
- [ ] Nie — pytania są całkowicie definiowane przez użytkownika i wymagają wysokiej entropii  
- [ ] Tak — pytania są ustalone, ale obejmują opcje trudne do odkrycia za pomocą OSINT  
- [ ] Tak — pytania pochodzą z ustalonej listy powszechnych tematów możliwych do odkrycia przez OSINT *(Podatność)*  

### Czy do prób udzielenia odpowiedzi na pytania pomocnicze stosowane jest ograniczenie liczby żądań (Rate Limiting) lub blokada konta?
- [ ] Tak — stosowane jest rygorystyczne Rate Limiting i blokada konta, aby zapobiec atakom Brute Force  
- [ ] Tak — stosowane jest Rate Limiting, ale możliwe jest jego **obejście** poprzez rotację IP lub manipulację nagłówkami  
- [ ] Nie — **nie wymuszono żadnego Rate Limiting** dla prób udzielenia odpowiedzi  

### Czy aplikacja wymusza złożoność lub walidację treści odpowiedzi?
- [ ] Tak — walidacja zapobiega używaniu popularnych słów i zapewnia złożoność/unikalność odpowiedzi  
- [ ] Tak — podstawowa walidacja istnieje, ale **można** ją obejść za pomocą popularnych list haseł lub ataków słownikowych  
- [ ] Nie — **nie wykonuje się żadnej walidacji**; dozwolone są proste lub jednoznakowe odpowiedzi  

### Czy wyzwanie w postaci pytania pomocniczego może zostać pominięte poprzez błędy logiczne?
- [ ] Nie — proces odzyskiwania ściśle wymaga poprawnej odpowiedzi, aby kontynuować  
- [ ] Tak — proces odzyskiwania **może** zostać pominięty poprzez manipulację kodami odpowiedzi HTTP lub parametrami sesji  
- [ ] Tak — odpowiedź jest odzwierciedlona w kodzie źródłowym lub ukrytych polach strony  

---