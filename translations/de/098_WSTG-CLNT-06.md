## WSTG-CLNT-06 — Testing for Client-Side Resource Manipulation

Clientseitige Ressourcenmanipulation (Client-Side Resource Manipulation) tritt auf, wenn eine Anwendung zulässt, dass benutzergesteuerte Eingaben die Quelle oder den Pfad von Ressourcen beeinflussen, die vom Browser geladen werden, wie z. B. Skripte, Stylesheets oder Iframes. Durch die Manipulation von URI-Fragmenten, Query-Parametern oder anderen DOM-Quellen kann ein Angreifer die Anwendung dazu zwingen, bösartige externe Inhalte zu laden oder nicht autorisierten Code auszuführen. Diese Schwachstelle findet sich typischerweise in der JavaScript-Logik, die `src`- oder `href`-Attribute von HTML-Elementen dynamisch füllt, ohne eine ausreichende Validierung gegen eine vertrauenswürdige Allow-List durchzuführen. Aus Sicht eines Angreifers wird dies ausgenutzt, indem eine URL erstellt wird, die das Laden von Ressourcen auf einen vom Angreifer kontrollierten Server umleitet, was potenziell zu DOM-basiertem Cross-Site Scripting (XSS), der Exfiltration sensibler Daten oder UI-Redressing führen kann.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CLNT-06 |
| **CWE** | CWE-99 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/06-Testing_for_Client-side_Resource_Manipulation  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  

**Tools:** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `grep`, `Snyk`, `LinkFinder`

### Nutzt die Anwendung clientseitige Sinks, um Ressourcen dynamisch zu laden oder einzubetten?
- [ ] Nein — Ressourcen werden ausschließlich über statisches HTML oder serverseitige Logik geladen  
- [ ] Ja — clientseitiges JavaScript füllt Ressourcen-Attribute wie `src`, `href` oder `data` dynamisch  

### Sind Validierungskontrollen oder Allow-Lists für Ressourcen-URI-Quellen implementiert?
- [ ] Ja — strenge Allow-Lists und Origin-Validierungen **werden auf alle** dynamischen Ressourcen angewendet  
- [ ] Ja — eine Validierung ist vorhanden, stützt sich jedoch auf schwache Blacklists oder leicht zu umgehende Regex  
- [ ] Nein — es werden keine Validierungskontrollen auf benutzergesteuerte Ressourcenpfade angewendet  

### Ist es möglich, die URI-Validierung durch alternative Protokolle oder Kodierungen zu umgehen?
- [ ] Nein — eine Umgehung der Ressourcenfilter ist **nicht möglich**  
- [ ] Ja — eine Umgehung **ist möglich** durch die Verwendung verschiedener Protokolle wie `data:`, `vbscript:` oder `javascript:`  
- [ ] Ja — eine Umgehung **ist möglich** durch die Verwendung von kodierten Zeichen oder Null-Bytes, um einfache String-Abgleiche zu umgehen  

### Kann ein Angreifer das Laden von Ressourcen auf eine externe, nicht vertrauenswürdige Domain umleiten?
- [ ] Nein — Ressourcenpfade sind auf denselben Ursprung (Same-Origin) oder ein festes Unterverzeichnis beschränkt  
- [ ] Ja — die Anwendung **kann** gezwungen werden, Skripte oder Styles von einer beliebigen externen Domain zu laden  

### Was ist die maximale Auswirkung der Ressourcenmanipulation?
- [ ] Niedrig — die Manipulation betrifft nur nicht ausführbare Elemente wie Bilder oder unkritische Texte  
- [ ] Mittel — die Manipulation ermöglicht CSS Injection oder erhebliches UI-Redressing  
- [ ] Hoch — die Manipulation führt zu DOM-basiertem XSS oder der Ausführung von beliebigem JavaScript *(Kritisch)*  

---