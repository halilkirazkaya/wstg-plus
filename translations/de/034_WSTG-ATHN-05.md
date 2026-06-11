## WSTG-ATHN-05 — Prüfung auf Schwachstellen in der "Passwort merken"-Funktion

Die "Remember Me"- oder persistente Login-Funktionalität dient dazu, den authentifizierten Status eines Benutzers über Browser-Neustarts hinweg beizubehalten, indem ein Token oder ein Credential auf der Client-Seite gespeichert wird. Wenn diese Funktion mangelhaft implementiert ist — etwa durch die Speicherung von Klartext-Passwörtern, schwachen Hashes oder Tokens mit geringer Entropie in Cookies — setzt sie den Benutzer dem Risiko einer Kontoübernahme (Account Takeover) durch Cross-Site Scripting (XSS), physischen Zugriff oder Netzwerk-Interception aus. Pentester bewerten die Entropie dieser Persistenz-Tokens, die auf den Speichermechanismus angewendeten Security Flags und den Lebenszyklus der Sitzung, um sicherzustellen, dass die Bequemlichkeit des persistenten Zugriffs keine kritischen Sicherheitskontrollen wie die Multi-Faktor-Authentifizierung (MFA) oder die Sitzungsinvalidierung umgeht. Die Ausnutzung beinhaltet häufig das Harvesting dieser langlebigen Tokens, um unbefugten Zugriff auf Konten zu erlangen, ohne die ursprünglichen Zugangsdaten zu benötigen.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-ATHN-05 |
| **CWE** | CWE-522 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch* |

> *Der Schweregrad wird als "Hoch" eingestuft, wenn Klartext-Zugangsdaten oder ungesalzene Hashes im persistenten Cookie gespeichert werden oder wenn das Token die Multi-Faktor-Authentifizierung (MFA) umgeht.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/05-Testing_for_Vulnerable_Remember_Password  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  
* https://portswigger.net/web-security/authentication  

**Tools:** `Burp Suite (Cookie Editor/Sequencer)`, `Browser Developer Tools`, `EditThisCookie`

### Implementiert die Anwendung eine "Remember Me"- oder "Angemeldet bleiben"-Funktion?
- [ ] Nein — Funktion ist in der Anwendung **nicht** vorhanden  
- [ ] Ja — Funktion existiert und verwendet kryptografisch starke, nicht vorhersehbare Tokens  
- [ ] Ja — Funktion existiert, verwendet jedoch vorhersehbare Tokens oder solche mit geringer Entropie *(Mittel)*  
- [ ] Ja — Funktion existiert und speichert Klartext-Zugangsdaten oder Base64-kodierte Passwörter *(Hoch)*  

### Sind die Security Flags korrekt auf das persistente Cookie angewendet?
- [ ] Ja — Die Flags `HttpOnly`, `Secure` und `SameSite` sind **gesetzt**  
- [ ] Ja — Einige Flags sind gesetzt, aber `HttpOnly` **fehlt**  
- [ ] Ja — Einige Flags sind gesetzt, aber `Secure` **fehlt** über HTTPS  
- [ ] Nein — Es sind **keine** Security Flags auf das persistente Cookie angewendet  

### Bleibt das "Remember Me"-Token nach einer manuellen Abmeldung (Logout) gültig?
- [ ] Nein — Das Token wird serverseitig sofort nach dem Logout entwertet  
- [ ] Ja — Das Token bleibt gültig, erfordert jedoch ein Passwort für sensible Aktionen  
- [ ] Ja — Das Token bleibt gültig und ermöglicht den vollen Kontozugriff **ohne** erneute Authentifizierung *(Mittel)*  

### Wird die persistente Sitzung nach einer Passwortänderung beendet?
- [ ] Ja — Alle persistenten Tokens werden widerrufen, wenn der Benutzer sein Passwort ändert  
- [ ] Nein — Das "Remember Me"-Token bleibt nach einem Passwort-Reset oder einer Passwortänderung **gültig** *(Hoch)*  

### Umgeht das persistente Token die Multi-Faktor-Authentifizierung (MFA) bei nachfolgenden Besuchen?
- [ ] Nein — MFA ist erforderlich, auch wenn ein "Remember Me"-Token vorhanden ist  
- [ ] Ja — MFA wird umgangen, aber das Token ist an einen spezifischen Geräte-Fingerabdruck (Device Fingerprint) gebunden  
- [ ] Ja — MFA wird unter Verwendung des persistenten Cookies von jedem beliebigen Gerät aus **vollständig umgangen** *(Hoch)*  

---