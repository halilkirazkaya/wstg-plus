## WSTG-ATHN-03 — Prüfung auf schwache Kontosperrmechanismen (Weak Lock Out Mechanism)

Ein Kontosperrmechanismus (Account Lockout Mechanism) ist eine kritische Defense-in-Depth-Maßnahme, die darauf ausgelegt ist, Brute-Force- und Credential-Stuffing-Angriffe abzumildern, indem ein Konto nach einer festgelegten Anzahl fehlgeschlagener Authentifizierungsversuche deaktiviert wird. Ohne eine robuste Lockout-Richtlinie können Angreifer automatisierte Tools verwenden, um systematisch Passwörter über mehrere Konten hinweg zu erraten (Password Spraying) oder ein einzelnes hochwertiges Konto bis zum Erfolg anzugreifen. Diese Schwachstelle tritt typischerweise bei Login-Portalen, Passwort-Reset-Formularen und API-Endpunkten auf, denen Rate Limiting oder ein Schutz auf Kontoebene fehlt. Aus der Sicht eines Angreifers ermöglicht ein schwacher oder fehlender Sperrmechanismus unbegrenzte Passwortversuche, während ein übermäßig aggressiver Mechanismus ausgenutzt werden kann, um einen verteilten Denial of Service (DoS) gegen legitime Benutzer zu verursachen, indem deren Konten absichtlich gesperrt werden.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-ATHN-03 |
| **CWE** | CWE-307 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch* |

> *Der Schweregrad wird als Hoch eingestuft, wenn die Anwendung sensible personenbezogene Daten (PII), Finanzdaten verarbeitet oder wenn administrative Konten ohne sekundäre Kontrollen anfällig für Brute-Force-Angriffe sind.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/03-Testing_for_Weak_Lock_Out_Mechanism  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Tools:** `Burp Suite Intruder`, `THC-Hydra`, `ffuf`, `Patator`, `wfuzz`

### Wird ein Kontosperrmechanismus nach einer festgelegten Anzahl fehlgeschlagener Versuche erzwungen?
- [ ] Ja — das Konto wird nach einem definierten, angemessenen Schwellenwert gesperrt (z. B. 5–10 Versuche)  
- [ ] Ja — das Konto wird gesperrt, jedoch erst nach einer übermäßig hohen Anzahl von Versuchen  
- [ ] Nein — das Konto kann nicht gesperrt werden und ermöglicht zeitlich unbegrenzte Brute-Force-Angriffe  

### Kann der Sperrmechanismus mit gängigen Umgehungstechniken (Evasion Techniques) umgangen werden?
- [ ] Nein — die Sperre wird serverseitig erzwungen und wird nicht durch clientseitige Änderungen beeinflusst  
- [ ] Ja — eine Umgehung der Sperre ist durch die Rotation von IP-Adressen oder die Verwendung eines Proxy-Pools möglich  
- [ ] Ja — eine Umgehung der Sperre ist durch Manipulation von HTTP-Headern wie `X-Forwarded-For` oder `User-Agent` möglich  
- [ ] Ja — eine Umgehung der Sperre ist durch das Löschen von Cookies oder das Starten neuer Sitzungen möglich  

### Bietet der Sperrmechanismus einen Pfad zur Wiederherstellung des Kontos oder zum Ablauf der Sperre?
- [ ] Ja — das Konto wird nach einer festgelegten Dauer automatisch oder per E-Mail-Verifizierung entsperrt  
- [ ] Ja — zum Entsperren des Kontos ist das Eingreifen eines Administrators erforderlich  
- [ ] Nein — die Sperre ist dauerhaft ohne klaren Wiederherstellungspfad, was das DoS-Risiko erhöht  

### Unterscheidet die Anwendung während der Sperrung zwischen gültigen und ungültigen Benutzernamen?
- [ ] Nein — die Antwort der Anwendung ist für existierende und nicht existierende Benutzer identisch  
- [ ] Ja — die Anwendung gibt preis, ob ein Konto gesperrt ist, was eine Benutzernamen-Aufzählung (Username Enumeration) ermöglicht  

### Ist der Sperrmechanismus anfällig für einen Denial of Service (DoS) Angriff?
- [ ] Nein — Kontrollen wie CAPTCHA oder IP-basiertes Rate Limiting verhindern das massenhafte Sperren von Konten  
- [ ] Ja — ein Angreifer kann systematisch alle bekannten Benutzer sperren, indem er falsche Passwörter angibt  

---