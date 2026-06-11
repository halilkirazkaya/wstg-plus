## WSTG-CLNT-09 — Prüfen auf Clickjacking

Clickjacking, auch bekannt als UI Redressing, tritt auf, wenn ein Angreifer transparente oder opake Ebenen verwendet, um einen Benutzer dazu zu verleiten, auf eine Schaltfläche oder einen Link auf einer anderen Seite zu klicken, während dieser beabsichtigte, auf die oberste Ebene der Seite zu klicken. Diese Schwachstelle beeinträchtigt die Integrität von Benutzeraktionen und kann zu unbefugten Datenänderungen, Kontoänderungen oder Finanztransfers führen, die im Namen des Opfers durchgeführt werden. Angreifer nutzen dies in der Regel aus, indem sie die Ziel-Website in einem `<iframe>` auf einer kontrollierten bösartigen Website einbetten und diese mit täuschenden Inhalten oder verlockenden Ködern überlagern. Dies tritt am häufigsten auf Seiten auf, die sensible zustandsändernde Aktionen durchführen, wie z. B. „Konto löschen“-Schaltflächen, Formulare zum Zurücksetzen von Passworten oder administrative Konfigurationspanels.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CLNT-09 |
| **CWE** | CWE-1021 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch* |

> *Der Schweregrad wird als „Hoch“ eingestuft, wenn sensible Aktionen (z. B. Geldtransfers, Kontolöschung oder Privilegieneskalation) via Clickjacking ausgelöst werden können.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/09-Testing_for_Clickjacking  
* https://hacktricks.wiki/en/pentesting-web/clickjacking.html  
* https://portswigger.net/web-security/clickjacking  

**Werkzeuge:** `Burp Suite Clickbandit`, `Browser Developer Tools`, `OWASP ZAP`, `Online Clickjack Test`

### Implementiert die Anwendung serverseitige Header, um unbefugtes Framing zu verhindern?
- [ ] Ja — `X-Frame-Options` oder `Content-Security-Policy` **werden angewendet** und verhindern Framing *(Am sichersten)*  
- [ ] Ja — Header sind vorhanden, aber **falsch konfiguriert** (z. B. `X-Frame-Options: ALLOW-FROM`, was an mangelnder breiter Browserunterstützung leidet)  
- [ ] Nein — es **werden keine** Header zum Schutz vor Framing angewendet  

### Ist die `Content-Security-Policy` (CSP) `frame-ancestors`-Direktive implementiert?
- [ ] Ja — `frame-ancestors 'none'` oder `'self'` **wird angewendet**  
- [ ] Ja — `frame-ancestors` ist vorhanden, **erlaubt** jedoch nicht vertrauenswürdige oder zu weit gefasste Ursprünge (Origins)  
- [ ] Nein — die Direktive **fehlt** oder CSP ist nicht implementiert  

### Ist der `X-Frame-Options` (XFO) Header für die Unterstützung älterer Browser korrekt konfiguriert?
- [ ] Ja — `DENY` oder `SAMEORIGIN` **wird angewendet**  
- [ ] Ja — XFO ist vorhanden, wird aber von modernen Browsern **ignoriert**, da eine CSP `frame-ancestors`-Direktive existiert  
- [ ] Nein — der XFO-Header **fehlt** oder ist auf einen unsicheren Wert gesetzt  

### Können die Framing-Schutzmaßnahmen mit bekannten Techniken umgangen werden?
- [ ] Nein — ein Bypass ist mit Standardtechniken (Double-Framing etc.) **nicht möglich**  
- [ ] Ja — der Schutz **kann** aufgrund schwacher Regex oder Logik in der `frame-ancestors`-Whitelist umgangen werden  
- [ ] Ja — der Schutz **kann** umgangen werden, da die Anwendung ausschließlich auf JavaScript-basiertem Frame-Busting basiert  

### Ist eine sensible Aktion über einen Clickjacking Proof-of-Concept ausnutzbar?
- [ ] Nein — Framing wird blockiert oder es sind keine sensiblen Aktionen erreichbar  
- [ ] Ja — UI Redressing **ist möglich**, erfordert jedoch komplexe Benutzerinteraktion  
- [ ] Ja — UI Redressing **ist möglich** und erlaubt sofortige zustandsändernde Aktionen *(Kritisch)*  

---