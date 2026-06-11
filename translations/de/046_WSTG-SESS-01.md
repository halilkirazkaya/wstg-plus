## WSTG-SESS-01 — Prüfung des Session-Management-Schemas

Die Prüfung des Session-Management-Schemas konzentriert sich auf die Analyse, wie die Anwendung den Lebenszyklus und die Struktur von Session-Identifiern handhabt, um sicherzustellen, dass diese robust gegen Vorhersagen und Hijacking sind. Angreifer erfassen mehrere Session-Tokens, um statistische Analysen durchzuführen und nach Mustern, niedriger Entropie oder vorhersagbaren Inkrementen zu suchen, die es ermöglichen, valide Sessions für andere Benutzer zu fälschen. Schwachstellen im Schema äußern sich häufig als Session Fixation-Schwachstellen oder durch die Verwendung unzureichend zufälliger Werte, die typischerweise in HTTP-Cookies, URL-Parametern oder benutzerdefinierten Header-Implementierungen zu finden sind. Die Kompromittierung des Session-Schemas ist ein Angriff mit hoher Auswirkung, der einem Angreifer vollständigen Zugriff auf den authentifizierten Zustand eines Opfers gewährt, ohne dass dessen primäre Anmeldedaten benötigt werden.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-SESS-01 |
| **CWE** | CWE-330 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/01-Testing_for_Session_Management_Schema  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Tools:** `Burp Suite (Sequencer)`, `OWASP ZAP`, `Python (pandas/numpy for entropy analysis)`, `Cookie Editor`

### Ist der Session-Identifier ausreichend komplex und zufällig?
- [ ] Ja — Token besteht statistische Zufallstests (hohe Entropie) und ein Bypass **ist nicht möglich**  
- [ ] Ja — Token ist lang, enthält aber vorhersagbare Muster oder statische Segmente  
- [ ] Nein — Token ist sequentiell, basiert auf vorhersagbaren Zeitstempeln oder weist eine **kritisch niedrige** Entropie auf *(Kritisch)*  

### Wo wird der Session-Identifier übertragen und gespeichert?
- [ ] Identifier wird in einem `Secure` und `HttpOnly` Cookie gespeichert *(Am sichersten)*  
- [ ] Identifier wird in einem Cookie gespeichert, dem jedoch die Flags `Secure` oder `HttpOnly` fehlen  
- [ ] Identifier wird **nur** über URL-Parameter oder `GET`-Anfragen übertragen *(Hohes Risiko)*  

### Gibt die Anwendung nach erfolgreicher Authentifizierung einen neuen Session-Identifier aus?
- [ ] Ja — ein neuer, eindeutiger Session-ID **wird unmittelbar** nach dem Login generiert  
- [ ] Nein — die Session-ID bleibt vor und nach dem Login identisch *(Session Fixation möglich)*  

### Ist der Session-Identifier an eine spezifische IP-Adresse oder einen User-Agent gebunden, um eine Wiederverwendung zu verhindern?
- [ ] Ja — die Session wird ungültig, wenn sich der Fingerprint des Clients signifikant ändert  
- [ ] Nein — die Session **kann** über verschiedene IP-Adressen oder Geräte hinweg ohne zusätzliche Verifizierung wiederverwendet werden  

### Werden Session-Identifier nach dem Logout oder einem Timeout serverseitig ordnungsgemäß ungültig gemacht?
- [ ] Ja — der serverseitige Session-Status wird beim Logout **vollständig zerstört**  
- [ ] Nein — die Session bleibt auf dem Server gültig, selbst wenn das clientseitige Cookie gelöscht wurde  
- [ ] Nein — die Session **läuft nie ab** oder hat einen übermäßig langen Timeout-Zeitraum  

---