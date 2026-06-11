## WSTG-BUSL-06 — Testing for the Circumvention of Work Flows

Die Umgehung von Workflows (Workflow Circumvention) beinhaltet die Manipulation der Logik einer Anwendung, um beabsichtigte Sequenzen oder Voraussetzungen innerhalb mehrstufiger Prozesse zu umgehen. Angreifer identifizieren sensible Endpunkte – wie Zahlungsbestätigungen, administrative Genehmigungen oder MFA-Abschlussseiten – und versuchen, direkt auf diese zuzugreifen, wobei obligatorische Zwischenschritte übersprungen werden. Diese Schwachstelle tritt auf, wenn die serverseitige Logik keine robuste State Machine implementiert, sondern stattdessen darauf vertraut, dass ein Benutzer dem UI-gesteuerten Pfad folgt. Eine erfolgreiche Ausnutzung kann zu kritischen Fehlern in der Business Logic führen, einschließlich nicht autorisierter Transaktionen, Privilege Escalation und der Umgehung von Identitätsverifizierungsmechanismen.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-BUSL-06 |
| **CWE** | CWE-841 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch* |

> *Der Schweregrad wird als „Hoch“ eingestuft, wenn die Umgehung nicht autorisierte Finanztransaktionen, Privilege Escalation oder administrativen Zugriff ermöglicht.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/06-Testing_for_the_Circumvention_of_Work_Flows  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**Werkzeuge:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Postman`, `Caido`

### Enthält die Anwendung mehrstufige Geschäftsprozesse?
- [ ] Nein — die Anwendung besteht nur aus Aktionen mit Einzelanfragen  
- [ ] Ja — mehrstufige Prozesse (z. B. Warenkörbe, Wizard-Formulare, Registrierung) **sind** vorhanden  

### Wird die Abfolge der Schritte vom Server strikt erzwungen?
- [ ] Ja — serverseitiges State Tracking stellt sicher, dass Schritte **nicht** übersprungen werden können  
- [ ] Ja — clientseitige Kontrollen geben eine Sequenz vor, aber der Server validiert den Fortschritt **nicht**  
- [ ] Nein — die Anwendung erzwingt **keine** spezifische Reihenfolge der Operationen  

### Kann ein Angreifer die letzte Phase eines Prozesses erreichen, indem er die finale URL oder den Endpunkt direkt aufruft?
- [ ] Nein — der direkte Zugriff auf finale Schritte ist ohne vorherigen Abschluss **nicht möglich**  
- [ ] Ja — finale Schritte (z. B. `/api/v1/checkout/complete`) **können** durch direkte Anfrage erreicht werden  

### Überprüft die Anwendung, ob die erforderlichen Daten aus vorherigen Schritten vorhanden und valide sind?
- [ ] Ja — der Server validiert alle Daten der vorherigen Schritte, bevor er fortfährt  
- [ ] Nein — der Server **ist** anfällig für das „Überspringen“ von Schritten, bei denen Daten erwartet, aber nicht verifiziert werden  

### Können Sicherheitskontrollen wie MFA oder Identitätsverifizierung durch Manipulation des Workflows umgangen werden?
- [ ] Nein — kritische Kontrollen **können nicht** durch Workflow-Manipulation umgangen werden  
- [ ] Ja — eine Umgehung von MFA oder Identitätsprüfungen **ist möglich**, indem zum Schritt nach der Verifizierung gesprungen wird  

---