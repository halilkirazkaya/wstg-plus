## WSTG-IDNT-02 — Prüfung des Benutzerregistrierungsprozesses

Die Prüfung des Benutzerregistrierungsprozesses umfasst die Bewertung des Workflows, über den neue Identitäten erstellt werden, um sicherzustellen, dass dieser nicht für unbefugten Zugriff oder automatisierte Ausnutzung missbraucht werden kann. Schwachstellen in diesem Prozess äußern sich häufig durch fehlende Identitätsverifizierung (E-Mail/SMS), Anfälligkeit für Massenerstellung von Konten durch Bots oder Offenlegung von Informationen durch ausführliche Fehlermeldungen, die existierende Benutzernamen preisgeben. Angreifer nutzen diese Mängel aus, um User Enumeration durchzuführen, großflächige Spam-Kampagnen zu betreiben oder Registrierungsparameter zu manipulieren, um während der Phase der Kontoerstellung erweiterte Privilegien zu erlangen. Dieser Test ist entscheidend für den Schutz der Integrität der Anwendung und um zu verhindern, dass Angreifer das System mit betrügerischen oder bösartigen Konten füllen.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-IDNT-02 |
| **CWE** | CWE-836 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/02-Test_User_Registration_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `OWASP ZAP`, `Python`, `ffuf`, `Cuppa`

### Ist eine Identitätsverifizierung erforderlich, bevor ein neues Konto aktiv wird?
- [ ] Ja — die Registrierung erfordert ein eindeutiges, zeitlich begrenztes Token, das per E-Mail oder SMS versendet wird  
- [ ] Ja — eine Verifizierung ist erforderlich, aber die Token sind **schwach** oder **vorhersehbar**  
- [ ] Nein — Konten werden **sofort** ohne Verifizierungsschritt aktiviert  

### Verhindert der Registrierungsprozess User Enumeration?
- [ ] Ja — die Anwendung gibt **identische** Antworten sowohl für neue als auch für bereits existierende E-Mails/Benutzernamen zurück  
- [ ] Nein — Fehlermeldungen (z. B. „E-Mail bereits vergeben“) **ermöglichen** es einem Angreifer, registrierte Benutzer zuzuordnen  
- [ ] Nein — Zeitunterschiede (Timing differences) in den Serverantworten **geben Aufschluss** darüber, ob ein Benutzer existiert  

### Sind Anti-Automatisierungs-Mechanismen implementiert, um Massenregistrierungen zu verhindern?
- [ ] Ja — CAPTCHA- oder Proof-of-Work-Mechanismen sind **vorhanden** und **effektiv**  
- [ ] Ja — Kontrollen existieren, aber ein Bypass **ist möglich** über API-Endpunkte oder Header-Manipulation  
- [ ] Nein — am Registrierungs-Endpunkt wird kein Rate Limiting oder CAPTCHA **angewendet**  

### Können Registrierungsparameter manipuliert werden, um Privilegien zu eskalieren?
- [ ] Nein — Benutzerrollen werden **strikt** durch die serverseitige Logik zugewiesen  
- [ ] Ja — Parameter wie `role`, `admin` oder `group_id` **können** in der Anfrage geändert werden, um höheren Zugriff zu erlangen  

### Wird die Passwortrichtlinie (Password Policy) während der Registrierungsphase erzwungen?
- [ ] Ja — Anforderungen an starke Passwörter werden serverseitig **erzwungen**  
- [ ] Nein — schwache Passwörter (z. B. „password123“) werden vom System **akzeptiert**  

---