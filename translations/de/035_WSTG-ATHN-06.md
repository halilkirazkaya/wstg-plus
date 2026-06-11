## WSTG-ATHN-06 — Prüfung auf Schwachstellen im Browser-Cache

Schwachstellen im Browser-Cache treten auf, wenn sensible Informationen im lokalen Browser-Cache gespeichert werden und von einem unbefugten Benutzer mit Zugriff auf denselben physischen Rechner abgerufen werden können. Das Fehlen ordnungsgemäßer Cache-Control-Header ermöglicht es, dass potenziell sensible Daten wie persönliche Informationen, Kontodaten oder Session-Identifier nach dem Abmelden des Benutzers oder dem Schließen der Sitzung bestehen bleiben. Angreifer oder nachfolgende Benutzer an gemeinsam genutzten oder öffentlichen Terminals können dies ausnutzen, indem sie den Browserverlauf durchsuchen, die „Zurück“-Schaltfläche verwenden oder lokale Cache-Dateien untersuchen, um gecachte Antworten zu exfiltrieren. Die Implementierung strenger Direktiven wie `Cache-Control: no-store` und `Pragma: no-cache` ist für jede Anwendung, die authentifizierte Inhalte verarbeitet, von entscheidender Bedeutung, um sicherzustellen, dass sensible Daten niemals auf die Festplatte geschrieben werden.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-ATHN-06 |
| **CWE** | CWE-525 |
| **Teststatus** | Nicht durchgeführt |
| **Kritikalität** | Niedrig / Mittel* |

> *Die Kritikalität wird als „Mittel“ eingestuft, wenn die Anwendung häufig von gemeinsam genutzten/öffentlichen Kiosksystemen aus aufgerufen wird oder hochsensible PII/PHI-Daten enthält.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/06-Testing_for_Browser_Cache_Weaknesses  

**Werkzeuge:** `Burp Suite (Proxy/Repeater)`, `Browser Developer Tools (Network Tab)`, `Zed Attack Proxy (ZAP)`

### Sind angemessene Cache-Control-Header auf authentifizierten oder sensiblen Seiten vorhanden?
- [ ] Ja — `Cache-Control: no-store, no-cache` und `Pragma: no-cache` sind **vorhanden** und **korrekt konfiguriert**  
- [ ] Ja — einige Header sind vorhanden, aber `no-store` **fehlt**, was das Caching auf der Festplatte ermöglicht  
- [ ] Nein — es werden keine Cache-bezogenen Header **angewendet**, wodurch das Standardverhalten des Browsers genutzt wird  

### Verwendet die Anwendung die `private`-Direktive für benutzerspezifische Daten?
- [ ] Ja — `Cache-Control: private` ist für alle personalisierten Inhalte **aktiviert**, um das Caching durch Proxies zu verhindern  
- [ ] Nein — Inhalte sind als `public` markiert oder es fehlt die `private`-Direktive, was sie für zwischengeschaltete Proxies **cachebar** macht  

### Sind sensible Informationen nach einem erfolgreichen Logout über die „Zurück“-Schaltfläche des Browsers zugänglich?
- [ ] Nein — der Browser fordert zur erneuten Authentifizierung auf oder die Seite wird **nicht** aus dem lokalen Cache geladen  
- [ ] Ja — sensible Informationen sind nach Beendigung der Sitzung weiterhin über die „Zurück“-Schaltfläche **sichtbar**  

### Sind sensible Nicht-HTML-Dateien (z. B. PDFs, CSV-Exporte, JSON-API-Antworten) vor Caching geschützt?
- [ ] Nein — es werden keine sensiblen Nicht-HTML-Dateien von der Anwendung generiert oder verarbeitet  
- [ ] Ja — strikte Cache-Control-Header werden auf alle sensiblen Dateidownloads und API-Endpunkte **angewendet**  
- [ ] Nein — sensible Downloads oder API-Antworten werden im lokalen Browser-Cache oder in temporären Verzeichnissen **gespeichert**  

### Verwendet die Anwendung `Expires: 0` oder ein Datum in der Vergangenheit, um die Verwendung veralteter Daten zu verhindern?
- [ ] Ja — der `Expires`-Header ist auf `0` oder ein Datum in der Vergangenheit gesetzt, um **sicherzustellen**, dass der Browser den Inhalt sofort als veraltet behandelt  
- [ ] Nein — der `Expires`-Header **fehlt** oder ist für sensible Seiten auf ein Datum in der Zukunft gesetzt  

---