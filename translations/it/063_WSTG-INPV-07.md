## WSTG-INPV-07 — XML Injection

L'XML Injection si verifica quando un'applicazione incorpora dati forniti dall'utente in un documento o stream XML senza un'adeguata validazione, sanificazione o escaping dei metacaratteri XML. Iniettando specifici tag XML o elementi strutturali, un attaccante può manipolare la logica del documento, bypassare i meccanismi di autenticazione o interferire con l'integrità dei dati elaborati dal backend. Questa vulnerabilità si riscontra comunemente nei web service basati su SOAP, nelle API REST che accettano XML, nelle asserzioni SAML e nelle applicazioni che generano dinamicamente file di configurazione o report XML. Dal punto di vista di un attaccante, l'obiettivo è uscire dal contesto dei dati previsto per alterare la struttura del documento, portando potenzialmente a un'escalation dei privilegi non autorizzata o all'esecuzione di una logica di business non prevista.


| Campo | Valore |
|---|---|
| **WSTG ID** | WSTG-INPV-07 |
| **CWE** | CWE-91 |
| **Stato del Test** | Non Eseguito |
| **Gravità** | Alta / Media |

**Riferimenti:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/07-Testing_for_XML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/xxe  

**Strumenti:** `Burp Suite (Intruder/Repeater)`, `OWASP ZAP`, `XMLSpy`, `CyberChef`

### L'applicazione accetta ed elabora input in formato XML dall'utente?
- [ ] No — l'applicazione **non** elabora input XML  
- [ ] Sì — l'input XML viene elaborato tramite API (SOAP/REST)  
- [ ] Sì — l'XML viene elaborato tramite caricamento di file (SVG, DOCX, ecc.)  

### I metacaratteri XML (es. `<`, `>`, `&`, `'`, `"`) sono correttamente sottoposti a escaping o sanificati?
- [ ] Sì — tutti i metacaratteri sono **correttamente** sottoposti a escaping o sanificati *(Più sicuro)*  
- [ ] Sì — viene applicato del filtraggio ma il bypass **è possibile** utilizzando la codifica  
- [ ] No — l'input grezzo viene inserito direttamente nelle strutture XML **senza** sanificazione  

### La validazione dello schema lato server (XSD/DTD) è applicata per tutti gli input XML?
- [ ] Sì — la validazione rigorosa dello schema è **abilitata** e impedisce modifiche strutturali  
- [ ] Sì — la validazione è **abilitata** ma **non** impedisce l'iniezione di tag all'interno dei campi dati  
- [ ] No — la validazione dello schema è **disabilitata** o non implementata  

### Un attaccante può iniettare nuovi tag XML per alterare la struttura o la logica del documento?
- [ ] No — la struttura rimane intatta indipendentemente dall'input  
- [ ] Sì — i tag possono essere iniettati ma **non possono** alterare la logica di business  
- [ ] Sì — l'iniezione di tag **è possibile** e consente il bypass della logica o la modifica dei dati *(Critico)*  

### Il parser XML può essere manipolato utilizzando sezioni CDATA o commenti XML?
- [ ] No — il parser gestisce correttamente o rifiuta i marcatori CDATA/commenti  
- [ ] Sì — l'iniezione di `<![CDATA[...]]>` o `<!-- ... -->` **è possibile** per nascondere o bypassare i filtri  

---