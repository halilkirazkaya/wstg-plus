## WSTG-ERRH-02 — Prüfung auf Stack Traces

Stack Traces werden generiert, wenn eine Anwendung eine Exception nicht kontrolliert verarbeitet, was zur Offenlegung des internen Ausführungsstatus und der Aufrufhierarchie gegenüber dem Endbenutzer führt. Für einen Penetrationstester sind diese Traces von unschätzbarem Wert, da sie das Anwendungsframework, Bibliotheksversionen, interne Dateisystempfade und den Logikfluss preisgeben, was den Aufwand für die Reconnaissance erheblich reduziert. Die Ausnutzung beinhaltet das absichtliche Auslösen von Fehlern durch manipulierte Eingaben, unerwartete Datentypen oder Verletzungen von Grenzwerten, um die Anwendung in einen instabilen Zustand zu zwingen und Informationen über den zugrunde liegenden Technologiestack zu exfiltrieren. Dieser Informationsabfluss bietet oft den notwendigen Kontext, um verwundbare Drittanbieter-Komponenten zu identifizieren oder spezifische Exploits für Logikfehler zu entwickeln, die innerhalb der offengelegten Pfade im Code entdeckt wurden.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-ERRH-02 |
| **CWE** | CWE-209 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Niedrig / Mittel* |

> *Der Schweregrad erhöht sich auf Mittel, wenn der Stack Trace sensible architektonische Details, Datenbankabfragen oder interne Konfigurationsparameter preisgibt.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/02-Testing_for_Stack_Traces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `ffuf`, `Arjun`, `Wfuzz`, `Wappalyzer`

### Werden bei Anwendungsfehlern vollständige Stack Traces an den Client zurückgegeben?
- [ ] Nein — Anwendung gibt generische Fehlerseiten oder benutzerdefinierte Fehlermeldungen zurück  
- [ ] Ja — Stack Traces sind **aktiviert**, aber nur für spezifische, nicht-sensible Module  
- [ ] Ja — vollständige Stack Traces sind **aktiviert** und im HTTP-Response-Body sichtbar  

### Offenbaren Fehlermeldungen sensible Umgebungsinformationen?
- [ ] Nein — Fehlermeldungen enthalten keine technischen oder umgebungsbezogenen Details  
- [ ] Ja — Meldungen offenbaren **interne Dateipfade** oder serverseitige **Verzeichnisstrukturen**  
- [ ] Ja — Meldungen offenbaren **Datenbankschemata**, **SQL-Abfragen** oder **Versionen von Drittanbieter-Bibliotheken**  

### Können Stack Traces durch Manipulation von Eingabeparametern ausgelöst werden?
- [ ] Nein — manipulierte Eingaben werden kontrolliert verarbeitet oder durch die Validierung abgelehnt  
- [ ] Ja — Traces können durch **unerwartete Datentypen** ausgelöst werden (z. B. Übergabe eines Arrays, wo ein String erwartet wird)  
- [ ] Ja — Traces werden durch **Null-Bytes**, **Sonderzeichen** oder **Grenzwerte** ausgelöst  

### Wird ein globaler Error Handler konsistent in der gesamten Anwendung angewendet?
- [ ] Ja — ein globaler Exception Handler wird **konsistent** auf alle getesteten Endpunkte angewendet  
- [ ] Ja — ein Handler ist vorhanden, wird jedoch **nicht** auf Legacy-Systeme, APIs oder neu hinzugefügte Endpunkte angewendet  
- [ ] Nein — es wurde kein globaler Error-Handling-Mechanismus **festgestellt**, was zu Standard-Serverfehlern führt  

---