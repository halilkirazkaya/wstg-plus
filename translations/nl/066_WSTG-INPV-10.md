## WSTG-INPV-10 — IMAP SMTP Injection

IMAP- en SMTP-injectiekwetsbaarheden ontstaan wanneer een webapplicatie door de gebruiker verstrekte gegevens onvoldoende filtert voordat deze worden opgenomen in commando's die naar een mailserver worden verzonden. Door Carriage Return en Line Feed (CRLF) karakters te injecteren, kan een aanvaller het beoogde commando beëindigen en willekeurige mailinstructies toevoegen, zoals het toevoegen van extra ontvangers (CC/BCC) of het wijzigen van de berichtinhoud. Deze kwetsbaarheden worden het vaakst aangetroffen in web-to-mail gateways, contactformulieren en e-mailclients die rechtstreeks met mailservers communiceren via protocollen zoals IMAP4 of SMTP. Succesvolle exploitatie maakt het ongeautoriseerd doorsturen (relaying) van spam mogelijk, evenals de exfiltratie van gevoelige e-mailinhoud en, in sommige gevallen, de volledige compromittering van de integriteit van de mailserver.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-INPV-10 |
| **CWE** | CWE-93 |
| **Test Status** | Niet uitgevoerd |
| **Ernst** | Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/10-Testing_for_IMAP_SMTP_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Repeater/Intruder)`, `netcat`, `telnet`, `nmap`

### Verwerkt de applicatie door de gebruiker verstrekte invoer om IMAP- of SMTP-commando's op te stellen?
- [ ] Nee — de applicatie communiceert **niet** met mailservers via gebruikersinvoer  
- [ ] Ja — de applicatie communiceert met mailservers maar gebruikt een veilige API of library  
- [ ] Ja — de applicatie stelt ruwe (raw) mailcommando's op met parameters die door de gebruiker worden gecontroleerd  

### Wordt invoer gevalideerd om de injectie van Carriage Return (`\r`) en Line Feed (`\n`) sequenties te voorkomen?
- [ ] Ja — CRLF-karakters worden **strikt** gefilterd, gecodeerd of geweigerd  
- [ ] Ja — invoer wordt gevalideerd tegen een strikte allowlist van karakters  
- [ ] Nee — CRLF-karakters worden **niet** gefilterd, wat commandobeëindiging en injectie mogelijk maakt  

### Kan een aanvaller aanvullende mailheaders zoals `Bcc:` of `Cc:` injecteren?
- [ ] Nee — het wijzigen van headers is **niet mogelijk** vanwege strikte invoerbeperkingen  
- [ ] Ja — injectie van aanvullende headers **is mogelijk** via CRLF-sequenties  
- [ ] Ja — mail relaying naar externe domeinen **is ingeschakeld** en exploiteerbaar *(Kritiek)*  

### Staat de applicatie de manipulatie van IMAP-commando's toe om toegang te krijgen tot ongeautoriseerde mailboxen?
- [ ] Nee — de applicatie gebruikt **geen** IMAP of beperkt de toegang tot de mailbox strikt tot de geauthenticeerde gebruiker  
- [ ] Ja — IMAP-commando-injectie **is mogelijk** maar beperkt tot metadata-enumeratie  
- [ ] Ja — IMAP-commando-injectie maakt het lezen of verwijderen van e-mails van andere gebruikers mogelijk  

### Is de backend mailserver kwetsbaar voor command pipelining?
- [ ] Nee — de mailserver weigert gebatchte commando's of meerdere commando's in een enkele sessie  
- [ ] Ja — de mailserver verwerkt meerdere commando's die via een enkel invoerveld zijn geïnjecteerd  

---