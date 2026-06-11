## WSTG-CLNT-04 — Prüfung auf Client-seitige URL-Weiterleitung

Client-seitige URL-Weiterleitung (Client-Side URL Redirect) tritt auf, wenn eine Webanwendung benutzergesteuerte Eingaben akzeptiert – in der Regel über URL-Parameter oder Hash-Fragmente – und diese innerhalb eines client-seitigen Redirection Sinks wie `window.location` oder `document.location.href` verwendet. Diese Schwachstelle wird primär von Angreifern ausgenutzt, um hochentwickelte Phishing-Kampagnen durchzuführen, da das Opfer zunächst der legitimen Domain vertraut, bevor es unbemerkt auf eine bösartige Seite weitergeleitet wird. Über Phishing hinaus können client-seitige Weiterleitungen oft verkettet werden, um sensible Daten wie OAuth Access Tokens oder Session-Identifikatoren zu exfiltrieren, falls die Weiterleitung erfolgt, während diese Werte in der URL oder im `Referer`-Header vorhanden sind. In modernen Single Page Applications (SPAs) tritt diese Schwachstelle häufig in der Routing-Logik, in „return to“-Parametern nach der Authentifizierung oder in Funktionen zum Sprachwechsel auf.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CLNT-04 |
| **CWE** | CWE-601 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel* |

> *Der Schweregrad erhöht sich auf „Hoch“, wenn die Weiterleitung zur Exfiltration von OAuth-Token, Session-IDs oder zur Umgehung von Sicherheitskontrollen in einem Authentifizierungsfluss genutzt werden kann.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/04-Testing_for_Client-side_URL_Redirect  
* https://hacktricks.wiki/en/pentesting-web/open-redirect.html  

**Tools:** `Burp Suite (DOM Invader)`, `Browser DevTools`, `LinkFinder`, `Arjun`, `B-Config`

### Verwendet die Anwendung client-seitige Skripte, um Weiterleitungen basierend auf Benutzereingaben zu verarbeiten?
- [ ] Nein — die Weiterleitung erfolgt ausschließlich serverseitig über `3xx` HTTP-Statuscodes  
- [ ] Ja — die Anwendung verwendet JavaScript-Sinks wie `window.location`, um basierend auf URL-Parametern zu navigieren  
- [ ] Ja — die Anwendung verwendet einen Router eines client-seitigen Frameworks (z. B. `React Router`, `Vue Router`), der vom Benutzer bereitgestellte Pfade verarbeitet  

### Sind Eingabevalidierungen oder Whitelisting-Kontrollen für die Ziel-URL implementiert?
- [ ] Ja — eine strikte **Whitelist** erlaubter Domains/Pfade wird erzwungen und ein Bypass ist **nicht möglich**  
- [ ] Ja — eine Validierung ist vorhanden, basiert jedoch auf schwachen regulären Ausdrücken (Regex) oder Blacklists, und ein Bypass ist **möglich**  
- [ ] Nein — die Anwendung akzeptiert beliebige Zeichenfolgen als Weiterleitungsziel  

### Kann die Weiterleitungslogik mit gängigen Obfuskationstechniken umgangen werden?
- [ ] Nein — die Kontrollen verarbeiten kodierte Zeichen, protokollrelative URLs (`//`) und Backslashes korrekt  
- [ ] Ja — ein Bypass ist mittels URL-Encoding oder Double-Encoding **möglich**  
- [ ] Ja — ein Bypass ist mittels protokollrelativer URLs oder des `@`-Zeichens (z. B. `https://expected.com@attacker.com`) **möglich**  
- [ ] Ja — ein Bypass ist durch spezifische Browser-Eigenheiten (Quirks) oder Null-Byte-Injection **möglich**  

### Werden durch den Weiterleitungsprozess sensible Daten an die Zielseite exponiert?
- [ ] Nein — es befinden sich keine sensiblen Daten in der URL und es werden keine sensiblen Daten während der Weiterleitung über Header übertragen  
- [ ] Ja — sensible Daten (z. B. Session-Token, API-Keys) werden über den `Referer`-Header an die externe Seite **geleakt**  
- [ ] Ja — sensible Daten im URL-Fragment (`#`) sind für die Skripte der Zielseite **zugänglich**  

### Ist es möglich, JavaScript über den Redirection Sink auszuführen (DOM-based XSS)?
- [ ] Nein — der Sink erlaubt nur die Navigation zu `http`- oder `https`-Protokollen  
- [ ] Ja — der Redirection Sink erlaubt `javascript:`- oder `data:`-URIs, wodurch DOM-based XSS **möglich** ist  

---