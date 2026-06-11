## WSTG-APIT-99 — Testen von GraphQL

GraphQL ist eine Abfragesprache für APIs, die es Clients ermöglicht, spezifische Datenstrukturen anzufordern. Fehlkonfigurationen führen jedoch häufig zu erheblichen Sicherheitsrisiken, einschließlich Information Disclosure und Resource Exhaustion. Schwachstellen entstehen typischerweise durch aktiviertes Introspection, das Fehlen von Limits für Query Depth/Complexity und Broken Object-Level Authorization (BOLA) innerhalb der Resolver-Funktionen. Angreifer nutzen diese Endpunkte aus, indem sie Introspection verwenden, um das gesamte Schema abzubilden, zirkuläre Fragmente einsetzen, um einen Denial of Service (DoS) auszulösen, oder die Autorisierung umgehen, indem sie über verschachtelte Abfragen auf unbefugte Felder zugreifen. Das Testen konzentriert sich auf die Endpunkte `/graphql`, `/v1/graphql` oder `/graphiql`, um sicherzustellen, dass die Implementierung strenge Zugriffskontrollen und Ressourcenmanagement erzwingt.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-APIT-99 |
| **CWE** | CWE-200 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Kritisch* |

> *Der Schweregrad wird als Kritisch eingestuft, wenn Broken Object-Level Authorization (BOLA) den unbefugten Zugriff auf PII oder administrative Mutationen ermöglicht.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/99-Testing_GraphQL  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/graphql  

**Tools:** `InQL`, `Graphw00f`, `GraphQL Voyager`, `Burp Suite`, `Altair GraphQL Client`, `Clairvoyance`

### Ist das GraphQL-Introspection-System aktiviert?
- [ ] Nein — Introspection ist **deaktiviert** und das Schema kann nicht abgebildet werden  
- [ ] Ja — Introspection ist **aktiviert**, erfordert jedoch eine administrative Authentifizierung  
- [ ] Ja — Introspection ist **aktiviert** und öffentlich zugänglich *(Hoch)*  

### Werden Ressourcenbeschränkungen (Tiefe und Komplexität) für Abfragen erzwungen?
- [ ] Ja — Strenge Limits für Query Depth und Complexity werden **angewendet** und erzwungen  
- [ ] Ja — Limits werden **angewendet**, können aber durch Aliase oder Fragmente umgangen werden  
- [ ] Nein — Es werden keine Limits **angewendet**, was rekursive Abfragen und DoS ermöglicht  

### Liegt Broken Object-Level Authorization (BOLA) in den Resolvern vor?
- [ ] Nein — Die Autorisierung wird für jede Abfrage auf Feld- und Objektebene **angewendet**  
- [ ] Ja — Die Autorisierung wird auf einige Felder **angewendet**, aber sensible Objekte sind exponiert  
- [ ] Ja — Die Autorisierung wird **nicht angewendet**, was den Zugriff auf beliebige Objekte über ID-Manipulation ermöglicht  

### Sind GraphQL-Mutationen ordnungsgemäß gegen unbefugte Nutzung geschützt?
- [ ] Nein — Mutationen sind im Schema **nicht** vorhanden  
- [ ] Ja — Mutationen erfordern eine gültige Authentifizierung und strenge Autorisierungsprüfungen  
- [ ] Ja — Mutationen sind für nicht authentifizierte Benutzer **möglich** oder es fehlt an Autorisierung  

### Geben Fehlermeldungen sensible Details zur Implementierung oder zum Schema preis?
- [ ] Nein — Fehlermeldungen sind generisch und geben **keine** interne Logik preis  
- [ ] Ja — Fehlermeldungen geben zugrunde liegende Datenbanktypen oder Vorschläge über „Did you mean...?“ preis  
- [ ] Ja — Vollständige Stack-Traces und sensible Konfigurationsdaten werden in den Antworten **offengelegt**  

---