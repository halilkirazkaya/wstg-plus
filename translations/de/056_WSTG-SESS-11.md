## WSTG-SESS-11 — Prüfung auf gleichzeitige Sitzungen (Testing for Concurrent Sessions)

Die Prüfung auf gleichzeitige Sitzungen (Concurrent Sessions) bewertet, ob eine Anwendung es einem einzelnen Benutzerkonto ermöglicht, mehrere simultane aktive Sitzungen über verschiedene Browser, Geräte oder IP-Adressen hinweg aufrechtzuerhalten. Das Versäumnis, gleichzeitige Sitzungen zu beschränken, vergrößert das Zeitfenster für Angreifer, um gekaperte Session Tokens oder kompromittierte Anmeldedaten zu nutzen, ohne den rechtmäßigen Benutzer zu alarmieren oder bestehende Sitzungen zu beenden. Dieses Verhalten wird in der Regel durch Authentifizierung von mehreren Quellen und die Überwachung der Sitzungsstabilität bewertet, was häufig Schwachstellen in der Sitzungsmanagement-Logik oder fehlende sicherheitsorientierte Geschäftsregeln aufdeckt. Aus der Sicht eines Angreifers ermöglicht das Fehlen einer Concurrency Control eine unauffällige Persistenz nach einer ersten Kompromittierung und erleichtert parallele automatisierte Angriffe.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-SESS-11 |
| **CWE** | CWE-613 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Niedrig / Mittel |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/11-Testing_for_Concurrent_Sessions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Repeater/Containers)`, `Firefox Multi-Account Containers`, `Postman`, `cURL`

### Werden gleichzeitige Sitzungen durch die Anwendungsrichtlinie begrenzt?
- [ ] Ja — nur eine Sitzung **ist zulässig** pro Benutzer; neue Logins machen alte ungültig *(Am sichersten)*  
- [ ] Ja — eine feste Anzahl gleichzeitiger Sitzungen **wird erzwungen** und kann nicht überschritten werden  
- [ ] Nein — eine unbegrenzte Anzahl gleichzeitiger Sitzungen **ist möglich**  

### Wie reagiert die Anwendung, wenn das Sitzungslimit erreicht ist?
- [ ] Die älteste aktive Sitzung **wird automatisch beendet** (Mitigation von Session Fixation/Hijacking)  
- [ ] Der Benutzer **wird aufgefordert**, bestehende Sitzungen zu beenden, bevor die neue Sitzung autorisiert wird  
- [ ] Der neue Login-Versuch **wird blockiert**, bis eine manuelle Abmeldung von einem anderen Gerät erfolgt  
- [ ] Keine Maßnahmen — mehrere Sitzungen bleiben gleichzeitig **aktiviert**  

### Bietet die Anwendung eine Schnittstelle zur Sitzungsverwaltung für den Benutzer an?
- [ ] Ja — Benutzer **können** aktive Sitzungen einsehen (IP, Gerät, Zeit) und diese remote beenden  
- [ ] Ja — Benutzer **können** aktive Sitzungen einsehen, diese aber **nicht** remote beenden  
- [ ] Nein — Benutzer **können** keine anderen aktiven Sitzungen ihres Kontos sehen oder verwalten  

### Wird der Benutzer benachrichtigt, wenn gleichzeitige Login-Aktivitäten erkannt werden?
- [ ] Ja — ein Alarm (E-Mail, SMS oder In-App) **wird ausgelöst** bei Logins von neuen Geräten oder Standorten  
- [ ] Nein — es **erfolgt keine Benachrichtigung**, wenn gleichzeitige Sitzungen aufgebaut werden  

---