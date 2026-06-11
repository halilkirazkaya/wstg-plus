## WSTG-IDNT-03 — Prüfung des Account-Bereitstellungsprozesses

Der Account-Bereitstellungsprozess (Account Provisioning Process) regelt, wie neue Identitäten innerhalb einer Anwendungsumgebung erstellt, verifiziert und mit Berechtigungen ausgestattet werden. Angreifer zielen auf diesen Workflow ab, um automatisierte Massenregistrierungen für Spam oder Denial-of-Service (DoS) durchzuführen, Identitätsverifizierungsschritte zu umgehen, um betrügerische Konten zu erstellen, oder Parameter zu manipulieren, um sich während der Registrierungsphase erweiterte Berechtigungen zu verschaffen. Schwachstellen zeigen sich häufig in Self-Service-Registrierungsformularen, Systemen mit Einladungszwang (Invitation-only) oder administrativen Bereitstellungskonsolen, in denen Business Logic Flaws eine unbefugte Account-Erstellung oder eine Privilege Escalation ermöglichen. Aus der Sicht eines Angreifers bietet die Kompromittierung des Bereitstellungsflusses einen legitimen Zugangspunkt, der als Ausgangsbasis für tiefgreifendere Exploits, Datenexfiltration oder Social Engineering Angriffe genutzt werden kann.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-IDNT-03 |
| **CWE** | CWE-285 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch* |

> *Der Schweregrad wird als "Critical" eingestuft, wenn ein Angreifer administrative Konten bereitstellen oder globale Registrierungsbeschränkungen umgehen kann.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/03-Test_Account_Provisioning_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `FFUF`, `Postman`, `Python`, `MailHog`

### Wird die Selbstregistrierung streng kontrolliert oder auf autorisierte Benutzer beschränkt?
- [ ] Ja — die Registrierung ist **deaktiviert** oder erfordert eine manuelle administrative Genehmigung  
- [ ] Ja — die Registrierung ist offen, aber auf **autorisierte** E-Mail-Domains oder Invitation-Token beschränkt  
- [ ] Nein — die Registrierung ist für jeden Benutzer **offen**, ohne Domain- oder Token-Beschränkungen  

### Ist ein robuster Mechanismus zur Identitätsverifizierung (z. B. E-Mail/SMS) vor der Account-Aktivierung erforderlich?
- [ ] Ja — eine Verifizierung ist **erforderlich** und **kann nicht** umgangen werden  
- [ ] Ja — eine Verifizierung ist **erforderlich**, kann aber durch Parameter Tampering oder vorhersehbare Token **umgangen** werden  
- [ ] Nein — Konten sind nach der Übermittlung **sofort aktiv**, ohne jegliche Verifizierung  

### Kann der Benutzer während der Registrierungsanfrage Rollen- oder Berechtigungsparameter manipulieren?
- [ ] Nein — Rollen werden **serverseitig zugewiesen** und es existieren keine clientseitigen Parameter  
- [ ] Ja — Rollenparameter existieren, aber eine Manipulation ist aufgrund serverseitiger Validierung **nicht möglich**  
- [ ] Ja — Rollen-/Berechtigungsparameter (z. B. `role=admin`, `is_staff=true`) **können** modifiziert werden, um erweiterten Zugriff zu erhalten *(Critical)*  

### Sind Rate Limiting oder Anti-Automatisierungskontrollen implementiert, um Massenerstellungen von Konten zu verhindern?
- [ ] Ja — Rate Limiting und CAPTCHAs sind **aktiv** und effektiv  
- [ ] Ja — Kontrollen existieren, aber ein Bypass **ist möglich** über Header-Spoofing oder CAPTCHA-Lösungsdienste  
- [ ] Nein — es werden **keine** Mechanismen für Rate Limiting oder Anti-Automatisierung angewendet  

### Gibt der Bereitstellungsprozess Informationen über bestehende Konten preis (User Enumeration)?
- [ ] Nein — Antwortnachrichten sind für existierende und nicht existierende Benutzer **identisch**  
- [ ] Ja — unterschiedliche Antworten oder Zeitabweichungen (Timing Variations) **ermöglichen** die Enumeration existierender Benutzer  

---