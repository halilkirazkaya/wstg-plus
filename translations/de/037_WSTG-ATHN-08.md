## WSTG-ATHN-08 — Prüfung auf schwache Antworten auf Sicherheitsfragen

Die Prüfung auf schwache Antworten auf Sicherheitsfragen umfasst die Bewertung von Mechanismen der wissensbasierten Authentifizierung (Knowledge-Based Authentication, KBA), die zur Verifizierung der Identität eines Benutzers verwendet werden, üblicherweise während der Passwortwiederherstellung oder in Multi-Faktor-Authentifizierungs-Flows (MFA). Dies ist von Bedeutung, da Sicherheitsfragen oft auf statischen, nicht geheimen Informationen beruhen, die leicht durch Open-Source Intelligence (OSINT) oder Social Engineering ermittelt werden können, was zu einer unbefugten Kontoübernahme (Account Takeover) führen kann. Diese Schwachstellen treten typischerweise in den Modulen „Passwort vergessen“ oder „Konto entsperren“ auf, in denen Benutzer Antworten auf vordefinierte Fragen geben. Aus der Sicht eines Angreifers ist dies ein primäres Ziel für automatisierte Brute-Force-Angriffe oder gezielte Recherche, da viele Benutzer vorhersehbare oder öffentlich überprüfbare Antworten auf gängige Fragen geben, wie z. B. „Wie lautet der Geburtsname Ihrer Mutter?“ oder „Welche Highschool haben Sie besucht?“.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-ATHN-08 |
| **CWE** | CWE-640 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch* |

> *Der Schweregrad wird als „Hoch“ eingestuft, wenn die Sicherheitsfrage die einzige Anforderung für einen vollständigen Passwort-Reset und eine Kontoübernahme ohne weitere Verifizierung darstellt.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/08-Testing_for_Weak_Security_Question_Answer  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**Tools:** `Burp Suite (Intruder)`, `ffuf`, `Maltego`, `Sherlock`, `Social Engineering Toolkit (SET)`

### Implementiert die Anwendung Sicherheitsfragen zur Identitätsverifizierung oder Passwortwiederherstellung?
- [ ] Nein — Sicherheitsfragen werden **nicht** zur Authentifizierung oder Wiederherstellung verwendet  
- [ ] Ja — Sicherheitsfragen werden als optionaler sekundärer Faktor **verwendet**  
- [ ] Ja — Sicherheitsfragen werden als primärer/einziger Wiederherstellungsmechanismus **verwendet** *(Hohes Risiko)*  

### Werden Sicherheitsfragen aus einer festen Liste ausgewählt oder sind sie benutzerdefiniert?
- [ ] Nein — die Fragen sind vollständig benutzerdefiniert und erfordern eine hohe Entropie  
- [ ] Ja — die Fragen sind festgelegt, enthalten aber Optionen, die nicht über OSINT ermittelbar sind  
- [ ] Ja — die Fragen stammen aus einer festen Liste gängiger, über OSINT ermittelbarer Themen *(Anfällig)*  

### Wird Rate Limiting oder eine Kontosperrung auf Versuche zur Beantwortung von Sicherheitsfragen angewendet?
- [ ] Ja — striktes Rate Limiting und Kontosperrung (Account Lockout) **werden angewendet**, um Brute-Force-Angriffe zu verhindern  
- [ ] Ja — Rate Limiting **wird angewendet**, aber ein Bypass **ist möglich** über IP-Rotation oder Header-Manipulation  
- [ ] Nein — es wird **kein Rate Limiting** für Antwortversuche erzwungen  

### Erzwingt die Anwendung Komplexität oder Validierung für den Inhalt der Antwort?
- [ ] Ja — die Validierung verhindert gängige Wörter und stellt die Komplexität/Einzigartigkeit der Antwort sicher  
- [ ] Ja — eine grundlegende Validierung ist vorhanden, kann aber mit gängigen Wortlisten oder Dictionary Attacks **umgangen werden**  
- [ ] Nein — es wird **keine Validierung** durchgeführt; einfache oder einstellige Antworten **sind zulässig**  

### Kann die Abfrage der Sicherheitsfrage durch logische Fehler umgangen werden?
- [ ] Nein — der Wiederherstellungs-Flow erfordert zwingend eine gültige Antwort, um fortzufahren  
- [ ] Ja — der Wiederherstellungs-Flow **kann** durch Manipulation von HTTP-Response-Codes oder Session-Parametern umgangen werden  
- [ ] Ja — die Antwort wird im Quellcode oder in versteckten Feldern (Hidden Fields) der Seite reflektiert  

---