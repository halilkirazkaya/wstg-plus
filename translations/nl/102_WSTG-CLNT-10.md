## WSTG-CLNT-10 — WebSockets Testen

WebSocket-testen richt zich op het full-duplex communicatiekanaal dat tot stand is gebracht tussen de client en de server, dat de traditionele HTTP-request-response-patronen omzeilt. Pentesters evalueren de beveiliging van het handshake-proces, de aanwezigheid van Cross-Site WebSocket Hijacking (CSWSH)-beveiligingen en de stritheid van de input-validatie die wordt toegepast op message payloads. Exploitatie omvat doorgaans het overnemen (hijacking) van een actieve sessie om gegevens in real-time te exfiltreren of het injecteren van kwaadaardige payloads die server-side kwetsbaarheden zoals Command Injection of SQLi triggeren via de persistente socket. Aangezien veel traditionele Web Application Firewalls (WAFs) WebSocket-verkeer niet effectief inspecteren, biedt dit protocol vaak een onopvallende vector voor het omzeilen van perimeterbeveiligingen.


| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CLNT-10 |
| **CWE** | CWE-1385 |
| **Teststatus** | Niet Uitgevoerd |
| **Severity** | Hoog / Medium |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/10-Testing_WebSockets  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/websockets  

**Tools:** `Burp Suite (WebSockets tab)`, `OWASP ZAP`, `wscat`, `Socket.io-client`, `Wireshark`

### Wordt de WebSocket-verbinding tot stand gebracht via een beveiligd kanaal?
- [ ] Ja — `wss://` (WebSocket Secure) wordt afgedwongen voor alle verbindingen  
- [ ] Nee — `ws://` wordt gebruikt, wat person-in-the-middle (PITM)-afluisteren mogelijk maakt  

### Is de applicatie beschermd tegen Cross-Site WebSocket Hijacking (CSWSH)?
- [ ] Nee — de server valideert de `Origin`-header strikt tijdens de handshake  
- [ ] Ja — `Origin`-header-validatie is zwak of **kan** worden omzeild via null of gespoofte origins  
- [ ] Ja — er wordt geen `Origin`-header-validatie uitgevoerd, waardoor cross-site aanvallers een verbinding kunnen initiëren *(Kritiek)*  

### Wordt input-validatie en sanitization toegepast op WebSocket message payloads?
- [ ] Ja — alle inkomende berichten worden gesaneerd en gevalideerd tegen een schema  
- [ ] Ja — er bestaat enige validatie, maar omzeiling **is mogelijk** met behulp van gecodeerde of niet-standaard payloads  
- [ ] Nee — berichten worden direct verwerkt, wat injectie (XSS, SQLi, RCE) of logische manipulatie mogelijk maakt  

### Worden authenticatie- en autorisatiecontroles uitgevoerd op elk WebSocket-bericht?
- [ ] Ja — de sessie wordt voor elk bericht geverifieerd of het protocol is stateless met tokens  
- [ ] Ja — authenticatie vindt plaats bij de handshake, maar autorisatie voor specifieke acties **wordt niet** per bericht afgedwongen  
- [ ] Nee — authenticatie wordt alleen gecontroleerd bij de handshake, waardoor session hijacking volledige toegang kan verlenen  

### Implementeert de server rate limiting of resource-beperkingen op de WebSocket?
- [ ] Ja — rate limiting en maximale berichtgroottes zijn **ingeschakeld** en worden afgedwongen  
- [ ] Nee — er bestaan geen limieten, wat Denial of Service (DoS) mogelijk maakt via message flooding of grote payloads  

---