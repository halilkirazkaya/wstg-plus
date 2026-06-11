## WSTG-CLNT-08 — Testing for Cross Site Flashing

Il Cross-Site Flashing (XSF) è una vulnerabilità client-side che si verifica quando un file Flash (SWF) gestisce in modo improprio l'input controllato dall'utente, consentendo a un attaccante di eseguire codice ActionScript malevolo o di interfacciarsi con l'ambiente JavaScript del browser. Manipolando parametri come `FlashVars` o stringhe di query URL passate a sink come `ExternalInterface.call`, `getURL` o `loadMovie`, un attaccante può eseguire azioni nel contesto del dominio vulnerabile, inclusi il session hijacking e l'esfiltrazione di dati. Questa vulnerabilità si riscontra principalmente in applicazioni enterprise legacy che ospitano ancora file SWF, dove l'insufficiente validazione dell'input consente al filmato Flash di essere riutilizzato come vettore per Cross-Site Scripting (XSS) o per bypassare la Same-Origin Policy (SOP) tramite configurazioni cross-domain permissive.

| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-CLNT-08 |
| **CWE** | CWE-79 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Media / Alta |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/08-Testing_for_Cross_Site_Flashing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Strumenti:** `JPEXS Free Flash Decompiler`, `FFDec`, `Burp Suite`, `Google Dorks`, `strings`

### Sull'applicazione sono ospitati file Adobe Flash (.swf) legacy?
- [ ] No — non sono ospitati o referenziati file Flash all'interno del perimetro dell'applicazione  
- [ ] Sì — i file SWF sono presenti ma servono solo contenuti statici  
- [ ] Sì — i file SWF sono presenti e accettano parametri dinamici tramite `FlashVars` o stringhe URL  

### L'input passato ai sink ActionScript viene sanificato?
- [ ] Sì — tutti gli input sono rigorosamente validati e i sink ActionScript **non possono** essere manipolati  
- [ ] Sì — la validazione viene applicata ma il bypass **è possibile** tramite specifiche tecniche di codifica  
- [ ] No — l'input viene passato direttamente a sink sensibili come `ExternalInterface.call` o `getURL` *(Critico)*  

### Il file Flash può essere utilizzato per eseguire JavaScript arbitrario (XSS)?
- [ ] No — `ExternalInterface` è **disabilitato** o il parametro `allowScriptAccess` è impostato su `never`  
- [ ] Sì — `allowScriptAccess` è impostato su `sameDomain` ma il file SWF è ospitato sul dominio target  
- [ ] Sì — `allowScriptAccess` è impostato su `always`, consentendo XSS da qualsiasi dominio  

### La policy `crossdomain.xml` impedisce richieste cross-origin non autorizzate?
- [ ] Sì — `crossdomain.xml` è restrittivo e consente solo origini specifiche e fidate  
- [ ] No — il file `crossdomain.xml` **è mancante**  
- [ ] Sì — la policy è eccessivamente permissiva (es. `<allow-access-from domain="*" />`)  

### È possibile forzare il file SWF a caricare filmati esterni controllati da un attaccante?
- [ ] No — l'applicazione **non può** essere forzata a caricare file SWF esterni  
- [ ] Sì — le funzioni `loadMovie` o `loadMovieNum` accettano URL esterni non validati  

---