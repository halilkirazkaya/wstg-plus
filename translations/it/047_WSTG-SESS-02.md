## WSTG-SESS-02 — Test degli attributi dei cookie

La gestione delle sessioni si basa spesso sui cookie HTTP per mantenere lo stato, rendendo gli attributi di sicurezza di questi cookie un obiettivo primario per gli attaccanti. Omettendo o configurando in modo errato attributi come `HttpOnly`, `Secure` e `SameSite`, le applicazioni espongono i token di sessione al furto tramite Cross-Site Scripting (XSS), all'intercettazione su canali non crittografati o ad attacchi di Cross-Site Request Forgery (CSRF). I pentester esaminano questi flag per determinare se un attaccante può esfiltrare gli identificatori di sessione dal Document Object Model (DOM) del browser o forzare il browser a inviarli in richieste cross-origin. Una corretta configurazione degli attributi è una misura fondamentale di defense-in-depth per garantire che i token sensibili rimangano limitati a contesti autorizzati e sicuri.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-SESS-02 |
| **CWE** | CWE-614 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/02-Testing_for_Cookies_Attributes  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Strumenti:** `Burp Suite (Proxy/Scanner)`, `OWASP ZAP`, `Browser Developer Tools (Storage Tab)`, `EditThisCookie`, `curl`

### Analisi dell'implementazione dei flag dei cookie
| Attributo | Presente | Mancante |
|-----------|:-------:|:-------:|
| `HttpOnly` |  |  |
| `Secure` |  |  |
| `SameSite` |  |  |

### Il flag `HttpOnly` è applicato ai cookie di sessione sensibili?
- [ ] Sì — applicato a **tutti** i cookie di sessione sensibili *(Massima sicurezza)*  
- [ ] Sì — applicato solo ad alcuni cookie di sessione  
- [ ] No — il flag **non è applicato**, consentendo l'accesso tramite script client-side *(Critico)*  

### Il flag `Secure` è applicato per i cookie trasmessi via HTTPS?
- [ ] Sì — applicato a **tutti** i cookie trasmessi via TLS  
- [ ] No — il flag **non è applicato**, consentendo l'invio dei cookie tramite HTTP in chiaro  

### L'attributo `SameSite` è utilizzato per mitigare i rischi CSRF?
- [ ] Sì — impostato su `Strict` o `Lax` per i cookie di sessione  
- [ ] Sì — impostato su `None` ma con il flag `Secure` presente  
- [ ] No — l'attributo è **mancante** o impostato su `None` **senza** `Secure`  

### Gli attributi `Domain` e `Path` sono limitati all'ambito minimo necessario?
- [ ] Sì — limitati al sottodominio **specifico** e al percorso dell'applicazione  
- [ ] No — l'ambito è troppo **ampio** (es. impostato su un dominio genitore o sulla root `/`), aumentando la superficie di attacco  

### I cookie di sessione sensibili scadono correttamente tramite gli attributi `Expires` o `Max-Age`?
- [ ] Sì — sono impostate date di scadenza appropriate  
- [ ] No — i cookie sono persistenti con una durata eccessivamente **lunga**  
- [ ] No — i cookie sono solo di sessione ma **mancano** dell'imposizione del timeout lato server  

---