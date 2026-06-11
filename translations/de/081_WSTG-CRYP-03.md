## WSTG-CRYP-03 — Prüfung auf Übertragung sensibler Informationen über unverschlüsselte Kanäle

Die Übertragung sensibler Daten über unverschlüsselte Kanäle, wie z. B. reines HTTP, setzt Informationen dem Abfangen und Modifizieren durch Man-in-the-Middle (MITM)-Angriffe aus. Diese Schwachstelle tritt typischerweise auf, wenn Webanwendungen es versäumen, Transport Layer Security (TLS) an allen Endpunkten zu erzwingen, insbesondere bei Altsystemen (Legacy-Komponenten), Login-Formularen oder während des initialen Übergangs von HTTP zu HTTPS. Aus der Sicht eines Angreifers ermöglicht dies das passive Sniffing von Authentifizierungsdaten (Credentials), Session-Token und personenbezogenen Daten (PII) in kompromittierten oder nicht vertrauenswürdigen Netzwerksegmenten wie öffentlichem WLAN. Die Sicherstellung, dass alle sensiblen Daten innerhalb eines robusten TLS-Tunnels gekapselt sind, ist grundlegend für die Wahrung der Vertraulichkeit und Integrität der Benutzerinteraktionen und zur Verhinderung von Session-Hijacking.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CRYP-03 |
| **CWE** | CWE-319 |
| **Teststatus** | Nicht durchgeführt |
| **Risikostufe** | Hoch / Mittel |

> *Die Risikostufe wird als "Hoch" eingestuft, wenn Authentifizierungsdaten, Session-Identifikatoren oder PII im Klartext übertragen werden.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/03-Testing_for_Sensitive_Information_Sent_via_Unencrypted_Channels  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Wireshark`, `tcpdump`, `Burp Suite (Proxy/Alerts)`, `nmap`, `sslyze`, `testssl.sh`

### Erlaubt die Anwendung die Kommunikation über unverschlüsseltes HTTP für sensible Endpunkte?
- [ ] Nein — der gesamte Anwendungsdatenverkehr wird strikt über HTTPS erzwungen *(Am sichersten)*  
- [ ] Ja — nicht-sensible Seiten sind über HTTP erreichbar, aber sensible Endpunkte sind es **nicht**  
- [ ] Ja — sensible Endpunkte (Login, Checkout, Profil) **sind** über unverschlüsseltes HTTP erreichbar *(Hohes Risiko)*  

### Gibt es eine globale Weiterleitung von HTTP zu HTTPS?
- [ ] Ja — die Anwendung führt für alle HTTP-Anfragen einen 301/302-Redirect auf HTTPS durch  
- [ ] Ja — die Weiterleitung wird nur für spezifische sensible Endpunkte durchgeführt  
- [ ] Nein — es wird **keine** Weiterleitung von HTTP zu HTTPS angewendet  

### Ist HTTP Strict Transport Security (HSTS) implementiert?
- [ ] Ja — HSTS ist **aktiviert** mit einem langen `max-age` und enthält die Direktiven `includeSubDomains` und `preload`  
- [ ] Ja — HSTS ist **aktiviert**, aber es fehlen die Direktiven `includeSubDomains` oder `preload`  
- [ ] Nein — HSTS ist auf dem Anwendungsserver **nicht aktiviert**  

### Sind sensible Cookies vor einer Übertragung im Klartext geschützt?

| Cookie-Name | Secure-Flag vorhanden | Secure-Flag fehlt |
|-------------|:-------------------:|:------------------:|
| `SessionID` |                     |                    |
| `AuthToken` |                     |                    |
| `CSRF-Token`|                     |                    |

### Überträgt die Anwendung sensible Daten über unverschlüsselte Kanäle an APIs von Drittanbietern?
- [ ] Nein — alle externen API-Aufrufe erfolgen über verschlüsselte (HTTPS) Tunnel  
- [ ] Ja — einige nicht-sensible Telemetriedaten werden über HTTP gesendet  
- [ ] Ja — API-Keys, PII oder Credentials **werden** über unverschlüsseltes HTTP an Drittanbieter gesendet  

---