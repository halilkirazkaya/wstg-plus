## WSTG-CONF-07 — Test HTTP Strict Transport Security

HTTP Strict Transport Security (HSTS) ist ein Sicherheitsmechanismus, der es einem Webserver ermöglicht, Browsern mitzuteilen, dass der Zugriff nur über HTTPS erfolgen soll. Dies verhindert Angriffe durch Protokoll-Downgrades und Cookie-Hijacking. Durch die Erzwingung einer verschlüsselten Verbindung mildert HSTS Man-in-the-Middle (MitM)-Angriffe wie SSLStrip ab, bei denen ein Angreifer eine anfängliche unsichere HTTP-Anfrage abfängt, um den Übergang zu TLS zu verhindern. Aus der Sicht eines Angreifers bietet das Fehlen dieses Headers oder eine kurze `max-age`-Dauer ein Zeitfenster, um sensible Session-Token oder Anmeldedaten während des initialen Klartext-Handshake abzufangen. Dieser Test konzentriert sich auf die Überprüfung des Vorhandenseins, der Dauer und des Geltungsbereichs des `Strict-Transport-Security`-Headers über alle sensiblen Endpunkte der Anwendung hinweg.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CONF-07 |
| **CWE** | CWE-319 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Niedrig / Mittel* |

> *Der Schweregrad wird als "Mittel" eingestuft, wenn die Anwendung PII (personenbezogene Daten), Session-Token oder Anmeldedaten verarbeitet, da das Fehlen von HSTS die Erfolgsquote von MitM-Angriffen erhöht.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/07-Test_HTTP_Strict_Transport_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Werkzeuge:** `curl`, `Burp Suite`, `nmap`, `SSLLabs`, `hsts-preload-checker`

### Ist der `Strict-Transport-Security`-Header in HTTPS-Antworten vorhanden?
- [ ] Ja — Header **ist auf allen sensiblen Endpunkten vorhanden**  
- [ ] Ja — Header **ist vorhanden**, aber nur auf der Startseite  
- [ ] Nein — Header **fehlt** in der gesamten Anwendung vollständig  

### Ist die `max-age`-Direktive auf eine ausreichende Dauer eingestellt?
- [ ] Ja — `max-age` ist auf ein Jahr oder länger eingestellt (z. B. `31536000`)  
- [ ] Ja — `max-age` ist eingestellt, aber **zu kurz** (z. B. weniger als 6 Monate)  
- [ ] Nein — `max-age`-Direktive **fehlt** oder ist auf `0` gesetzt *(Kritisch)*  

### Deckt die HSTS-Richtlinie alle Subdomains ab?
- [ ] Ja — `includeSubDomains`-Direktive **wird angewendet**  
- [ ] Nein — `includeSubDomains`-Direktive **fehlt**, wodurch Subdomains anfällig für Downgrades bleiben  
- [ ] Nein — Funktion trifft nicht zu, da keine Subdomains existieren  

### Ist die Anwendung für HSTS Preloading registriert?
- [ ] Ja — `preload`-Direktive **ist vorhanden** und die Domain befindet sich in der HSTS-Preload-Liste  
- [ ] Ja — `preload`-Direktive **ist vorhanden**, aber die Domain ist **noch nicht** in der Preload-Liste  
- [ ] Nein — `preload`-Direktive **fehlt**, was ein Zeitfenster für MitM beim ersten Besuch ermöglicht  

### Wird der HSTS-Header fälschlicherweise über Klartext-HTTP gesendet?
- [ ] Nein — Header wird **nur** über sichere HTTPS-Verbindungen gesendet  
- [ ] Ja — Header **wird über HTTP gesendet**, was von Browsern ignoriert wird und Konfigurationsdetails preisgeben kann  

---