## WSTG-ATHN-10 — Prüfung auf schwächere Authentifizierung in alternativen Kanälen

Die Prüfung auf schwächere Authentifizierung in alternativen Kanälen umfasst die Identifizierung und Analyse sekundärer Pfade für den Kontozugriff – wie mobile APIs, Workflows zur Passwortwiederherstellung, Helpdesk-Schnittstellen oder Legacy-Subdomains –, die möglicherweise nicht dieselbe Sicherheitsstrenge wie das primäre Webportal aufweisen. Diesen alternativen Kanälen mangelt es häufig an robuster Multi-Factor Authentication (MFA), striktem Rate Limiting oder komplexen Passwortanforderungen, was ein „schwächstes Glied“ schafft, das das gesamte Authentifizierungssystem untergräbt. Angreifer zielen gezielt auf diese übersehenen Einstiegspunkte ab, um Credential Stuffing durchzuführen oder MFA zu umgehen (Bypass), indem sie Diskrepanzen in der Handhabung der Identitätsprüfung durch verschiedene Schnittstellen ausnutzen. Die Gewährleistung der Parität über alle Authentifizierungsoberflächen hinweg ist entscheidend, um unbefugten Zugriff zu verhindern und die Integrität der Benutzersitzung und der Daten zu wahren.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-ATHN-10 |
| **CWE** | CWE-287 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/10-Testing_for_Weaker_Authentication_in_Alternative_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `Postman`, `cURL`, `MobSF`, `Ffuf`

### Sind alternative Authentifizierungskanäle vorhanden und identifiziert?
- [ ] Nein — es existiert nur ein einziger Authentifizierungskanal  
- [ ] Ja — mehrere Kanäle identifiziert (z. B. Mobile-App API, Desktop-Client, Legacy-Portal, SOAP-Dienste)  

### Erzwingen alternative Kanäle die gleiche Sicherheits-Parität wie der primäre Kanal?
- [ ] Ja — alle Kanäle erzwingen **identische** Anforderungen an Passwortkomplexität und MFA  
- [ ] Ja — Kanäle erzwingen Passwortkomplexität, aber MFA ist **nicht erforderlich** oder **kann** umgangen werden  
- [ ] Nein — alternative Kanäle haben **signifikant schwächere** Authentifizierungsanforderungen  

### Sind Rate Limiting und Kontosperren (Account Lockout) über alle Kanäle hinweg konsistent?
- [ ] Ja — Rate Limiting und Sperren werden **konsistent** über alle Endpunkte hinweg erzwungen  
- [ ] Ja — Kontrollen sind vorhanden, aber ein Bypass **ist möglich** auf spezifischen Kanälen (z. B. mobile API)  
- [ ] Nein — Rate Limiting oder Sperren werden **nicht** auf sekundäre Authentifizierungspfade angewendet  

### Kann MFA durch den Wechsel zu einem alternativen Kanal umgangen werden?
- [ ] Nein — MFA ist obligatorisch und **kann nicht** umgangen werden, unabhängig vom Einstiegspunkt  
- [ ] Ja — MFA ist im Webportal erforderlich, wird aber in der mobilen oder Legacy-API **nicht erzwungen**  

### Sind Workflows zur Passwortwiederherstellung oder „Passwort vergessen“-Funktionen schwächer als der Standard-Login?
- [ ] Nein — Wiederherstellungs-Workflows nutzen eine starke Out-of-Band-Verifizierung und lassen keine Informationen abfließen  
- [ ] Ja — Wiederherstellungs-Workflows nutzen schwache Sicherheitsfragen, die leicht erraten oder recherchiert werden **können**  
- [ ] Ja — Wiederherstellungs-Workflows sind anfällig für Enumeration oder **erfordern nicht** das gleiche Maß an Identitätsnachweis wie ein Standard-Login  

---