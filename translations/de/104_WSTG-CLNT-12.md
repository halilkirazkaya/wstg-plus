## WSTG-CLNT-12 — Testen von Browser-Speichern

Das Testen von Browser-Speichern umfasst die Analyse, wie eine Anwendung clientseitige Mechanismen wie LocalStorage, SessionStorage, IndexedDB und WebSQL nutzt, um Daten zu persistieren. Unsicher gespeicherte sensible Informationen – einschließlich PII, Authentifizierungs-Token oder Sitzungs-Identifier – können kompromittiert werden, wenn ein Angreifer Cross-Site Scripting (XSS) erreicht oder physischen Zugriff auf das Gerät des Benutzers erhält. Pentester müssen feststellen, ob Daten im Klartext gespeichert werden, ob sie nach dem Logout zugänglich bleiben und ob der Storage-Lebenszyklus angemessen verwaltet wird, um Datenlecks zu verhindern. Aus der Sicht eines Angreifers sind diese Speichermechanismen lukrative Ziele für das Exfiltrieren von zustandserhaltenden Informationen, die Session Hijacking oder langfristige Account-Persistenz ermöglichen.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CLNT-12 |
| **CWE** | CWE-922 |
| **Teststatus** | Nicht durchgeführt |
| **Kritikalität** | Mittel / Hoch* |

> *Die Kritikalität wird als „Hoch“ eingestuft, wenn Sitzungs-Token oder sensible PII in LocalStorage gespeichert werden und über XSS zugänglich sind.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/12-Testing_Browser_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Browser DevTools`, `Storage Explorer`, `Burp Suite`, `OWASP ZAP`

### Speichert die Anwendung sensible Informationen im Browser-Speicher?
- [ ] Nein — die Anwendung verwendet **keinen** Browser-Speicher oder speichert nur unkritische UI-Zustände  
- [ ] Ja — sensible Daten werden gespeichert, sind jedoch mit robuster clientseitiger Kryptographie **verschlüsselt**  
- [ ] Ja — sensible Daten (Tokens, PII, Geheimnisse) werden im **Klartext** gespeichert *(Mittel)*  

### Sind gespeicherte Daten für unbefugte Skripte zugänglich (XSS-Risiko)?
- [ ] Nein — Daten werden in HttpOnly-Cookies gespeichert (nicht im Browser-Speicher) oder der Speicher wird **nicht genutzt**  
- [ ] Ja — LocalStorage/SessionStorage wird verwendet, wodurch Daten über jeden XSS-Payload **vollständig zugänglich** sind *(Hoch)*  

### Wird der Browser-Speicher beim Logout des Benutzers ordnungsgemäß gelöscht?
- [ ] Ja — der gesamte anwendungsbezogene Speicher wird während des Logout-Prozesses **explizit gelöscht**  
- [ ] Nein — der Speicher bleibt nach dem Logout bestehen, enthält aber **keine** sensiblen Sitzungsdaten  
- [ ] Nein — Authentifizierungs-Token oder PII **verbleiben** nach Beendigung der Sitzung im Speicher *(Mittel)*  

### Verwendet die Anwendung IndexedDB oder WebSQL für große/sensible Datensätze?
- [ ] Nein — diese Speichermechanismen werden **nicht verwendet**  
- [ ] Ja — Daten werden gespeichert und **ordnungsgemäß bereinigt (sanitized)**, bevor sie im DOM gerendert werden  
- [ ] Ja — sensible Datensätze werden im **Klartext** in IndexedDB/WebSQL-Strukturen gespeichert  

### Kann die Integrität der gespeicherten Daten manipuliert werden, um die Anwendungslogik zu beeinflussen?
- [ ] Nein — die Anwendung validiert oder signiert Daten, bevor sie aus dem Speicher verarbeitet werden  
- [ ] Ja — das Ändern von Speicherwerten (z. B. Benutzerrollen, Flags) **ist möglich** und führt zu einem veränderten Anwendungsverhalten  

---