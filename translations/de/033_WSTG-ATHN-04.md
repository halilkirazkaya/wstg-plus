## WSTG-ATHN-04 — Prüfung auf Umgehung des Authentifizierungsschemas

Die Umgehung des Authentifizierungsschemas beinhaltet das Identifizieren und Ausnutzen von Fehlern in der Applikationslogik, die es einem Angreifer ermöglichen, Zugriff auf geschützte Ressourcen zu erhalten, ohne gültige Zugangsdaten anzugeben. Diese Schwachstelle tritt typischerweise auf, wenn sich die Anwendung auf clientseitige Kontrollen verlässt, es versäumt, eine serverseitige Autorisierung bei jeder Anfrage zu erzwingen, oder administrative "Backdoors" und Debug-Endpunkte in der Produktionsumgebung exponiert lässt. Angreifer nutzen diese Schwächen aus, indem sie Forced Browsing zu sensiblen URLs durchführen, HTTP-Parameter wie `authenticated=true` manipulieren oder Header fälschen (Spoofing), um die Anwendung vorzutäuschen, dass eine Session bereits etabliert ist. Eine erfolgreiche Ausnutzung führt zu einem vollständigen Zusammenbruch des Authentifizierungsperimeters, was potenziell zu unbefugter Datenexfiltration oder einer vollständigen Systemkompromittierung führen kann.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-ATHN-04 |
| **CWE** | CWE-287 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/04-Testing_for_Bypassing_Authentication_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Tools:** `Burp Suite`, `Ffuf`, `Gobuster`, `Dirsearch`, `Postman`

### Können sensible interne Seiten direkt ohne eine aktive Session aufgerufen werden?
- [ ] Nein — der direkte Zugriff führt zu einer Weiterleitung zum Login oder einer 403/404-Antwort  
- [ ] Ja — einige sensible Seiten sind zugänglich, liefern jedoch nur begrenzte oder unkritische Daten  
- [ ] Ja — sensible administrative oder benutzerspezifische Seiten **sind** ohne Authentifizierung vollständig zugänglich *(Kritisch)*  

### Verlässt sich die Anwendung auf clientseitige Flags oder Parameter, um den Authentifizierungsstatus zu bestimmen?
- [ ] Nein — der Authentifizierungsstatus wird strikt über den serverseitigen Session-Status verwaltet  
- [ ] Ja — clientseitige Flags existieren, aber eine Modifikation gewährt **keinen** Zugriff auf geschützte Bereiche  
- [ ] Ja — die Modifikation von Parametern (z. B. `is_authenticated=true`, `user_role=admin`) **ermöglicht** die Umgehung der Authentifizierung  

### Kann die Authentifizierung durch Spoofing oder Manipulation spezifischer HTTP-Header umgangen werden?
- [ ] Nein — Header wie `X-Forwarded-For`, `Referer` oder benutzerdefinierte Header haben keinen Einfluss auf die Authentifizierung  
- [ ] Ja — die Manipulation von Headern (z. B. `X-Custom-IP-Authorization`, `X-Remote-User`) **ist möglich** und gewährt unbefugten Zugriff  

### Gibt es alternative Pfade oder Entwicklungsartefakte, die den standardmäßigen Login-Flow umgehen?
- [ ] Nein — alle geschützten Ressourcen sind durch eine zentralisierte, produktionsreife Authentifizierungsprüfung abgesichert  
- [ ] Ja — Legacy-Endpunkte, Debug-Routen (z. B. `/debug/login`) oder vergessene API-Versionen **sind** ohne Zugangsdaten zugänglich  

### Versäumt es die Anwendung, bei privilegierten Aktionen eine erneute Authentifizierung oder Session-Verifizierung durchzuführen?
- [ ] Nein — privilegierte oder sensible Aktionen erfordern ein frisches oder gültiges Session-Token  
- [ ] Ja — sobald eine Umgehung an einem Endpunkt gefunden wurde, **kann** diese genutzt werden, um administrative Aktionen in der gesamten Anwendung durchzuführen  

---