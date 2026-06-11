## WSTG-BUSL-10 — Testen der Zahlungsfunktionalität

Das Testen der Zahlungsfunktionalität umfasst die Identifizierung von Business Logic Flaws, die es einem Angreifer ermöglichen, Payment Gateways zu umgehen, Bestellsummen zu manipulieren oder erfolgreiche Transaktionssignale vorzutäuschen (Spoofing). Diese Schwachstellen befinden sich typischerweise in der Kommunikation zwischen der Anwendung, dem Client-Browser und Drittanbietern von Zahlungsdiensten wie Stripe, PayPal oder internen Ledger-Systemen. Ein Angreifer könnte versuchen, Preisparameter während der Übertragung zu modifizieren, negative Mengen zu übermitteln, um die Gesamtkosten zu senken, oder erfolgreiche Transaktions-Callbacks per Replay-Angriff erneut zu senden, um Bestellungen ohne tatsächliche Zahlung auszuführen. Eine erfolgreiche Ausnutzung führt zu direktem finanziellen Verlust, Inventardifferenzen und einer potenziellen Beeinträchtigung der E-Commerce-Integrität der Anwendung.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-BUSL-10 |
| **CWE** | CWE-840 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/10-Test-Payment-Functionality  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Werkzeuge:** `Burp Suite`, `Turbo Intruder`, `Postman`, `Hackvertor`

### Wird der Transaktionsbetrag vor der Verarbeitung serverseitig validiert?
- [ ] Ja — der Betrag wird strikt gegen die serverseitige Produktdatenbank validiert *(Am sichersten)*  
- [ ] Ja — der Betrag wird validiert, aber ein Logic Bypass ist über Parameter Pollution oder Währungswechsel möglich  
- [ ] Nein — der Transaktionsbetrag kann in der clientseitigen Anfrage modifiziert werden und wird vom Backend übernommen  

### Kann die Artikelmenge so manipuliert werden, dass eine Summe von Null oder ein negativer Gesamtbetrag entsteht?
- [ ] Nein — negative oder Null-Mengen werden durch die Validierungslogik der Anwendung abgelehnt  
- [ ] Ja — negative Mengen werden im Warenkorb akzeptiert, aber der Gesamtbetrag wird beim Checkout korrigiert  
- [ ] Ja — negative Mengen können verwendet werden, um den Gesamtrechnungsbetrag zu reduzieren oder eine Gutschrift zu erhalten *(Kritisch)*  

### Verarbeitet die Anwendung Payment-Gateway-Callbacks oder Webhooks sicher?
- [ ] Ja — Callbacks erfordern eine gültige kryptografische Signatur, die nicht vorgetäuscht werden kann  
- [ ] Ja — Signaturen werden geprüft, aber das Secret ist schwach oder die Verifizierung kann umgangen werden  
- [ ] Nein — die Anwendung verlässt sich auf nicht authentifizierte oder nicht verifizierte Callbacks, um den Zahlungsstatus zu bestätigen  

### Ist die Anwendung während der Checkout- oder Zahlungsphase anfällig für Race Conditions?
- [ ] Nein — Transaktionsintegrität und Datenbank-Locking-Mechanismen sind aktiviert  
- [ ] Ja — Race Conditions sind möglich, haben aber keinen Einfluss auf den Endpreis oder die Auftragsabwicklung  
- [ ] Ja — gleichzeitige Anfragen können zu mehreren Auftragsabwicklungen für eine einzige Zahlung oder zum „Double-Spending“ von Geschenkkarten führen  

### Sind Rabattcodes oder Treuepunkte anfällig für Manipulation oder Wiederverwendung?
- [ ] Nein — Codes werden für die einmalige Verwendung validiert und Logikbeschränkungen werden angewendet  
- [ ] Ja — Codes können wiederverwendet oder auf nicht berechtigte Artikel angewendet werden, aber die Auswirkungen sind begrenzt  
- [ ] Ja — Angreifer können mehrere widersprüchliche Codes anwenden oder Ablaufdaten und Nutzungslimits umgehen  

---