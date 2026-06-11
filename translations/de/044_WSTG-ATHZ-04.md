## WSTG-ATHZ-04 — Prüfung auf Insecure Direct Object References

Insecure Direct Object References (IDOR) treten auf, wenn eine Anwendung direkten Zugriff auf Objekte basierend auf benutzereingebauten Eingaben gewährt, ohne eine Autorisierungsprüfung durchzuführen, um die Berechtigungen des Anfragenden zu verifizieren. Diese Schwachstelle beeinträchtigt die Vertraulichkeit und Integrität erheblich, da sie es Angreifern ermöglicht, Daten anderer Benutzer oder des Systems einzusehen, zu ändern oder zu löschen. Sie manifestiert sich typischerweise in URL-Parametern, POST-Body-Daten oder JSON-Keys, die auf interne Datenbankschlüssel, Dateinamen oder Account-Identifikatoren verweisen. Aus der Sicht eines Angreifers beinhaltet die Ausnutzung die Manipulation dieser Identifikatoren – oft durch Inkrementierung von Ganzzahlen oder Fuzzing von UUIDs –, um auf Ressourcen außerhalb des autorisierten Bereichs zuzugreifen.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-ATHZ-04 |
| **CWE** | CWE-639 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/04-Testing_for_Insecure_Direct_Object_References  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/access-control  

**Tools:** `Burp Suite (Autorize extension)`, `Caido`, `Postman`, `ffuf`, `Intruder`

### Verwendet die Anwendung direkte Objekt-Identifikatoren in Anfrageparametern?
- [ ] Nein — die Anwendung verwendet indirekte Referenzen oder verschlüsselte Maps *(Am sichersten)*  
- [ ] Ja — die Anwendung verwendet nicht vorhersehbare Identifikatoren (z. B. lange UUIDs/Hashes)  
- [ ] Ja — die Anwendung verwendet vorhersehbare Identifikatoren (z. B. inkrementelle Ganzzahlen oder Benutzernamen)  

### Wird für jede Anfrage, die einen Objekt-Identifikator enthält, eine serverseitige Autorisierung durchgeführt?
- [ ] Ja — serverseitige Prüfungen **können nicht** für einen Identifikator umgangen werden  
- [ ] Ja — serverseitige Prüfungen sind vorhanden, aber eine Umgehung **ist möglich** über Parameter Pollution oder Header-Manipulation  
- [ ] Nein — es **wird keine** Autorisierungsprüfung angewendet, um den Besitz des angeforderten Objekts zu verifizieren  

### Kann ein Angreifer auf Objekte zugreifen oder diese ändern, die anderen Benutzern gehören (Horizontal IDOR)?
- [ ] Nein — der Zugriff auf Daten anderer Benutzer **ist nicht möglich**  
- [ ] Ja — das Einsehen von Daten anderer Benutzer **ist möglich**, aber eine Änderung **ist nicht möglich**  
- [ ] Ja — sowohl das Einsehen als auch die Änderung von Daten anderer Benutzer **ist möglich** *(Kritisch)*  

### Ist es möglich, auf administrative oder systemnahe Objekte zuzugreifen (Vertical IDOR)?
- [ ] Nein — der Zugriff auf höher privilegierte Objekte **ist nicht möglich**  
- [ ] Ja — der Zugriff auf administrative Konfigurationen oder Systemdateien **ist möglich**  

### Gibt die Anwendung Objekt-Identifikatoren über die Suche, Metadaten oder andere Endpunkte preis?
- [ ] Nein — Identifikatoren werden **nicht unbeabsichtigt** offengelegt  
- [ ] Ja — Identifikatoren für andere Benutzer/Objekte **können** über sekundäre Endpunkte oder öffentliche Profile enumeriert werden  

---