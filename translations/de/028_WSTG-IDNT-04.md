## WSTG-IDNT-04 — Prüfung auf Account-Enumeration und erratbare Benutzerkonten

Account-Enumeration tritt auf, wenn eine Anwendung die Existenz oder Nichtexistenz eines Benutzerkontos durch Variationen in HTTP-Antworten, Fehlermeldungen oder messbare Zeitunterschiede preisgibt. Angreifer nutzen dieses Verhalten aus, indem sie systematisch Listen von Benutzernamen übermitteln, um valide Konten zu identifizieren, die anschließend Ziel von Credential Stuffing, Brute-Force-Angriffen oder Social Engineering werden. Diese Schwachstelle tritt häufig in Login-Portalen, Funktionen zum Zurücksetzen von Passwörtern, Registrierungsformularen und API-Endpunkten auf, die benutzerspezifische Identifikatoren verarbeiten. Aus Sicht eines Angreifers verringert eine erfolgreiche Enumeration die Angriffsfläche erheblich, indem sie einen blinden Brute-Force-Versuch in eine gezielte Ausnutzung bekannter, valider Identitäten verwandelt.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-IDNT-04 |
| **CWE** | CWE-204 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Niedrig / Mittel |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/04-Testing_for_Account_Enumeration_and_Guessable_User_Account  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Intruder/Comparer)`, `ffuf`, `Wfuzz`, `GoBuster`, `Hashcat`

### Sind die Authentifizierungs-Fehlermeldungen in allen Szenarien konsistent?
- [ ] Ja — generische Fehlermeldungen werden sowohl für ungültige Benutzernamen als auch für ungültige Passwörter verwendet  
- [ ] Ja — die Meldungen sind ähnlich, aber geringfügige Abweichungen in der Interpunktion oder Groß-/Kleinschreibung **sind vorhanden**  
- [ ] Nein — Fehlermeldungen geben explizit „Benutzer nicht gefunden“ oder „Falsches Passwort“ an *(Anfällig)*  

### Gibt der Prozess zum Zurücksetzen des Passworts („Passwort vergessen“) die Existenz von Konten preis?
- [ ] Nein — die Anwendung gibt eine generische Meldung zurück, unabhängig davon, ob die E-Mail/der Benutzername existiert  
- [ ] Ja — Kontrollen sind vorhanden, aber ein Bypass **ist möglich** über die Antwortlänge oder Statuscodes  
- [ ] Ja — die Anwendung bestätigt explizit, ob eine E-Mail zum Zurücksetzen gesendet wurde oder ob das Konto **nicht existiert**  

### Ist der Registrierungsprozess gegen User Enumeration geschützt?
- [ ] Nein — die Registrierung gibt keine Informationen preis oder verwendet CAPTCHA/Rate Limiting, um Massenprüfungen zu verhindern  
- [ ] Ja — Kontrollen sind vorhanden, aber ein Bypass **ist möglich** durch Timing-Unterschiede oder Abweichungen in der API-Antwort  
- [ ] Ja — die Anwendung benachrichtigt den Benutzer sofort, wenn der Benutzername oder die E-Mail **bereits vergeben ist**  

### Sind Zeitunterschiede beim Vergleich von validen gegenüber ungültigen Benutzernamen messbar?
- [ ] Nein — die Antwortzeiten sind unabhängig von der Kontogültigkeit konsistent, da Vergleiche in konstanter Zeit (Constant-Time) durchgeführt werden  
- [ ] Ja — Timing-Unterschiede existieren, sind aber zu gering, um zuverlässig über das Netzwerk gemessen zu werden  
- [ ] Ja — messbare Zeitabweichungen **sind vorhanden** (z. B. da das Password-Hashing nur für valide Benutzer ausgeführt wird)  

### Verwendet die Anwendung vorhersagbare oder erratbare Namenskonventionen für Konten?
- [ ] Nein — Konto-Identifikatoren sind zufällig (UUIDs) oder benutzerdefiniert und komplex  
- [ ] Ja — Konto-Identifikatoren folgen einem Muster (z. B. `emp001`, `emp002`) und eine Enumeration **ist möglich**  
- [ ] Ja — Identifikatoren sind sequenzielle Ganzzahlen, was eine vollständige Datenbank-Enumeration **möglich macht**  

---