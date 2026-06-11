## WSTG-ATHN-07 — Prüfung auf schwache Passwortrichtlinien

Die Prüfung auf schwache Passwortrichtlinien umfasst die Bewertung der Anforderungen an Komplexität, Länge und Entropie, die von einer Anwendung bei der Kontoerstellung und der Aktualisierung von Anmeldedaten erzwungen werden. Unzureichende Richtlinien beeinträchtigen direkt die Vertraulichkeit von Benutzerkonten, indem sie diese anfällig für automatisierte Dictionary Attacks, Brute-Force-Versuche und Credential Stuffing machen. Diese Schwachstellen finden sich in der Regel in Registrierungsendpunkten, Workflows zum Zurücksetzen von Passwörtern und administrativen Benutzerverwaltungspanels. Aus der Sicht eines Angreifers reduziert eine freizügige Richtlinie die Rechenkosten und den Zeitaufwand erheblich, die erforderlich sind, um Anmeldedaten mithilfe gängiger Wortlisten oder musterbasierter Cracking-Verfahren erfolgreich zu kompromittieren.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-ATHN-07 |
| **CWE** | CWE-521 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Niedrig* |

> *Der Schweregrad erhöht sich auf „Hoch“, wenn der Anwendung Mechanismen für Account Lockout oder Rate Limiting fehlen, um automatisiertes Erraten zu verhindern.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/07-Testing_for_Weak_Authentication_Methods  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**Tools:** `Burp Suite (Intruder)`, `ZAP (Fuzzer)`, `Hydra`, `Hashcat`, `Cewl`

### Wird eine Mindestlänge für Passwörter von der Anwendung erzwungen?
- [ ] Ja — Mindestlänge beträgt 12+ Zeichen *(Am sichersten)*  
- [ ] Ja — Mindestlänge liegt zwischen 8 und 11 Zeichen  
- [ ] Ja — Mindestlänge ist **geringer als** 8 Zeichen  
- [ ] Nein — es wird keine Mindestlänge erzwungen  

### Werden Komplexitätsanforderungen (Großbuchstaben, Kleinbuchstaben, Ziffern, Symbole) erzwungen?
- [ ] Ja — mehrere Zeichenklassen sind obligatorisch und ein Bypass ist **nicht möglich**  
- [ ] Ja — Zeichenklassen werden empfohlen, aber **nicht** erzwungen  
- [ ] Nein — es werden keine Komplexitätsanforderungen angewendet  

### Blockiert die Anwendung gängige/schwache Passwörter oder Passwörter, die benutzerspezifische Daten enthalten?
- [ ] Ja — bekannte schwache Passwörter (z. B. „password123“) und auf dem Benutzernamen basierende Passwörter **können nicht** verwendet werden  
- [ ] Ja — gängige Passwörter werden blockiert, aber benutzerspezifische Daten (z. B. Benutzername, E-Mail) **können** verwendet werden  
- [ ] Nein — jede Zeichenfolge, welche die Anforderungen an Länge/Komplexität erfüllt, wird akzeptiert  

### Können Komplexitäts- oder Längenanforderungen durch clientseitige Modifikation umgangen werden?
- [ ] Nein — Richtlinien werden strikt auf der **Serverseite (Server-side)** erzwungen  
- [ ] Ja — Richtlinien werden über JavaScript erzwungen und **können** durch Abfangen der Anfrage umgangen werden  

### Wird die Passwortrichtlinie dem Benutzer klar kommuniziert, ohne Implementierungsdetails preiszugeben?
- [ ] Ja — Richtlinie ist während der Eingabe sichtbar und liefert generische Fehlermeldungen  
- [ ] Ja — Richtlinie ist sichtbar, aber Fehlermeldungen lassen Rückschlüsse auf spezifische Regex oder Logiklücken zu  
- [ ] Nein — Richtlinie ist **nicht** sichtbar, was eine Entdeckung durch Trial-and-Error erzwingt  

---