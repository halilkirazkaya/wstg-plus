## WSTG-BUSL-07 — Abwehrmechanismen gegen Anwendungsmissbrauch testen

Abwehrmechanismen gegen Anwendungsmissbrauch bewerten die Fähigkeit der Anwendung, anomales Benutzerverhalten zu identifizieren, zu protokollieren und darauf zu reagieren, das von der beabsichtigten Business Logic abweicht. Im Gegensatz zur traditionellen signaturbasierten Sicherheit konzentrieren sich diese Abwehrmechanismen auf die Erkennung von Mustern wie schnell aufeinanderfolgenden Anfragen, Credential Stuffing oder sequenzieller Ressourcen-Enumeration, die auf automatisierten Missbrauch hindeuten. Aus der Sicht eines Angreifers bestimmt dieser Test die Widerstandsfähigkeit der Anwendung gegen Automatisierung und „Low and Slow“-Angriffe, die darauf abzielen, Daten zu exfiltrieren oder Ressourcen zu erschöpfen, ohne Standard-Sicherheitswarnungen auszulösen. Das Versäumnis, diese Abwehrmechanismen zu implementieren, führt häufig zu massivem Data Scraping, Account Takeovers oder Denial of Service (DoS) durch legitime, aber ressourcenintensive Anwendungsfunktionen.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-BUSL-07 |
| **CWE** | CWE-799 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/07-Test_Defenses_Against_Application_Misuse  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Intruder/Turbo Intruder)`, `OWASP ZAP`, `Python (Requests/Selenium)`, `Caido`, `K6`

### Sind Mechanismen für Rate Limiting oder Throttling an sensiblen Endpunkten implementiert?
- [ ] Ja — striktes Rate Limiting **wird angewendet** und an allen sensiblen Endpunkten durchgesetzt *(Am sichersten)*  
- [ ] Ja — Rate Limiting **wird angewendet**, kann aber durch IP-Rotation oder Header-Manipulation umgangen werden  
- [ ] Ja — Rate Limiting **wird nur** auf Authentifizierungs-Endpunkte angewendet, wodurch andere exponiert bleiben  
- [ ] Nein — an keinerlei Anwendungsfunktionen **wird** Rate Limiting oder Throttling **angewendet**  

### Erkennt und blockiert die Anwendung automatisierte Interaktionsmuster?
- [ ] Ja — Verhaltensanalyse oder CAPTCHAs **sind aktiviert** und blockieren Bots effektiv  
- [ ] Ja — Anti-Automation **ist aktiviert**, aber eine Umgehung **ist möglich** durch Headless Browser oder Session Cycling  
- [ ] Nein — die Anwendung **kann nicht** zwischen einem menschlichen Benutzer und einem automatisierten Skript unterscheiden  

### Wird anomales Verhalten protokolliert und folgt darauf eine defensive Reaktion?
- [ ] Ja — Missbrauch löst eine automatisierte Blockierung aus und benachrichtigt das Sicherheitsteam  
- [ ] Ja — Missbrauch wird für Audits protokolliert, aber es **wird keine** automatisierte defensive Maßnahme ergriffen  
- [ ] Nein — die Anwendung protokolliert ungewöhnliche Aktivitätsmuster **nicht** und reagiert auch nicht darauf  

### Können Abwehrmechanismen durch Manipulation von clientseitigen Identifikatoren oder Headern umgangen werden?
- [ ] Nein — Abwehrmechanismen verlassen sich auf den serverseitigen Status und verifizierte Telemetrie  
- [ ] Ja — Kontrollmechanismen **können** durch das Löschen von Cookies oder das Rotieren von `User-Agent`-Strings umgangen werden  
- [ ] Ja — Kontrollmechanismen **können** durch das Injizieren von Headern wie `X-Forwarded-For` oder `X-Real-IP` umgangen werden  

---