## WSTG-INPV-19 — Testen op Server-Side Request Forgery (SSRF)

Server-Side Request Forgery vindt plaats wanneer een aanvaller een webapplicatie kan beïnvloeden om ongeautoriseerde verzoeken te doen vanaf de server naar interne of externe bronnen. Door parameters te manipuleren die URL's, hostnamen of IP-adressen verwachten, kan een aanvaller controles op netwerkniveau, zoals firewalls en ACL's, omzeilen om interne netwerksegmenten te scannen, toegang te krijgen tot cloud metadata services (IMDS) of interactie te hebben met kwetsbare interne API's en databases. Deze kwetsbaarheid komt doorgaans voor in functionaliteiten voor het ophalen van afbeeldingen, PDF-generatie, webhooks of bestandsuploads via URL. Vanuit het perspectief van een aanvaller is SSRF een krachtig primitief dat wordt gebruikt om te pivoten naar geïsoleerde omgevingen, gevoelige configuratiegegevens te exfiltreren of mogelijk remote code execution te bereiken via interactie met interne services zoals Redis of Memcached.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INPV-19 |
| **CWE** | CWE-918 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog / Kritiek |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/19-Testing_for_Server-Side_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/ssrf-server-side-request-forgery/index.html  
* https://portswigger.net/web-security/ssrf  

**Tools:** `Burp Suite (Collaborator/Interactsh)`, `Interact.sh`, `Gopherus`, `ffuf`, `cURL`, `DNSRebind Tool`

### Verwerkt de applicatie door de gebruiker opgegeven URL's of hostnamen?
- [ ] Nee — geen enkele functionaliteit accepteert externe URL's of hostnamen  
- [ ] Ja — functionaliteit bestaat, maar invoer **wordt strikt gevalideerd** tegen een allow-list  
- [ ] Ja — functionaliteit bestaat en accepteert door de gebruiker opgegeven URL's  

### Worden er validatiecontroles of zwarte lijsten toegepast op de opgevraagde bestemming?
- [ ] Ja — een strikte **allow-list** van vertrouwde domeinen/IP's wordt afgedwongen  
- [ ] Ja — een **deny-list** wordt gebruikt (bijv. het blokkeren van `127.0.0.1`, `169.254.169.254`)  
- [ ] Nee — er wordt **geen validatie** of filtering toegepast op de invoer  

### Is het mogelijk om filters te omzeilen via encoding, redirects of DNS rebinding?
- [ ] Nee — filters zijn robuust en verwerken redirects/DNS-resolutie op een veilige manier  
- [ ] Ja — bypass **is mogelijk** met behulp van URL-encoding of alternatieve IP-formaten (hex, octaal)  
- [ ] Ja — bypass **is mogelijk** via 3xx HTTP-redirects naar interne doelen  
- [ ] Ja — bypass **is mogelijk** via DNS rebinding-aanvallen om te herleiden naar interne IP-adressen  

### Kan de applicatie interne metadata services of lokale interfaces bereiken?
- [ ] Nee — lokaal netwerk en cloud metadata endpoints zijn **niet bereikbaar**  
- [ ] Ja — toegang tot localhost (`127.0.0.1`) of loopback-services **is mogelijk**  
- [ ] Ja — toegang tot cloud metadata services (bijv. AWS/GCP/Azure IMDS) **is mogelijk** *(Kritiek)*  

### Wat is de aard van de SSRF-respons?
- [ ] Blind — er wordt geen respons of metadata teruggestuurd naar de gebruiker  
- [ ] Gedeeltelijk — responsmetadata (headers, grootte, timing) wordt teruggestuurd naar de gebruiker  
- [ ] Volledig — de volledige response body van de interne bron **wordt gereflecteerd** naar de gebruiker  

---