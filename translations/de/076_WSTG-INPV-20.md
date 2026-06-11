## WSTG-INPV-20 — Mass Assignment

Mass Assignment, auch bekannt als Overposting oder unsichere Deserialisierung von Parametern, tritt auf, wenn eine Webanwendung eingehende HTTP-Request-Parameter automatisch an interne Objekteigenschaften bindet, ohne eine ordnungsgemäße Filterung der Attribute vorzunehmen. Diese Sicherheitslücke ist in modernen MVC-Frameworks und RESTful APIs verbreitet, bei denen JSON-, XML- oder form-encoded Daten direkt in anwendungsseitige Datenmodelle deserialisiert werden. Angreifer nutzen dieses Verhalten aus, indem sie zusätzliche, nicht aufgeführte Parameter in eine Anfrage einschleusen – wie z. B. `isAdmin`, `role` oder `account_balance` –, um sensible Felder zu modifizieren, die von den Entwicklern nicht für die Benutzerkontrolle vorgesehen waren. Eine erfolgreiche Ausnutzung führt in der Regel zu Privilege Escalation, unbefugter Datenänderung oder der Umgehung kritischer Business-Logic-Workflows.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INPV-20 |
| **CWE** | CWE-915 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Mittel* |

> *Der Schweregrad wird als "Hoch" eingestuft, wenn administrative Attribute, Berechtigungsstufen oder Abrechnungsfelder geändert werden können.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/20-Testing_for_Mass_Assignment  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Arjun`, `Param Miner`, `Burp Suite (Repeater)`, `Postman`, `ffuf`

### Nutzt die Anwendung automatisches Binding für eingehende Request-Parameter?
- [ ] Nein — Parameter werden manuell auf spezifische Variablen oder sichere Objekte gemappt  
- [ ] Ja — automatisches Binding wird verwendet, aber strikte **Whitelists** oder DTOs (Data Transfer Objects) werden erzwungen  
- [ ] Ja — automatisches Binding wird verwendet und sensible interne Attribute **können** modifiziert werden  

### Können administrative oder berechtigungsrelevante Attribute erfolgreich injiziert werden?
- [ ] Nein — injizierte Parameter werden ignoriert, entfernt oder führen zu einem Validierungsfehler  
- [ ] Ja — zusätzliche Parameter werden akzeptiert, beeinflussen aber **nicht** den Zustand des Backend-Objekts  
- [ ] Ja — das Injizieren von Parametern wie `is_admin`, `role` oder `permissions` führt **erfolgreich** zu einer Privilege Escalation *(Kritisch)*  

### Ist es möglich, sensible Business-Logik oder Finanzfelder via Overposting zu modifizieren?
- [ ] Nein — Felder wie `price`, `balance` oder `status` sind vor Mass Assignment geschützt  
- [ ] Ja — sensible Geschäftsattribute **können** modifiziert werden, indem sie dem JSON- oder Form-Request-Body hinzugefügt werden  

### Offenbart die Anwendung interne Objektstrukturen, die das Erraten von Parametern erleichtern?
- [ ] Nein — Antworten enthalten nur vorgesehene öffentliche Felder und die Dokumentation ist eingeschränkt  
- [ ] Ja — interne Objektfelder sind in GET-Antworten sichtbar, was das Discovery von Parametern **möglich** macht  
- [ ] Ja — die API-Dokumentation (Swagger/OpenAPI) listet explizit interne Eigenschaften auf, die zugewiesen (**assigned**) werden können  

---