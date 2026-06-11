## WSTG-BUSL-01 — Test der Validierung von Geschäftslogik-Daten

Die Prüfung der Validierung von Geschäftslogik-Daten konzentriert sich auf die Fähigkeit der Anwendung, Daten zu identifizieren und abzulehnen, die zwar syntaktisch korrekt, aber im Kontext der Geschäftsdomäne logisch ungültig sind. Angreifer nutzen diese Schwachstellen aus, indem sie unerwartete Werte übermitteln – wie negative Mengen in einem Warenkorb, Daten in der Vergangenheit für zukünftig datierte Ereignisse oder extrem große Ganzzahlen (Integers) –, um Einschränkungen zu umgehen und den Status der Anwendung zu manipulieren. Diese Schwachstellen befinden sich häufig in mehrstufigen Workflows, Checkout-Prozessen und Kontoverwaltungsmodulen, bei denen die serverseitige Logik die kontextuelle Integrität der vom Benutzer bereitgestellten Daten nicht überprüft. Eine erfolgreiche Ausnutzung kann zu finanziellen Verlusten, Beeinträchtigungen der Integrität und unbefugtem Zugriff auf eingeschränkte Funktionen oder Daten führen.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-BUSL-01 |
| **CWE** | CWE-20 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Mittel |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/01-Test_Business_Logic_Data_Validation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**Werkzeuge:** `Burp Suite (Repeater/Intruder)`, `Postman`, `Python (Requests)`, `OWASP ZAP`

### Erzwingt die Anwendung logische Bereichsgrenzen für numerische Eingaben?
- [ ] Ja — Die Anwendung weist negative Werte, Null oder übermäßig große Werte dort korrekt ab, wo sie unangebracht sind *(Am sichersten)*  
- [ ] Ja — Die Anwendung weist einige ungültige Bereiche ab, ist aber anfällig für Integer Overflow oder das Umgehen von Grenzwerten (Boundary Bypass)  
- [ ] Nein — Die Anwendung akzeptiert jeden numerischen Wert ungeachtet des Geschäftskontexts *(Kritisch)*  

### Sind serverseitige Validierungsprüfungen über alle Schichten der Anwendung hinweg konsistent?
- [ ] Ja — Die Validierung wird konsistent sowohl auf der API-/Service- als auch auf der Datenbankebene angewendet  
- [ ] Ja — Die Validierung wird im UI/Frontend angewendet, aber eine Umgehung durch direkte API-Anfragen ist möglich  
- [ ] Nein — Die Validierung wird nicht angewendet oder ist inkonsistent, was eine logische Datenkorruption ermöglicht  

### Kann die Geschäftslogik durch Manipulation von Daten in mehrstufigen Workflows untergraben werden?
- [ ] Nein — Der Status wird sicher auf dem Server verwaltet, und Daten aus vorherigen Schritten können nicht manipuliert werden  
- [ ] Ja — Die Validierung erfolgt in frühen Schritten, aber die endgültige Übermittlung prüft die Integrität der Daten nicht erneut  
- [ ] Ja — Ein Workflow-Bypass ist möglich, indem Schritte übersprungen oder Daten in der falschen Reihenfolge übermittelt werden  

### Verarbeitet die Anwendung „Edge-Case“-Daten ordnungsgemäß, die Geschäftseinschränkungen umgehen?
- [ ] Ja — Die Einschränkungen sind robust und verarbeiten ungewöhnliche, aber syntaktisch gültige Eingaben (z. B. Schaltjahre, Zeitzonenverschiebungen)  
- [ ] Ja — Einige Sonderfälle werden behandelt, aber spezifische Kombinationen können zu unerwartetem Verhalten führen  
- [ ] Nein — Edge-Cases führen direkt zu Logikfehlern, wie z. B. kostenlosen Käufen oder unbefugten Statusänderungen  

---