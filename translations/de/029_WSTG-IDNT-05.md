## WSTG-IDNT-05 — Prüfung auf schwache oder nicht erzwungene Benutzernamen-Richtlinien

Die Prüfung auf schwache oder nicht erzwungene Benutzernamen-Richtlinien (Username Policies) konzentriert sich auf die Regeln zur Erstellung von Kontoidentifikatoren und deren Anfälligkeit für Enumeration oder Spoofing. Schwache Richtlinien erlauben oft vorhersehbare Sequenzen, gängige Wörterbuchbegriffe oder Identifikatoren, die interne Mitarbeiter-IDs widerspiegeln, was den Suchraum für Brute Force- und Credential Stuffing-Angriffe erheblich verkleinert. Aus der Sicht eines Angreifers ist die Identifizierung dieser Muster der erste Schritt zur massenhaften Entdeckung von Konten, was oft durch die Analyse von Fehlermeldungen bei der Registrierung, dem Verhalten bei Passwort-Resets oder Metadaten in öffentlich zugänglichen Profilen erreicht wird. Die Gewährleistung einer robusten Richtlinie verhindert, dass Angreifer die Benutzerbasis systematisch erfassen können, und erleichtert die Abwehr gezielter Phishing-Versuche sowie automatisierter Versuche zum Authentication Bypass.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-IDNT-05 |
| **CWE** | CWE-521 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Niedrig / Mittel |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/05-Testing_for_Weak_or_Unenforced_Username_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Intruder)`, `ffuf`, `Custom Python Scripts`, `theHarvester`

### Basieren Benutzernamen auf hochgradig vorhersehbaren oder sequentiellen Mustern?
- [ ] Nein — Benutzernamen sind zufällig, benutzerdefiniert oder Strings mit hoher Entropie  
- [ ] Ja — Benutzernamen folgen einem vorhersehbaren Format (z. B. `user1001`, `user1002`), aber die Authentifizierung ist robust  
- [ ] Ja — Benutzernamen sind **streng sequentiell** oder folgen einem bekannten Unternehmensformat, was eine einfache Enumeration ermöglicht  

### Erzwingt die Anwendung Anforderungen an Mindestlänge und Komplexität für Benutzernamen?
- [ ] Ja — strikte Richtlinien für Länge und Zeichensatz **werden erzwungen** während der Registrierung  
- [ ] Ja — Richtlinien existieren, erlauben jedoch extrem kurze (z. B. 1–2 Zeichen) oder zu einfache Benutzernamen  
- [ ] Nein — **keine Richtlinie** wird bezüglich der Länge des Benutzernamens oder der Zeichentypen erzwungen  

### Können gültige Benutzernamen durch Anwendungsantworten enumeriert werden?
- [ ] Nein — Anwendung gibt generische Fehlermeldungen zurück und weist ein konsistentes Zeitverhalten für alle Versuche auf  
- [ ] Ja — Anwendung gibt unterschiedliche Fehlermeldungen zurück (z. B. „Benutzername bereits vergeben“), aber Rate Limiting **wird angewendet**  
- [ ] Ja — Anwendung **leakt** gültige Benutzernamen durch Registrierungsfehler, Passwort-Resets oder Zeitunterschiede  

### Verhindert die Richtlinie die Registrierung gängiger oder reservierter administrativer Benutzernamen?
- [ ] Ja — Anwendung blockiert gängige Namen (z. B. `admin`, `root`, `support`) und systemreservierte Begriffe  
- [ ] Nein — Benutzer **können** Konten mit sensiblen oder administrativen Namen registrieren  

### Ist die Benutzernamen-Richtlinie über alle Einstiegspunkte (API, Mobile, Web) hinweg konsistent?
- [ ] Ja — Richtlinien **werden konsistent** über alle Schnittstellen und Versionen hinweg angewendet  
- [ ] Nein — Legacy-Endpunkte oder spezifische API-Versionen **erzwingen nicht** dieselben Einschränkungen für Benutzernamen  

---