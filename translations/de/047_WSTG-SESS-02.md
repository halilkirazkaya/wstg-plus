## WSTG-SESS-02 — Prüfung der Cookie-Attribute

Das Session-Management stützt sich häufig auf HTTP-Cookies, um den Status aufrechtzuerhalten, was die Sicherheitsattribute dieser Cookies zu einem primären Ziel für Angreifer macht. Durch das Weglassen oder die Fehlkonfiguration von Attributen wie `HttpOnly`, `Secure` und `SameSite` setzen Anwendungen Session-Token dem Diebstahl durch Cross-Site Scripting (XSS), dem Abfangen über unverschlüsselte Kanäle oder Cross-Site Request Forgery (CSRF)-Angriffen aus. Pentester untersuchen diese Flags, um festzustellen, ob ein Angreifer Session-Identifikatoren aus dem Document Object Model (DOM) des Browsers extrahieren oder den Browser dazu zwingen kann, diese in Cross-Origin-Anfragen zu senden. Eine korrekte Attributkonfiguration ist eine grundlegende Defense-in-Depth-Maßnahme, um sicherzustellen, dass sensible Token auf autorisierte, sichere Kontexte beschränkt bleiben.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-SESS-02 |
| **CWE** | CWE-614 |
| **Teststatus** | Nicht durchgeführt |
| **Kritikalität** | Mittel |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/02-Testing_for_Cookies_Attributes  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Tools:** `Burp Suite (Proxy/Scanner)`, `OWASP ZAP`, `Browser Developer Tools (Storage Tab)`, `EditThisCookie`, `curl`

### Analyse der Implementierung von Cookie-Flags
| Attribut | Vorhanden | Fehlt |
|-----------|:-------:|:-------:|
| `HttpOnly` |  |  |
| `Secure` |  |  |
| `SameSite` |  |  |

### Wird das `HttpOnly`-Flag auf sensible Session-Cookies angewendet?
- [ ] Ja — auf **alle** sensiblen Session-Cookies angewendet *(Am sichersten)*  
- [ ] Ja — nur auf einige Session-Cookies angewendet  
- [ ] Nein — das Flag wird **nicht angewendet**, was den Zugriff über clientseitige Skripte ermöglicht *(Kritisch)*  

### Wird das `Secure`-Flag für über HTTPS übertragene Cookies erzwungen?
- [ ] Ja — auf **alle** über TLS übertragenen Cookies angewendet  
- [ ] Nein — das Flag wird **nicht angewendet**, wodurch Cookies über unverschlüsseltes HTTP gesendet werden können  

### Wird das `SameSite`-Attribut zur Minderung von CSRF-Risiken verwendet?
- [ ] Ja — für Session-Cookies auf `Strict` oder `Lax` gesetzt  
- [ ] Ja — auf `None` gesetzt, aber mit vorhandenem `Secure`-Flag  
- [ ] Nein — das Attribut **fehlt** oder ist auf `None` **ohne** `Secure` gesetzt  

### Sind die Attribute `Domain` und `Path` auf den minimal erforderlichen Bereich beschränkt?
- [ ] Ja — auf die **spezifische** Subdomain und den Anwendungspfad beschränkt  
- [ ] Nein — der Bereich ist zu **weit gefasst** (z. B. auf eine übergeordnete Domain oder das Root-Verzeichnis `/` gesetzt), was die Angriffsfläche vergrößert  

### Werden sensible Session-Cookies über die Attribute `Expires` oder `Max-Age` ordnungsgemäß für ungültig erklärt?
- [ ] Ja — es sind angemessene Ablaufdaten gesetzt  
- [ ] Nein — Cookies sind persistent mit übermäßig **langen** Lebensdauern  
- [ ] Nein — Cookies sind nur für die Sitzung gültig (Session-only), aber es **fehlt** die serverseitige Erzwingung eines Timeouts  

---