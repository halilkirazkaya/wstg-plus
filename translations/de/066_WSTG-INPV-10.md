## WSTG-INPV-10 — IMAP SMTP Injection

IMAP- und SMTP-Injection-Schwachstellen entstehen, wenn eine Webanwendung benutzergelieferte Daten unzureichend filtert, bevor sie in Befehle integriert werden, die an einen Mailserver gesendet werden. Durch das Injizieren von Carriage-Return- und Line-Feed-Zeichen (CRLF) kann ein Angreifer den beabsichtigten Befehl beenden und beliebige Mail-Anweisungen anhängen, wie z. B. das Hinzufügen weiterer Empfänger (CC/BCC) oder das Modifizieren des Nachrichtentextes. Diese Schwachstellen treten am häufigsten in Web-to-Mail-Gateways, Kontaktformularen und E-Mail-Clients auf, die direkt über Protokolle wie IMAP4 oder SMTP mit Mailservern kommunizieren. Eine erfolgreiche Ausnutzung ermöglicht das unbefugte Weiterleiten (Relaying) von Spam, die Exfiltration sensibler E-Mail-Inhalte und in einigen Fällen die vollständige Kompromittierung der Integrität des Mailservers.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INPV-10 |
| **CWE** | CWE-93 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/10-Testing_for_IMAP_SMTP_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Werkzeuge:** `Burp Suite (Repeater/Intruder)`, `netcat`, `telnet`, `nmap`

### Verarbeitet die Anwendung vom Benutzer bereitgestellte Eingaben, um IMAP- oder SMTP-Befehle zu konstruieren?
- [ ] Nein — Die Anwendung interagiert **nicht** über Benutzereingaben mit Mailservern  
- [ ] Ja — Die Anwendung interagiert mit Mailservern, verwendet jedoch eine sichere API oder Bibliothek  
- [ ] Ja — Die Anwendung konstruiert Raw-Mail-Befehle unter Verwendung benutzergesteuerter Parameter  

### Werden Eingaben validiert, um die Injection von Carriage-Return- (`\r`) und Line-Feed-Sequenzen (`\n`) zu verhindern?
- [ ] Ja — CRLF-Zeichen werden **strikt** gefiltert, kodiert oder abgelehnt  
- [ ] Ja — Eingaben werden gegen eine strikte Whitelist (Allowlist) von Zeichen validiert  
- [ ] Nein — CRLF-Zeichen werden **nicht** gefiltert, was den Abbruch von Befehlen und Injection ermöglicht  

### Kann ein Angreifer zusätzliche Mail-Header wie `Bcc:` oder `Cc:` injizieren?
- [ ] Nein — Eine Header-Modifikation ist aufgrund strikter Eingabebeschränkungen **nicht möglich**  
- [ ] Ja — Die Injection zusätzlicher Header **ist** über CRLF-Sequenzen **möglich**  
- [ ] Ja — Mail-Relaying an externe Domains **ist aktiviert** und ausnutzbar *(Kritisch)*  

### Ermöglicht die Anwendung die Manipulation von IMAP-Befehlen, um auf nicht autorisierte Postfächer zuzugreifen?
- [ ] Nein — Die Anwendung verwendet **kein** IMAP oder beschränkt den Postfachzugriff strikt auf den authentifizierten Benutzer  
- [ ] Ja — IMAP-Command-Injection **ist möglich**, jedoch auf die Enumeration von Metadaten beschränkt  
- [ ] Ja — IMAP-Command-Injection ermöglicht das Lesen oder Löschen von E-Mails anderer Benutzer  

### Ist der Backend-Mailserver anfällig für Command-Pipelining?
- [ ] Nein — Der Mailserver lehnt gebündelte Befehle (Batched Commands) oder mehrere Befehle in einer einzigen Sitzung ab  
- [ ] Ja — Der Mailserver verarbeitet mehrere Befehle, die über ein einzelnes Eingabefeld injiziert wurden  

---