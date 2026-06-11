## WSTG-INPV-06 — Testing for LDAP Injection

Eine LDAP-Injection tritt auf, wenn eine Anwendung vom Benutzer bereitgestellte Daten in LDAP-Filter (Lightweight Directory Access Protocol) einbindet, ohne diese ordnungsgemäß zu bereinigen oder zu maskieren (Escaping), wodurch ein Angreifer die Abfragelogik manipulieren kann. Durch das Injizieren von Sonderzeichen wie Sternchen, Klammern und logischen Operatoren können Angreifer den Suchfilter modifizieren, um Authentifizierungsmechanismen zu umgehen oder sensible Verzeichnisinformationen wie Benutzernamen, Gruppenmitgliedschaften und Organisationsattribute zu exfiltrieren. Diese Schwachstelle tritt typischerweise in Funktionen wie Unternehmensverzeichnissuchen, Mitarbeiterportalen oder Single Sign-On (SSO) Systemen auf, die mit Active Directory oder OpenLDAP interagieren. Aus der Sicht eines Angreifers beinhaltet eine erfolgreiche Ausnutzung oft Blind-Techniken, um die Verzeichnisstruktur oder Attributwerte Bit für Bit zu enumerieren, wenn die direkte Abfrageausgabe von der Anwendung unterdrückt wird.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INPV-06 |
| **CWE** | CWE-90 |
| **Test Status** | Not Performed |
| **Schweregrad** | High |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/06-Testing_for_LDAP_Injection  
* https://hacktricks.wiki/en/pentesting-web/ldap-injection.html  

**Tools:** `Burp Suite (Intruder/Repeater)`, `ldapsearch`, `JNDIExploit`, `Wfuzz`, `nmap`

### Nutzt die Anwendung LDAP für die Authentifizierung oder Verzeichnissuchen?
- [ ] Nein — LDAP wird in der Anwendungsarchitektur **nicht verwendet**  
- [ ] Ja — LDAP wird verwendet und alle Eingaben werden streng bereinigt oder parametrisiert  
- [ ] Ja — LDAP wird verwendet und Benutzereingaben werden **ohne** ordnungsgemäßes Escaping in Filter verkettet  

### Werden LDAP-Metazeichen in Benutzereingaben ordnungsgemäß maskiert oder gefiltert?
- [ ] Ja — Zeichen wie `(`, `)`, `&`, `|`, `*` und `\` sind **nicht erlaubt** oder werden korrekt maskiert  
- [ ] Nein — Metazeichen **können** injiziert werden, um die Struktur des LDAP-Filters zu verändern  

### Kann der Authentifizierungsmechanismus durch Injection umgangen werden?
- [ ] Nein — Die Login-Logik ist **nicht anfällig** für Abfagemanipulationen  
- [ ] Ja — Die Authentifizierung **kann** durch die Verwendung von logischem ODER (`|`) oder Wildcard-Injection (`*`) in den Benutzernamen- oder Passwortfeldern umgangen werden  

### Ist eine Blind-Datenexfiltration aus dem Verzeichnisdienst möglich?
- [ ] Nein — Die Suchergebnisse sind begrenzt und es werden keine Zeitunterschiede oder booleschen Differenzen beobachtet  
- [ ] Ja — Verzeichnisattribute **können** Bit für Bit über boolesche Blind-Injection-Techniken enumeriert werden  
- [ ] Ja — Vollständige Verzeichnisdatensätze **werden** aufgrund von Abfragemanipulation in der Antwort der Anwendung widergespiegelt  

### Sind sekundäre Anwendungskomponenten (z. B. Mailer, SSO) anfällig für LDAP-basierte Attribut-Injection?
- [ ] Nein — LDAP-Attribute werden in allen Komponenten sicher verarbeitet  
- [ ] Ja — Ein Angreifer **kann** seine eigenen Attribute (z. B. E-Mail, Gruppenmitgliedschaft) mittels Injection ändern, um Berechtigungen zu eskalieren oder die Kommunikation umzuleiten  

---