## WSTG-INPV-17 — Testing for Host Header Injection

L'Host Header Injection si verifica quando un'applicazione si fida dell'header `Host` fornito dall'utente e lo incorpora nella logica lato server, come la generazione di link o i reindirizzamenti, senza una validazione sufficiente. Gli attaccanti manipolano questo header per facilitare il Web Cache Poisoning, il Password Reset Poisoning reindirizzando token sensibili verso un dominio esterno, o per bypassare i controlli di sicurezza basati sul routing degli header. L'esfruttamento riuscito permette a un attaccante di esfiltrare dati di sessione, eseguire Account Takeover o reindirizzare utenti ignari verso infrastrutture malevole. Questa vulnerabilità si riscontra comunemente in ambienti bilanciati (Load-balanced) o in applicazioni che generano dinamicamente URL assoluti basati sul contesto della richiesta in arrivo.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INPV-17 |
| **CWE** | CWE-20 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Alta* |

> *La gravità diventa Alta se l'header Host viene utilizzato per generare link sensibili nelle email, come i reset della password.

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/17-Testing_for_Host_Header_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/host-header  

**Strumenti:** `Burp Suite (Repeater/Collaborator)`, `curl`, `Param Miner`, `nmap (http-vhosts)`

### L'applicazione riflette il valore dell'header `Host` in link, reindirizzamenti o script?
- [ ] No — l'applicazione utilizza URL di base hardcoded o una configurazione validata rigorosamente  
- [ ] Sì — l'header `Host` viene riflesso ma validato rigorosamente rispetto a una whitelist  
- [ ] Sì — l'header `Host` viene riflesso e il bypass **è possibile** tramite valori malformati (es. `expected.com:attacker.com`)  
- [ ] Sì — l'header `Host` viene riflesso **senza** alcuna validazione  

### Vengono elaborati header alternativi relativi all'host dall'applicazione o dall'infrastruttura?
- [ ] No — gli header come `X-Forwarded-Host`, `X-Host` o `Forwarded` vengono ignorati  
- [ ] Sì — gli header alternativi vengono elaborati ma **non** sovrascrivono la logica interna  
- [ ] Sì — gli header alternativi **possono** essere utilizzati per sovrascrivere il valore dell'header `Host` primario  

### L'header `Host` può essere manipolato per avvelenare le email generate dal sistema (es. reset della password)?
- [ ] No — i link nelle email utilizzano un dominio statico e fidato dalla configurazione del server  
- [ ] Sì — l'header `Host` influenza i link delle email ma la validazione **non** può essere bypassata  
- [ ] Sì — l'header `Host` **può** essere manipolato per reindirizzare i token di reset della password verso un dominio controllato dall'attaccante *(Critico)*  

### L'applicazione è suscettibile al Web Cache Poisoning tramite l'header `Host`?
- [ ] No — non è presente alcun meccanismo di caching o l'header `Host` fa parte della cache key  
- [ ] Sì — è presente una cache ma questa valida rigorosamente o ignora l'header `Host` per gli input non inclusi nella chiave (unkeyed inputs)  
- [ ] Sì — esiste una cache e **può** essere avvelenata da un header `Host` iniettato per servire contenuti malevole ad altri utenti  

### Il server accetta header `Host` arbitrari per il routing dei virtual host?
- [ ] No — il server restituisce un errore o una pagina predefinita per gli header `Host` non riconosciuti  
- [ ] Sì — il server accetta qualsiasi header `Host`, il che **è** utile per attività di reconnaissance o enumerazione di servizi interni  

---