## WSTG-ATHZ-03 — Prüfung auf Privilege Escalation

Eine Privilege Escalation tritt auf, wenn ein Angreifer Schwachstellen ausnutzt, um Zugriff auf Ressourcen oder Funktionen zu erhalten, die Benutzern mit höheren Berechtigungsstufen oder anderen Identitäten vorbehalten sind. Bei der vertikalen Eskalation versucht ein Standardbenutzer, auf administrative Funktionen zuzugreifen, während die horizontale Eskalation den Zugriff auf Daten eines anderen Benutzers mit derselben Berechtigungsstufe umfasst. Diese Mängel manifestieren sich typischerweise in schlecht implementierten Access Control Lists (ACLs), Insecure Direct Object References (IDOR) oder Parameter Tampering innerhalb von Session- oder Identity-Tokens. Aus der Perspektive eines Angreifers wird dies häufig durch die Manipulation numerischer IDs, das Ändern rollenbasierter Parameter in Cookies oder JWTs oder das direkte Aufrufen versteckter API-Endpunkte erreicht, bei denen serverseitige Autorisierungsprüfungen fehlen.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-ATHZ-03 |
| **CWE** | CWE-269 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/03-Testing_for_Privilege_Escalation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**Tools:** `Burp Suite`, `Authorize`, `AuthMatrix`, `Authz`, `Postman`, `Ffuf`

### Implementiert die Anwendung mehrere Berechtigungsstufen oder Multi-Tenancy?
- [ ] Nein — Die Anwendung ist ein Einbenutzer- oder Einzelrollensystem  
- [ ] Ja — Es sind mehrere Rollen oder Mandanten definiert, die Autorisierungstests erfordern  

### Ist eine horizontale Privilege Escalation (Zugriff auf derselben Ebene) möglich?
- [ ] Nein — Autorisierungsprüfungen werden auf alle Ressourcenbezeichner und Session-Besitzer angewendet  
- [ ] Ja — Der Zugriff auf Daten anderer Benutzer ist durch IDOR oder Parameter Tampering möglich  
- [ ] Ja — Ein Datenabfluss ist möglich, aber die Änderung von Daten anderer Benutzer ist nicht möglich  

### Ist eine vertikale Privilege Escalation (niedrig zu hoch) möglich?
- [ ] Nein — Administrative Endpunkte erzwingen strikt rollenbasierte Zugriffskontrollen (RBAC)  
- [ ] Ja — Administrative Funktionen können von Benutzern mit geringen Privilegien über den direkten URL-Zugriff aufgerufen werden  
- [ ] Ja — Administrative Funktionen können durch die Manipulation von rollenbezogenen Headern, Cookies oder JWT-Claims aufgerufen werden  

### Können Benutzerrollen oder Berechtigungen über Mass Assignment oder Parameter Pollution modifiziert werden?
- [ ] Nein — Rollenbasierte Felder sind strikt schreibgeschützt und können nicht von Benutzern geändert werden  
- [ ] Ja — Benutzerberechtigungen können durch das Einfügen versteckter Parameter (z. B. `role=admin`) bei Profilaktualisierungen oder der Registrierung erhöht werden  

### Werden Autorisierungsprüfungen konsistent über alle API-Versionen und HTTP-Methoden hinweg erzwungen?
- [ ] Ja — Kontrollen werden konsistent über alle Versionen und Methoden hinweg angewendet  
- [ ] Nein — Veraltete API-Versionen (z. B. `/v1/`) oder spezifische HTTP-Methoden (z. B. `PUT`, `DELETE`, `PATCH`) umgehen die Autorisierung  
- [ ] Nein — Administrative Funktionen sind in der Benutzeroberfläche ausgeblendet, aber auf API-Ebene für alle Benutzer aktiviert  

---