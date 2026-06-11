## WSTG-SESS-06 — Prüfung der Logout-Funktionalität

Die Logout-Funktionalität ist eine kritische Sicherheitskontrolle, die darauf ausgelegt ist, die Sitzung eines Benutzers zu beenden und die zugehörigen Sitzungskennungen (Session Identifier) sowohl clientseitig als auch serverseitig zu entwerten. Eine fehlerhafte Implementierung des Logouts ermöglicht Session Fixation oder Hijacking-Angriffe, da ein Angreifer, der nach dem "Abmelden" eines Benutzers Zugriff auf dessen Rechner erhält, den immer noch aktiven Session Token potenziell wiederverwenden könnte. Pentester bewerten dies, indem sie prüfen, ob das Session Cookie aus dem Browser gelöscht wird, ob der serverseitige Sitzungsstatus explizit zerstört wird und ob die Navigation über die Zurück-Schaltfläche des Browsers den Zugriff auf gecachte sensible Daten ermöglicht. Eine sichere Logout-Implementierung stellt sicher, dass nach Auslösung der Aktion keine nachfolgenden Anfragen mit dem alten Session Token von der Anwendung autorisiert werden.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-SESS-06 |
| **CWE** | CWE-613 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/06-Testing_for_Logout_Functionality  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Tools:** `Burp Suite (Repeater/Proxy)`, `Browser Developer Tools`, `OWASP ZAP`

### Ist ein funktionaler Logout-Mechanismus vorhanden und zugänglich?
- [ ] Ja — Logout-Schaltfläche existiert und löst eine Terminanfrage (Termination Request) aus  
- [ ] Nein — Logout-Schaltfläche **fehlt** oder löst **kein** Terminierungsereignis aus  

### Wird die Sitzungskennung (Session Identifier) serverseitig entwertet?
- [ ] Ja — Der Server lehnt alle nachfolgenden Anfragen ab, die den alten Session Token verwenden  
- [ ] Nein — Der Server **akzeptiert weiterhin** den alten Session Token nach dem Logout  

### Löscht die Anwendung die Session Cookies im Browser beim Logout?
- [ ] Ja — Cookies werden mit einem abgelaufenen Datum oder einem leeren Wert überschrieben  
- [ ] Ja — Cookies bleiben bestehen, sind aber auf dem Server **nicht mehr gültig**  
- [ ] Nein — Cookies verbleiben im Browser und **behalten** ihre ursprünglichen Werte bei  

### Kann auf sensible authentifizierte Inhalte über die 'Zurück'-Schaltfläche des Browsers nach dem Logout zugegriffen werden?
- [ ] Nein — `Cache-Control: no-store` oder ähnliche Header verhindern das Anzeigen sensibler Seiten nach dem Logout  
- [ ] Ja — Sensible Seiten sind gecacht und **können** durch Navigation nach Beendigung der Sitzung eingesehen werden  

---