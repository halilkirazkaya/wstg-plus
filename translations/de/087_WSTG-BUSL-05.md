## WSTG-BUSL-05 — Prüfung der Nutzungslimits von Funktionen

Dieser Test bewertet, ob eine Anwendung Geschäftslogik-Einschränkungen (Business Logic Constraints) für die Häufigkeit und Gesamtzahl der Ausführungen einer bestimmten Aktion oder Funktion durch einen einzelnen Benutzer, eine Session oder eine IP-Adresse erzwingt. Das Fehlen dieser Limits ermöglicht es Angreifern, hochwertige Funktionen zu missbrauchen – wie den Versand von SMS OTPs, das Auslösen von Passwort-Reset-E-Mails, das Anwenden von Rabattcodes oder die Durchführung umfangreicher Datenexporte –, was zu finanziellen Verlusten, Ressourcenerschöpfung oder Reputationsschäden führen kann. Diese Schwachstellen finden sich typischerweise in transaktionalen Endpunkten oder Kommunikationsfunktionen und werden durch automatisierte Skripte ausgenutzt, die Aktionen weit über die beabsichtigten Geschäftsschwellenwerte hinaus wiederholen. Aus der Sicht eines Angreifers ist dies ein primäres Ziel für SMS Pumping, Mail Bombing oder das Brute-Forcing von Workflows, denen explizites Rate Limiting oder zählerbasierte Drosselungen fehlen.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-BUSL-05 |
| **CWE** | CWE-799 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Mittel / Hoch* |

> *Der Schweregrad wird als "Hoch" eingestuft, wenn die Funktion Finanztransaktionen oder SMS-Kosten betrifft oder die systemweite Verfügbarkeit beeinträchtigt.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/05-Test_Number_of_Times_a_Function_Can_Be_Used_Limits  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Intruder/Repeater)`, `Turbo Intruder`, `Python (requests)`, `OWASP ZAP`

### Definiert und erzwingt die Anwendung Limits für sensible Geschäftsfunktionen?
- [ ] Ja — Limits werden strikt erzwungen und ein Bypass ist **nicht möglich**  
- [ ] Ja — Limits werden erzwungen, sind aber zu hoch, um Missbrauch zu verhindern  
- [ ] Nein — es werden keine Limits auf die Funktionsausführung angewendet  

### Werden Rate Limits und Nutzungszähler serverseitig erzwungen?
- [ ] Ja — die Durchsetzung erfolgt strikt serverseitig und **kann nicht** manipuliert werden  
- [ ] Nein — die Durchsetzung stützt sich auf clientseitige Logik (z. B. JavaScript oder versteckte Felder)  
- [ ] Nein — es findet auf keiner Ebene eine Durchsetzung statt  

### Können die Nutzungslimits durch Header- oder Session-Manipulation umgangen werden?
- [ ] Nein — ein Bypass via `X-Forwarded-For`, `Origin` oder Session-Rotation ist **nicht möglich**  
- [ ] Ja — Limits **können** durch Spoofing von IP-Headern oder das Löschen von Cookies umgangen werden  
- [ ] Ja — Limits **können** durch das Erstellen mehrerer Accounts oder Sessions umgangen werden  

### Löst das Überschreiten des Limits eine angemessene Abwehrreaktion aus?
- [ ] Ja — die Anwendung gibt `429 Too Many Requests` zurück und protokolliert das Ereignis *(Am sichersten)*  
- [ ] Ja — die Anwendung gibt einen Fehler zurück, protokolliert aber den potenziellen Missbrauch **nicht**  
- [ ] Nein — die Anwendung verarbeitet Anfragen weiter, ignoriert aber die Ausgabe  
- [ ] Nein — die Anwendung liefert keine Antwort oder stürzt unter Last ab  

### Ist die Auswirkung des Funktionsmissbrauchs für das Unternehmen erheblich?
- [ ] Nein — der Missbrauch der Funktion hat vernachlässigbare Kosten- oder Ressourceneffekte  
- [ ] Ja — Missbrauch **kann** zu geringfügigem Ressourcenverbrauch oder Belästigung der Benutzer führen  
- [ ] Ja — Missbrauch **kann** zu erheblichen finanziellen Kosten (z. B. SMS-Gebühren) oder Dienstverweigerung (Service Denial) führen *(Kritisch)*  

---