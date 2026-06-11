## WSTG-ATHZ-02 — Prüfung auf Umgehung des Autorisierungsschemas

Eine Umgehung des Autorisierungsschemas tritt auf, wenn eine Anwendung die Zugriffskontrollen nicht korrekt erzwingt, wodurch ein Angreifer auf Ressourcen oder Funktionen zugreifen kann, die außerhalb seiner vorgesehenen Berechtigungen liegen. Diese Schwachstelle äußert sich typischerweise als horizontale Privilege Escalation, bei der ein Angreifer auf Daten eines anderen Benutzers mit demselben Rang zugreift, oder als vertikale Privilege Escalation, bei der ein Benutzer mit geringen Privilegien administrative Funktionen erlangt. Angreifer nutzen diese Mängel aus, indem sie Anfrageparameter wie Ressourcen-IDs manipulieren, Session-Token modifizieren oder HTTP-Header wie `X-Original-URL` verwenden, um pfadbasierte Einschränkungen zu umgehen. Die Gewährleistung einer robusten Autorisierung ist entscheidend für die Wahrung der Vertraulichkeit von Daten und die Verhinderung unbefugter administrativer Aktionen, die das gesamte System kompromittieren könnten.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-ATHZ-02 |
| **CWE** | CWE-285 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/02-Testing_for_Bypassing_Authorization_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**Tools:** `Burp Suite (Autorize extension)`, `Burp Suite (Repeater/Intruder)`, `Postman`, `cURL`, `OWASP ZAP`

### Wird horizontale Privilege Escalation zwischen Benutzern derselben Rolle verhindert?
- [ ] Ja — Autorisierungsprüfungen **werden** bei jeder Ressourcenanfrage angewendet und eine Umgehung ist **nicht möglich**  
- [ ] Ja — Autorisierungsprüfungen **werden angewendet**, können aber über IDOR oder Parameter Tampering umgangen werden  
- [ ] Nein — Benutzer **können** auf Daten anderer Benutzer zugreifen, indem sie einfach eine Ressourcen-ID ändern *(Hoch)*  

### Wird vertikale Privilege Escalation für Benutzer mit geringen Privilegien verhindert?
- [ ] Nein — die Anwendung hat nur eine Berechtigungsstufe  
- [ ] Ja — auf administrative Funktionen **kann nicht** von Benutzern mit geringen Privilegien zugegriffen werden  
- [ ] Ja — administrative Funktionen sind in der Benutzeroberfläche ausgeblendet, **können** aber über eine direkte URL aufgerufen werden  
- [ ] Nein — Benutzer mit geringen Privilegien **können** administrative Aktionen durch die Manipulation von Rollen oder Berechtigungen durchführen *(Kritisch)*  

### Kann die Autorisierung mittels HTTP Verb Tampering umgangen werden?
- [ ] Nein — Zugriffskontrollen werden unabhängig von der verwendeten HTTP-Methode erzwungen  
- [ ] Ja — das Ändern der Methode (z. B. von `GET` zu `POST` oder `PUT`) **umgeht** die Autorisierungsprüfungen  

### Sind administrative oder eingeschränkte Endpunkte ohne Authentifizierung zugänglich?
- [ ] Nein — alle sensiblen Endpunkte erfordern eine gültige Sitzung und entsprechende Berechtigungen  
- [ ] Ja — einige administrative Endpunkte sind zugänglich, wenn der Angreifer die direkte URL kennt  
- [ ] Ja — ein nicht authentifizierter Zugriff auf sensible Funktionen **ist möglich** *(Kritisch)*  

### Ermöglichen HTTP-Header die Umgehung einer pfadbasierten Autorisierung?
- [ ] Nein — die Anwendung ist **nicht** anfällig für Header-basierte Bypasses  
- [ ] Ja — Header wie `X-Original-URL`, `X-Rewrite-URL` oder `X-Forwarded-For` **können** verwendet werden, um Zugriffskontrollen zu umgehen  

---