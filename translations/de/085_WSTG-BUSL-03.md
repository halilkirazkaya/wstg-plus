## WSTG-BUSL-03 — Integritätsprüfungen testen

Die Prüfung der Integritätskontrollen konzentriert sich auf die Verifizierung, dass die Anwendung sicherstellt, dass Daten unverändert und vertrauenswürdig bleiben, während sie verschiedene Geschäftsprozesse durchlaufen oder zwischen dem Client und dem Server übertragen werden. Angreifer zielen auf diese Logik ab, indem sie kritische Parameter – wie Produktpreise, Mengen, Benutzerberechtigungen oder Transaktionsstatus – innerhalb von HTTP-Requests, versteckten Feldern oder Cookies manipulieren. Falls der Anwendung eine serverseitige Verifizierung oder robuste Integritätskontrollen wie Hashing oder HMACs fehlen, kann ein Angreifer diese Werte manipulieren, um Betrug zu begehen, die eigenen Berechtigungen zu eskalieren oder beabsichtigte Geschäftsbeschränkungen zu umgehen. Dieser Test ist entscheidend für jede Anwendung, die Finanztransaktionen, sensible Statusübergänge oder mehrstufige Workflows verarbeitet, bei denen der Output einer Phase als vertrauenswürdiger Input für die nächste dient.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-BUSL-03 |
| **CWE** | CWE-353 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch* |

> *Der Schweregrad wird als "Hoch" eingestuft, wenn der Integritätsfehler Finanzdiebstahl, unbefugte Privilegieneskalation oder die Modifikation dauerhafter Datensätze ermöglicht.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/03-Test_Integrity_Checks  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Proxy/Repeater)`, `CyberChef`, `Postman`, `Python (Requests)`

### Sind sensible Geschäftsparameter clientseitigen Manipulationen ausgesetzt?
- [ ] Nein — sensible Werte werden ausschließlich serverseitig verwaltet  
- [ ] Ja — Werte wie Preis, Menge oder Status-Flags **sind** exponiert, aber verschlüsselt  
- [ ] Ja — Werte **sind** im Klartext in versteckten Feldern, URL-Parametern oder Cookies exponiert  

### Wird ein Integritätsmechanismus (z. B. HMAC, Checksum) auf clientgesteuerte Parameter angewendet?
- [ ] Ja — eine robuste Integritätsprüfung **wird angewendet** und bei jedem Request verifiziert  
- [ ] Ja — eine Integritätsprüfung **wird angewendet**, verwendet jedoch einen schwachen/vorhersehbaren Algorithmus oder ein hartcodiertes Secret  
- [ ] Nein — es **werden keine** Integritätsprüfungen auf sensible Parameter angewendet  

### Kann die Integritätsprüfung von einem Angreifer umgangen oder neu berechnet werden?
- [ ] Nein — die Signaturprüfung wird strikt erzwungen und ein Bypass ist **nicht möglich**  
- [ ] Ja — die Signaturprüfung kann durch Weglassen des Signaturparameters umgangen werden  
- [ ] Ja — die Signatur **kann** neu berechnet werden, da der Secret Key oder die Logik exponiert/schwach ist  

### Führt die Anwendung eine Backend-Revalidierung der Daten gegen eine vertrauenswürdige Quelle durch?
- [ ] Ja — der Server revalidiert alle Werte gegen die Datenbank und ein Bypass ist **nicht möglich**  
- [ ] Ja — der Server führt eine teilweise Validierung durch, aber ein Bypass **ist möglich** durch spezifische Edge Cases  
- [ ] Nein — der Server vertraut clientseitig bereitgestellten Daten ohne Backend-Verifizierung *(Kritisch)*  

### Ist es möglich, den Status eines mehrstufigen Prozesses zu manipulieren?
- [ ] Nein — der Session-Status wird serverseitig verwaltet und kann nicht manipuliert werden  
- [ ] Ja — Statusinformationen werden über den Client übergeben und Manipulationen **sind möglich**, um Schritte zu überspringen oder Aktionen zu wiederholen  

---