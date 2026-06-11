## WSTG-INPV-13 — Format String Injection

L'injection di stringhe di formato (Format String Injection) si verifica quando un'applicazione web passa l'input controllato dall'utente direttamente nell'argomento della stringa di formato di una funzione variadica, come la famiglia `printf` del C o wrapper di logging a basso livello. Sebbene sia meno comune nei moderni framework web a codice gestito (managed code), questa vulnerabilità rimane critica nelle applicazioni CGI legacy, nelle estensioni native o nei sistemi di logging in casi limite che interagiscono con librerie di sistema a basso livello. Gli attaccanti sfruttano questo comportamento fornendo specificatori di formato come `%x` o `%p` per esfiltrare indirizzi di memoria sensibili e dati dallo stack, oppure lo specificatore `%n` per eseguire scritture in memoria arbitrarie. Uno sfruttamento (Exploit) riuscito tipicamente si traduce nella completa compromissione del sistema tramite Remote Code Execution (RCE) o, come minimo, in una condizione di Denial-of-Service (DoS) attraverso il crash del processo.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INPV-13 |
| **CWE** | CWE-134 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta / Critica |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/13-Testing_for_Format_String_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `Burp Suite`, `GDB`, `strings`, `radare2`, `Checksec`, `AFL++`

### L'applicazione elabora l'input dell'utente attraverso codice nativo o interfacce CGI legacy?
- [ ] No — l'applicazione è costruita interamente su framework a codice gestito (es. JVM, CLR)  
- [ ] Sì — l'applicazione utilizza JNI, estensioni native C/C++ o CGI binari legacy dove le vulnerabilità di Format String Injection **possono** esistere  

### Gli input forniti dall'utente sono passati come argomento principale a funzioni sensibili al formato?
- [ ] No — gli input sono passati come argomenti di dati e le stringhe di formato sono **statiche** e **hardcoded**  
- [ ] Sì — l'input dell'utente è direttamente concatenato nell'argomento della stringa di formato, ma **viene applicata** una sanificazione  
- [ ] Sì — l'input dell'utente è usato direttamente come stringa di formato e **non viene applicata** alcuna sanificazione  

### È possibile esfiltrare contenuti della memoria utilizzando gli specificatori di formato?
- [ ] No — l'input è validato e specificatori come `%p`, `%x` o `%s` **non vengono elaborati**  
- [ ] Sì — l'inserimento di specificatori `%p` o `%x` comporta la visualizzazione di indirizzi di memoria dello stack o dello heap  
- [ ] Sì — dati sensibili (es. puntatori, stack cookies) **possono** essere esfiltrati tramite specificatori di formato  

### È possibile causare il crash dell'applicazione o manipolarla utilizzando lo specificatore `%n`?
- [ ] No — gli specificatori che permettono la scrittura in memoria sono **disabilitati** o bloccati dal compilatore/ambiente  
- [ ] Sì — l'applicazione va in crash (DoS) quando vengono fornite stringhe come `%s%s%s%s` o simili  
- [ ] Sì — scritture in memoria arbitrarie tramite `%n` sono **possibili**, portando potenzialmente a una Remote Code Execution (RCE)  

### Le moderne protezioni binarie (ASLR, DEP, Stack Canaries) sono aggirabili tramite questa injection?
- [ ] No — l'ambiente è configurato in modo sicuro (hardened) e il leak di memoria **non può** essere utilizzato per bypassare le protezioni  
- [ ] Sì — la Format String Injection **può** essere utilizzata per esfiltrare indirizzi base, rendendo **possibile** il bypass di ASLR/DEP  

---