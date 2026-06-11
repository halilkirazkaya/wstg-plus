## WSTG-INFO-05 — Überprüfung von Webseiteninhalten auf Informationsabfluss (Information Leakage)

Die Überprüfung von Webseiteninhalten umfasst die manuelle und automatisierte Inspektion von Anwendungsantworten, einschließlich HTML-, JavaScript- und CSS-Dateien, um unbeabsichtigt gegenüber Benutzern offengelegte sensible Informationen zu identifizieren. Angreifer untersuchen den clientseitigen Quellcode nach Entwicklerkommentaren, versteckten Formularfeldern, internen Serverpfaden, fest codierten API-Schlüsseln (API Keys) und Debugging-Informationen, die in weiteren Exploit-Phasen hilfreich sein können. Ein solcher Informationsabfluss (Information Leakage) tritt häufig auf, wenn Entwickler vergessen, Metadaten oder Debugging-Code vor dem Wechsel in die Produktion zu entfernen, was potenziell Logikfehler oder Details der Backend-Infrastruktur preisgibt. Aus Sicht eines Angreifers bietet dies eine aufwandsarme Methode, um die interne Struktur der Anwendung abzubilden und übersehene Einstiegspunkte (Entry Points) zu entdecken, ohne Sicherheitsalarme auszulösen oder direkt mit der Backend-Logik des Servers zu interagieren.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-INFO-05 |
| **CWE** | CWE-200 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Niedrig / Mittel* |

> *Der Schweregrad wird als "Hoch" eingestuft, wenn sensible fest codierte Anmeldedaten (Credentials), private Schlüssel (Private Keys) oder interne Umgebungsvariablen entdeckt werden.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/05-Review_Web_Page_Content_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**Werkzeuge:** `Burp Suite (Target/Proxy)`, `Browser Developer Tools`, `SecretFinder`, `grep`, `truffleHog`, `LinkFinder`

### Befinden sich Entwicklerkommentare mit sensiblen Informationen im Quellcode?
- [ ] Nein — keine Kommentare oder nur allgemeine, nicht-sensible Kommentare gefunden  
- [ ] Ja — Kommentare existieren, enthalten aber **keine** sensible Logik oder Daten  
- [ ] Ja — Kommentare **offenbaren** interne Pfade, SQL-Abfragen oder administrative Anweisungen  
- [ ] Ja — Kommentare **offenbaren** Anmeldedaten oder sensible Entwicklernotizen *(Hoch)*  

### Enthalten versteckte HTML-Eingabefelder sensible Daten oder Statusinformationen?
- [ ] Nein — keine versteckten Felder gefunden oder sie enthalten nur nicht-sensible Token  
- [ ] Ja — versteckte Felder werden für den Status verwendet, sind aber **verschlüsselt** oder **signiert**  
- [ ] Ja — versteckte Felder enthalten **Klartext-Passwörter**, Benutzerrollen oder interne IDs  

### Sind fest codierte Geheimnisse, API-Schlüssel oder interne IP-Adressen in JavaScript-Dateien vorhanden?
- [ ] Nein — keine sensiblen Zeichenfolgen oder Geheimnisse in JS-Dateien gefunden  
- [ ] Ja — JS-Dateien enthalten öffentliche API-Schlüssel mit **angemessenen** Beschränkungen  
- [ ] Ja — JS-Dateien enthalten **ungeschützte** API-Schlüssel, interne IPs oder private Endpunkte  
- [ ] Ja — JS-Dateien enthalten **fest codierte** administrative Anmeldedaten oder private Schlüssel *(Kritisch)*  

### Gibt die Anwendung Framework-Versionen oder interne Dateipfade im Quellcode preis?
- [ ] Nein — Framework-Informationen und Dateipfade sind **minimiert** (minified) oder **obfuskierte**  
- [ ] Ja — Framework-Namen und Versionen sind **sichtbar**, aber gepatcht  
- [ ] Ja — interne Dateipfade oder absolute Serverpfade sind im Quellcode **exponiert**  

### Ist der Quellcode aufgrund fehlender Minimierung oder Obfuskierung anfällig für Informationsabfluss?
- [ ] Nein — Produktionscode ist **vollständig minimiert** und von Kommentaren befreit  
- [ ] Ja — Code ist minimiert, aber Kommentare **verbleiben** im Quellcode  
- [ ] Ja — Source Maps (`.map` Dateien) sind **öffentlich zugänglich**, was eine vollständige Rekonstruktion des Quellcodes ermöglicht  

---