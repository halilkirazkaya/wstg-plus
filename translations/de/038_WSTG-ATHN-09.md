## WSTG-ATHN-09 — Prüfung auf Schwachstellen in den Funktionen zur Passwortänderung oder zum Zurücksetzen von Passwörtern

Mechanismen zur Passwortänderung und zum Zurücksetzen von Passwörtern sind kritische Komponenten der Authentifizierungsverwaltung. Bei unsachgemäßer Implementierung ermöglichen sie Angreifern die Übernahme von Benutzerkonten. Diese Schwachstellen umfassen in der Regel vorhersehbare Reset-Tokens, fehlendes Rate Limiting oder Kontosperren bei Brute Force Versuchen, eine unsichere Übermittlung von Tokens oder die Möglichkeit, Passwörter zu ändern, ohne das aktuelle Passwort zu verifizieren. Angreifer nutzen diese Mängel aus, indem sie Wiederherstellungs-Links abfangen oder erraten, Host Header Injection durchführen, um Reset-E-Mails umzuleiten, oder Session Fixation anwenden, um den Zugriff nach einer Passwortänderung aufrechtzuerhalten. Die Sicherstellung, dass diese Prozesse eine starke Verifizierung erfordern und kryptografische Zufälligkeit nutzen, ist für die Aufrechterhaltung der Kontointegrität und die Verhinderung unbefugter Zugriffe unerlässlich.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-ATHN-09 |
| **CWE** | CWE-640 |
| **Teststatus** | Nicht durchgeführt |
| **Kritikalität** | Hoch / Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/09-Testing_for_Weak_Password_Change_or_Reset_Functionalities  
* https://hacktricks.wiki/en/pentesting-web/reset-password.html  

**Werkzeuge:** `Burp Suite (Repeater/Intruder)`, `Python (Custom Scripts)`, `CyberChef`, `OAST (Interactsh/Burp Collaborator)`

### Wird bei einer Standardänderung das aktuelle Passwort benötigt, um ein neues Passwort festzulegen?
- [ ] Ja — das aktuelle Passwort **wird angefordert** und verifiziert  
- [ ] Ja — das aktuelle Passwort **wird angefordert**, kann jedoch durch Parameter Pollution oder Manipulation umgangen werden  
- [ ] Nein — das aktuelle Passwort **wird nicht** angefordert, was eine Kontoübernahme via Session Hijacking ermöglicht *(Hoch)*  

### Sind die Passwort-Reset-Tokens kryptografisch sicher und nicht vorhersehbar?
- [ ] Ja — Tokens weisen eine hohe Entropie auf, sind zufällig und **nicht vorhersehbar**  
- [ ] Ja — Tokens werden generiert, weisen jedoch eine geringe Entropie oder vorhersehbare Muster auf (z. B. zeitstempelbasiert)  
- [ ] Nein — Tokens sind **vorhersehbar** oder basieren auf statischen Benutzerdaten wie Base64-kodierten E-Mails/IDs *(Kritisch)*  

### Kann der Passwort-Reset-Link mittels Host Header Injection umgeleitet werden?
- [ ] Nein — die Anwendung ignoriert den `Host`-Header bei der Link-Generierung oder validiert diesen  
- [ ] Ja — die Anwendung nutzt den `Host`- oder `X-Forwarded-Host`-Header zur Erstellung von Links, jedoch findet eine Validierung **statt**  
- [ ] Ja — Links **können** durch Header-Manipulation auf eine vom Angreifer kontrollierte Domain umgeleitet werden *(Hoch)*  

### Verfügt das Reset-Token über eine begrenzte Lebensdauer und wird es sofort ungültig gemacht?
- [ ] Ja — Tokens laufen schnell ab und werden **unmittelbar** nach der Verwendung oder Passwortänderung invalidiert  
- [ ] Ja — Tokens laufen erst nach langer Zeit ab (z. B. 24+ Stunden) oder ermöglichen eine **Mehrfachverwendung**  
- [ ] Nein — Tokens **laufen niemals ab** oder bleiben gültig, nachdem das Passwort erfolgreich geändert wurde  

### Existieren Rate Limiting oder Kontosperren für die Endpunkte zum Zurücksetzen und Ändern von Passwörtern?
- [ ] Ja — striktes Rate Limiting und CAPTCHAs **werden angewendet**, um Enumeration und Brute Force zu verhindern  
- [ ] Ja — es existieren begrenzte Kontrollmechanismen, aber ein Bypass **ist möglich** via IP-Rotation oder Header-Spoofing  
- [ ] Nein — es wird **kein Rate Limiting** angewendet, was Massen-Enumeration von Konten oder Token-Brute-Forcing ermöglicht  

---