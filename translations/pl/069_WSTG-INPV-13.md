## WSTG-INPV-13 — Format String Injection

Wstrzyknięcie łańcucha formatującego (Format String Injection) występuje, gdy aplikacja webowa przekazuje dane wejściowe kontrolowane przez użytkownika bezpośrednio do argumentu będącego łańcuchem formatującym funkcji o zmiennej liczbie argumentów, takiej jak rodzina funkcji `printf` w języku C lub niskopoziomowe wrappery logowania. Chociaż podatność ta jest rzadziej spotykana w nowoczesnych frameworkach webowych opartych na kodzie zarządzanym (managed code), pozostaje ona krytyczna w starszych aplikacjach CGI, natywnych rozszerzeniach lub specyficznych systemach logowania wchodzących w interakcję z niskopoziomowymi bibliotekami systemowymi. Atakujący wykorzystują tę lukę, dostarczając specyfikatory formatu, takie jak `%x` lub `%p`, aby ujawnić wrażliwe adresy pamięci i dane ze stosu, lub specyfikator `%n` w celu wykonania dowolnego zapisu w pamięci. Skuteczna eksploatacja zazwyczaj skutkuje całkowitym przejęciem systemu poprzez zdalne wykonanie kodu (Remote Code Execution - RCE) lub, co najmniej, znaczącym stanem odmowy usługi (Denial-of-Service - DoS) poprzez wymuszenie awarii procesów.

| Pole | Wartość |
|---|---|
| **WSTG ID** | WSTG-INPV-13 |
| **CWE** | CWE-134 |
| **Status testu** | Nie przeprowadzono |
| **Poziom ważności** | Wysoki / Krytyczny |

**Referencje:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/13-Testing_for_Format_String_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Narzędzia:** `Burp Suite`, `GDB`, `strings`, `radare2`, `Checksec`, `AFL++`

### Czy aplikacja przetwarza dane wejściowe użytkownika poprzez kod natywny lub starsze interfejsy CGI?
- [ ] Nie — aplikacja została zbudowana całkowicie w oparciu o frameworki kodu zarządzanego (np. JVM, CLR)  
- [ ] Tak — aplikacja korzysta z JNI, natywnych rozszerzeń C/C++ lub starszych binarnych skryptów CGI, gdzie podatności typu format string **mogą** istnieć  

### Czy dane wejściowe użytkownika są przekazywane jako główny argument do funkcji obsługujących formatowanie?
- [ ] Nie — dane wejściowe są przekazywane jako argumenty danych, a łańcuchy formatujące są **statyczne** i **zakodowane na sztywno (hardcoded)**  
- [ ] Tak — dane wejściowe użytkownika są bezpośrednio łączone z argumentem łańcucha formatującego, ale **zastosowano** sanityzację  
- [ ] Tak — dane wejściowe użytkownika są bezpośrednio używane jako łańcuch formatujący i **nie zastosowano** żadnej sanityzacji  

### Czy możliwe jest wycieknięcie zawartości pamięci przy użyciu specyfikatorów formatu?
- [ ] Nie — dane wejściowe są walidowane, a specyfikatory takie jak `%p`, `%x` lub `%s` **nie są przetwarzane**  
- [ ] Tak — podanie specyfikatorów `%p` lub `%x` skutkuje odzwierciedleniem adresów pamięci stosu lub sterty  
- [ ] Tak — wrażliwe dane (np. wskaźniki, stack cookies) **mogą** zostać wytransferowane za pomocą specyfikatorów formatu  

### Czy można spowodować awarię aplikacji lub manipulować nią za pomocą specyfikatora `%n`?
- [ ] Nie — specyfikatory umożliwiające zapis w pamięci są **wyłączone** lub blokowane przez kompilator/środowisko  
- [ ] Tak — aplikacja ulega awarii (DoS), gdy podane zostaną ciągi `%s%s%s%s` lub podobne  
- [ ] Tak — dowolny zapis w pamięci poprzez `%n` jest **możliwy**, co potencjalnie prowadzi do zdalnego wykonania kodu (Remote Code Execution - RCE)  

### Czy nowoczesne zabezpieczenia binarne (ASLR, DEP, Stack Canaries) są możliwe do obejścia poprzez tę iniekcję?
- [ ] Nie — środowisko jest utwardzone (hardened), a wyciek pamięci **nie może** zostać wykorzystany do obejścia zabezpieczeń  
- [ ] Tak — Format String Injection **może** zostać użyte do wycieku adresów bazowych, co sprawia, że obejście ASLR/DEP jest **możliwe**  

---