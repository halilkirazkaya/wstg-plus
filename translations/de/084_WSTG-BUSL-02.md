## WSTG-BUSL-02 — Prüfung der Fähigkeit zur Fälschung von Anfragen

Das Fälschen von Anfragen (Forging Requests) im Kontext der Business Logic beinhaltet die Manipulation von anwendungsdefinierten Parametern, um Aktionen außerhalb des beabsichtigten Workflows oder Berechtigungsniveaus durchzuführen. Angreifer zielen auf mehrstufige Prozesse ab, wie z. B. Checkout-Systeme oder die Registrierung von Konten, bei denen der Status (State) über clientseitige Parameter verwaltet wird, die der Server nicht erneut gegen eine vertrauenswürdige Backend-Quelle validiert. Durch das Ändern versteckter Felder, JSON-Werte oder REST-Parameter wie Preise, Mengen oder interne Status-Flags kann ein Angreifer Zahlungsanforderungen umgehen, Berechtigungen ausweiten (Privilege Escalation) oder unbeabsichtigte Statusänderungen auslösen. Diese Schwachstelle tritt typischerweise in zustandsbehafteten Webformularen oder APIs auf, bei denen der Server der clientseitigen Darstellung des Business-Status während einer Transaktion implizit vertraut.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-BUSL-02 |
| **CWE** | CWE-602 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Kritisch* |

> *Der Schweregrad wird als Kritisch eingestuft, wenn die gefälschte Anfrage finanzielle Verluste, unbefugte administrative Aktionen oder eine vollständige Kontoübernahme (Account Takeover) ermöglicht.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/02-Test_Ability_to_Forge_Requests  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Werkzeuge:** `Burp Suite (Proxy, Repeater, Intruder)`, `Postman`, `OWASP ZAP`, `CyberChef`

### Validiert die Anwendung Transaktionsparameter gegen eine serverseitige "Source of Truth"?
- [ ] Ja — alle sensiblen Parameter (Preis, Rolle, ID) werden gegen die Datenbank validiert und ein Bypass ist **nicht möglich**  
- [ ] Ja — eine Validierung erfolgt, kann aber durch Parameter Pollution oder alternative Kodierung umgangen werden  
- [ ] Nein — die Anwendung vertraut clientseitigen Werten, um die Kosten oder das Ergebnis einer Transaktion zu bestimmen *(Kritisch)*  

### Können geschäftskritische Werte (z. B. Mengen, Beträge) zu unbefugten Werten modifiziert werden?
- [ ] Nein — negative Werte, Nullwerte oder übermäßig hohe Werte werden vom Server abgelehnt  
- [ ] Ja — der Server akzeptiert modifizierte Werte, jedoch nur innerhalb eines begrenzten Bereichs  
- [ ] Ja — negative Werte oder Nullwerte **können** übermittelt werden, was zu finanziellen Verlusten oder Logik-Bypasses führt  

### Werden versteckte Felder oder API-Parameter verwendet, um den Status über mehrstufige Prozesse hinweg aufrechtzuerhalten?
- [ ] Nein — der Status wird sicher über serverseitige Sessions oder signierte Token verwaltet  
- [ ] Ja — der Status wird über den Client übermittelt, aber Integritätsprüfungen (HMAC/Signaturen) sind **aktiviert** und robust  
- [ ] Ja — der Status wird über den Client übermittelt und Integritätsprüfungen sind **deaktiviert** oder können entfernt werden  

### Ist es möglich, Anfragen zu fälschen, die obligatorische Zwischenschritte in einem Workflow überspringen?
- [ ] Nein — der Server erzwingt eine strikte Abfolge von Operationen für jede Transaktion  
- [ ] Ja — Schritte **können** übersprungen werden, aber die finale Aktion erfordert weiterhin valide Daten aus vorherigen Schritten  
- [ ] Ja — die finale "Submit"- oder "Complete"-Anfrage **kann** direkt gefälscht werden, um Autorisierungs- oder Zahlungsschritte zu umgehen  

---