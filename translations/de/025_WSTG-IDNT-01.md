## WSTG-IDNT-01 — Prüfung der Rollendefinitionen

Die Prüfung der Rollendefinitionen umfasst die Identifizierung und Dokumentation der verschiedenen innerhalb der Anwendung definierten Benutzerrollen und Berechtigungsstufen, um sicherzustellen, dass diese logisch getrennt sind und dem Prinzip der geringsten Berechtigung (Principle of Least Privilege) entsprechen. Ein Angreifer analysiert die Anwendung, um Rollen mit hohen Privilegien gegenüber Standardbenutzerrollen abzubilden und nach Unklarheiten in den Berechtigungsgrenzen oder undokumentierten administrativen Funktionen zu suchen. Die Identifizierung dieser Rollen ist der erste Schritt zur Entdeckung von Wegen zur Privilegieneskalation (Privilege Escalation), da Inkonsistenzen in den Rollendefinitionen häufig zu unbefugtem Zugriff auf sensible Daten oder administrative Funktionen führen. Diese Phase findet in der Regel während der initialen Reconnaissance und des Mappings der Geschäftslogik (Business Logic Mapping) statt, wobei der Fokus auf Benutzerprofilen, administrativen Dashboards und API-Berechtigungsstrukturen liegt.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-IDNT-01 |
| **CWE** | CWE-285 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Niedrig / Mittel |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/01-Test_Role_Definitions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Compare Site Maps)`, `Postman`, `AuthMatrix`, `Authz`, `Browser Developer Tools`

### Sind Rollen in der Anwendung oder deren Dokumentation klar definiert und dokumentiert?
- [ ] Ja — Rollen sind vollständig dokumentiert und folgen dem Prinzip der geringsten Berechtigung (**Least Privilege**)  
- [ ] Ja — Rollen sind dokumentiert, enthalten jedoch **überlappende Berechtigungen**  
- [ ] Nein — es existieren keine formalen Rollendokumentationen oder -definitionen  

### Gibt es eine klare Trennung zwischen administrativen Funktionen und Standardbenutzerfunktionen?
- [ ] Ja — administrative Funktionen sind **vollständig isoliert** von Standardbenutzeransichten  
- [ ] Ja — eine Trennung ist vorhanden, aber administrative Endpunkte sind **vorhersehbar oder erratbar**  
- [ ] Nein — administrative und Standardfunktionen sind innerhalb derselben Schnittstellen **vermischt**  

### Verwendet die Anwendung einen zentralisierten Mechanismus für rollenbasierte Zugriffskontrolle (RBAC)?
- [ ] Ja — zentralisiertes **RBAC** oder **ABAC** ist **implementiert** und wird konsistent erzwungen  
- [ ] Ja — dezentrale Kontrollen existieren, werden jedoch über Module hinweg **inkonsistent angewendet**  
- [ ] Nein — die Zugriffskontrolllogik ist in einzelnen Seiten oder Komponenten **hartkodiert**  

### Sind undokumentierte oder versteckte Rollen im System vorhanden?
- [ ] Nein — alle entdeckten Rollen entsprechen der dokumentierten Funktionalität  
- [ ] Ja — versteckte oder "Schatten"-Rollen (z. B. `super_admin`, `support`, `test`) **sind vorhanden**  

### Kann ein Benutzer mit niedrigen Privilegien die Berechtigungen oder Rollen anderer Benutzer aufzählen (Enumerate)?
- [ ] Nein — Benutzer **können keine** Rollen/Berechtigungen anderer Entitäten einsehen oder aufzählen  
- [ ] Ja — Benutzer **können** Rollen über Profilseiten, Metadaten oder API-Antworten aufzählen *(Mittel)*  

---