## WSTG-ATHN-01 — Testen op het transport van inloggegevens via een versleuteld kanaal

Het testen op het transport van inloggegevens via een versleuteld kanaal waarborgt dat gevoelige authenticatiegegevens, zoals gebruikersnamen, wachtwoorden en sessietokens, beschermd zijn tegen onderschepping tijdens verzending. Aanvallers op het netwerk—bijvoorbeeld via Man-in-the-Middle (MitM) aanvallen op openbare Wi-Fi of gecompromitteerde interne netwerken—kunnen packet sniffing tools gebruiken om inloggegevens in cleartext te onderscheppen als TLS/SSL niet correct wordt afgedwongen. Deze kwetsbaarheid doet zich meestal voor bij login-endpoints, wachtwoord-resetformulieren en API-authenticatieheaders waar de applicatie nalaat HTTPS te verplichten of op onjuiste wijze doorverwijst vanaf HTTP. Succesvolle exploitatie geeft de aanvaller volledige toegang tot gebruikersaccounts, wat leidt tot een volledige compromittering van gebruikersgegevens en mogelijke lateral movement binnen de applicatie.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-ATHN-01 |
| **CWE** | CWE-319 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/01-Testing_for_Credentials_Transported_over_an_Encrypted_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Tools:** `Wireshark`, `tcpdump`, `Burp Suite`, `OWASP ZAP`, `testssl.sh`, `sslyze`, `nmap`

### Wordt HTTPS afgedwongen voor het gehele authenticatieproces?
- [ ] Ja — HSTS is **ingeschakeld** en al het verkeer wordt strikt over HTTPS gedwongen  
- [ ] Ja — omleiding naar HTTPS vindt plaats, maar HSTS is **niet ingeschakeld**  
- [ ] Nee — inloggegevens **kunnen** via onversleuteld HTTP worden verzonden  

### Worden inlogpagina's en formulieren aangeboden via een versleuteld kanaal?
- [ ] Ja — de pagina met het inlogformulier wordt via HTTPS geleverd  
- [ ] Nee — de inlogpagina wordt via HTTP geleverd, wat form-action hijacking of credential sniffing mogelijk maakt  

### Is de `Secure` flag toegepast op alle gevoelige sessie-cookies?
- [ ] Ja — de `Secure` flag is **toegepast** op alle authenticatie- en sessie-cookies  
- [ ] Ja — de `Secure` flag is slechts op **sommige** cookies toegepast  
- [ ] Nee — de `Secure` flag is **niet toegepast**, waardoor cookies via onversleutelde verzoeken kunnen worden geëxfiltreerd  

### Ondersteunt de server zwakke TLS-versies of onveilige cipher suites?
- [ ] Nee — alleen moderne TLS (1.2+) en sterke ciphers zijn **ingeschakeld**  
- [ ] Ja — legacy versies (TLS 1.0/1.1) of zwakke ciphers (RC4, DES, CBC) worden **ondersteund**  

### Voorkomt de applicatie de verzending van inloggegevens naar domeinen van derden via onversleutelde kanalen?
- [ ] Ja — er worden geen gevoelige gegevens naar externe domeinen verzonden of alle externe aanroepen worden over HTTPS gedwongen  
- [ ] Nee — authenticatieheaders of inloggegevens worden naar endpoints van derden verzonden (bijv. analytics of SSO) via HTTP  

---