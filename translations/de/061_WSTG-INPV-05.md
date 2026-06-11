## WSTG-INPV-05 — SQL Injection

SQL Injection tritt auf, wenn vom Benutzer bereitgestellte Eingaben ohne ordnungsgemäße Parametrisierung oder Bereinigung in SQL-Abfragen einbezogen werden, was es Angreifern ermöglicht, die Abfragelogik zu manipulieren. Eine erfolgreiche Ausnutzung kann zu unbefugter Datenexfiltration, Authentifizierungsumgehung, Datenmanipulation und in einigen Fällen zu Remote Code Execution über Datenbankfunktionen wie `xp_cmdshell` oder `LOAD_FILE()` führen. Diese Schwachstelle betrifft häufig Login-Formulare, Suchfunktionen, REST API-Parameter, HTTP-Header und alle Endpunkte, die dynamische SQL-Abfragen aus benutzergesteuerten Eingaben erstellen. Aus der Sicht eines Angreifers ist die Identifizierung eines Injection-Punkts das primäre Tor zur vollständigen Kompromittierung der Datenbank und zu potenziellem Lateral Movement innerhalb der Infrastruktur.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INPV-05 |
| **CWE** | CWE-89 |
| **Test-Status** | Nicht durchgeführt |
| **Schweregrad** | Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/05-Testing_for_SQL_Injection  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html  
* https://portswigger.net/web-security/sql-injection  
* https://portswigger.net/web-security/nosql-injection  

**Tools:** `sqlmap`, `Burp Suite (Intruder/Repeater)`, `Ghauri`, `jSQL Injection`, `BBQSQL`

### Werden benutzergesteuerte Parameter mit sicheren Datenbankzugriffsmethoden verarbeitet?
- [ ] Ja — Die Anwendung verwendet ausschließlich ORM oder parametrisierte Abfragen, und ein Bypass ist **nicht möglich**  
- [ ] Ja — Parametrisierte Abfragen werden verwendet, aber ein Bypass **ist möglich** über Randfälle (z. B. `ORDER BY`-Klauseln)  
- [ ] Nein — Zur Erstellung von SQL-Abfragen wird eine einfache String-Verkettung **ohne** Parametrisierung verwendet  

### Kann der Authentifizierungsmechanismus über SQL Injection umgangen werden?
- [ ] Nein — Login-Parameter sind **nicht** anfällig für Injection  
- [ ] Ja — Eine Authentifizierungsumgehung **ist möglich** unter Verwendung von Tautologie-basierten Payloads (z. B. `' OR 1=1 --`)  

### Ist eine Datenexfiltration über In-Band- oder Out-of-Band-Techniken möglich?
- [ ] Nein — Es wurde keine Datenexfiltration identifiziert  
- [ ] Ja — UNION-basierte oder Error-basierte Exfiltration **ist möglich**  
- [ ] Ja — Blinde (Boolean- oder zeitbasierte) Exfiltration **ist möglich**  
- [ ] Ja — Out-of-Band (DNS/HTTP) Exfiltration **ist möglich**  

### Weisen Anwendungsfunktionen Second-Order SQL Injection Schwachstellen auf?
- [ ] Nein — Gespeicherte Daten werden sicher verarbeitet, wenn sie für spätere Abfragen abgerufen werden  
- [ ] Ja — Benutzereingaben werden gespeichert und später in einer unsicheren SQL-Abfrage **ohne** Validierung verwendet  

### Sind die Berechtigungen des Datenbank-Dienstkontos auf das erforderliche Minimum beschränkt?
- [ ] Ja — Das Dienstkonto verfügt über **Least Privilege** (z. B. beschränkt auf bestimmte Tabellen/Aktionen)  
- [ ] Nein — Das Dienstkonto verfügt über erweiterte Berechtigungen (z. B. `DROP TABLE`, `FILE`-Berechtigungen oder `xp_cmdshell` ist **aktiviert**)  

---