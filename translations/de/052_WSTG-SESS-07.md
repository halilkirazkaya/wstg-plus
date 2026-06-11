## WSTG-SESS-07 — Testen von Session-Timeouts

Die Prüfung des Session-Timeouts bewertet, ob eine Anwendung eine Benutzersitzung nach einem vordefinierten Zeitraum der Inaktivität oder einer festgelegten Gesamtdauer effektiv beendet. Diese Kontrolle ist entscheidend zur Minderung des Risikos von Session Hijacking, insbesondere an gemeinsam genutzten Workstations oder in Szenarien, in denen ein Angreifer einen Session-Identifier durch Netzwerk-Interception oder Cross-Site Scripting (XSS) erlangt. Pentesters analysieren sowohl Idle-Timeouts (Inaktivitäts-Timeouts), die nach einem Zeitraum ohne Benutzerinteraktion ausgelöst werden, als auch absolute Timeouts, welche die Gesamtlebensdauer einer Sitzung unabhängig von der Aktivität begrenzen. Aus der Perspektive eines Angreifers bieten unbefristete oder übermäßig lange Sitzungen ein erweitertes Zeitfenster, um unbefugten Zugriff aufrechtzuerhalten und die Notwendigkeit einer erneuten Authentifizierung zu umgehen.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-SESS-07 |
| **CWE** | CWE-613 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/07-Testing_Session_Timeout  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Browser DevTools`

### Implementiert die Anwendung einen Idle-Session-Timeout?
- [ ] Ja — die Sitzung läuft nach einem kurzen Zeitraum (z. B. 15–30 Minuten) der Inaktivität ab  
- [ ] Ja — die Sitzung läuft ab, aber die Timeout-Dauer ist **übermäßig lang** (z. B. > 24 Stunden)  
- [ ] Nein — die Sitzung **bleibt während der Inaktivität unbegrenzt aktiv**  

### Wird der Session-Timeout serverseitig erzwungen?
- [ ] Ja — der Server lehnt Anfragen mit abgelaufenen Token unabhängig vom Client-seitigen Status ab  
- [ ] Nein — der Timeout wird **nur** über clientseitiges JavaScript oder Meta-Refresh erzwungen  

### Implementiert die Anwendung einen absoluten Session-Timeout?
- [ ] Ja — die Sitzung wird nach einer festen maximalen Dauer unabhängig von der Aktivität beendet  
- [ ] Nein — die Sitzung **kann** unbegrenzt verlängert werden, solange eine kontinuierliche Aktivität besteht  

### Wird der Session-Identifier bei Timeout auf dem Server ungültig gemacht?
- [ ] Ja — der Session-Token **kann nicht** wiederverwendet werden, sobald die Timeout-Schwelle erreicht ist  
- [ ] Nein — der Session-Token **ist auf dem Server weiterhin gültig**, selbst nachdem der Client zur Login-Seite weitergeleitet wurde  

### Kann die Sitzung durch automatisierte Heartbeat-Anfragen unbegrenzt aufrechterhalten werden?
- [ ] Nein — ein absoluter Timeout oder sekundäre Kontrollen **verhindern** eine unendliche Sitzungsverlängerung  
- [ ] Ja — periodische Hintergrundanfragen (z. B. AJAX) **können** die Sitzung unbegrenzt aufrechterhalten  

---