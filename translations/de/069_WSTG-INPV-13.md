## WSTG-INPV-13 — Format String Injection

Format-String-Injection tritt auf, wenn eine Webanwendung benutzergesteuerte Eingaben direkt in das Format-String-Argument einer variadischen Funktion übergibt, wie z. B. die `printf`-Familie in C oder Low-Level-Logging-Wrapper. Obwohl diese Sicherheitslücke in modernen Managed-Code-Web-Frameworks seltener vorkommt, bleibt sie in Legacy-CGI-Anwendungen, nativen Erweiterungen oder Edge-Case-Logging-Systemen, die mit systemnahen Bibliotheken interagieren, kritisch. Angreifer nutzen dies aus, indem sie Formatspezifikatoren wie `%x` oder `%p` liefern, um sensible Speicheradressen und Daten aus dem Stack auszulesen (Leak), oder den `%n`-Spezifikator verwenden, um beliebige Schreibzugriffe auf den Speicher (Arbitrary Memory Writes) durchzuführen. Eine erfolgreiche Ausnutzung führt in der Regel zur vollständigen Systemübernahme durch Remote Code Execution (RCE) oder zumindest zu einem schwerwiegenden Denial-of-Service (DoS) durch Prozessabstürze.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INPV-13 |
| **CWE** | CWE-134 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/13-Testing_for_Format_String_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `GDB`, `strings`, `radare2`, `Checksec`, `AFL++`

### Verarbeitet die Anwendung Benutzereingaben über nativen Code oder Legacy-CGI-Schnittstellen?
- [ ] Nein — die Anwendung basiert vollständig auf Managed-Code-Frameworks (z. B. JVM, CLR)  
- [ ] Ja — die Anwendung verwendet JNI, native C/C++-Erweiterungen oder binäre Legacy-CGIs, bei denen Format-String-Schwachstellen existieren **können**  

### Werden vom Benutzer bereitgestellte Eingaben als primäres Argument an format-sensitive Funktionen übergeben?
- [ ] Nein — Eingaben werden als Datenargumente übergeben, und Format-Strings sind **statisch** und **hartcodiert**  
- [ ] Ja — die Benutzereingabe wird direkt in das Format-String-Argument verkettet, aber eine Bereinigung (Sanitization) **wird angewendet**  
- [ ] Ja — die Benutzereingabe wird direkt als Format-String verwendet und es **wird keine** Bereinigung angewendet  

### Ist es möglich, Speicherinhalte mithilfe von Formatspezifikatoren auszulesen?
- [ ] Nein — die Eingabe wird validiert und Spezifikatoren wie `%p`, `%x` oder `%s` werden **nicht verarbeitet**  
- [ ] Ja — die Angabe von `%p`- oder `%x`-Spezifikatoren führt zur Reflektion von Stack- oder Heap-Speicheradressen  
- [ ] Ja — sensible Daten (z. B. Pointer, Stack Cookies) **können** über Formatspezifikatoren exfiltriert werden  

### Kann die Anwendung mithilfe des `%n`-Spezifikators zum Absturz gebracht oder manipuliert werden?
- [ ] Nein — Spezifikatoren, die Schreibzugriffe auf den Speicher erlauben, sind **deaktiviert** oder werden vom Compiler/der Umgebung blockiert  
- [ ] Ja — die Anwendung stürzt ab (DoS), wenn `%s%s%s%s` oder ähnliche Zeichenfolgen bereitgestellt werden  
- [ ] Ja — beliebige Speicherzugriffe über `%n` sind **möglich**, was potenziell zu Remote Code Execution (RCE) führt  

### Sind moderne Binärschutzmechanismen (ASLR, DEP, Stack Canaries) durch diese Injection umgehbar?
- [ ] Nein — die Umgebung ist gehärtet und Speicherleaks **können nicht** zum Umgehen von Schutzmechanismen verwendet werden  
- [ ] Ja — Format-String-Injection **kann** verwendet werden, um Basisadressen auszulesen, wodurch ein ASLR/DEP-Bypass **möglich** wird  

---