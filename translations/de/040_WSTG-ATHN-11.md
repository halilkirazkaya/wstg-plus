## WSTG-ATHN-11 — Prüfung der Mehrfaktor-Authentisierung (MFA)

Die Prüfung der Mehrfaktor-Authentisierung (MFA) bewertet die Robustheit der sekundären Sicherheitsschicht, die unbefugten Zugriff verhindern soll, selbst wenn die primären Anmeldedaten kompromittiert wurden. Angreifer versuchen häufig, die MFA zu umgehen, indem sie Endpunkte identifizieren, an denen diese nicht konsistent erzwungen wird, wie etwa veraltete API-Versionen, Mobile-Backends oder Workflows zum Zurücksetzen von Passwörtern. Die Ausnutzung umfasst häufig die Manipulation von Serverantworten, Brute-Force-Angriffe auf kurzlebige Codes (OTP), oder das Ausnutzen von Race Conditions und Schwachstellen im Session Management, die es einem Benutzer ermöglichen, in einen authentifizierten Zustand überzugehen, ohne den zweiten Faktor anzugeben.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-ATHN-11 |
| **CWE** | CWE-287 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/11-Testing_Multi-Factor_Authentication  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite (Intruder/Repeater/Turbo Intruder)`, `Postman`, `Mitproxy`

### Wird MFA konsistent über alle Authentifizierungsportale hinweg erzwungen?
- [ ] Ja — MFA ist für alle web-, mobil- und API-basierten Login-Versuche erforderlich  
- [ ] Ja — aber MFA wird an spezifischen Endpunkten **nicht erzwungen** (z. B. veraltete `/api/v1/login` oder mobilspezifische Portale)  
- [ ] Nein — MFA ist in der Anwendung **nicht implementiert** *(Kritisch)*  

### Kann der MFA-Verifizierungsschritt durch direkte URL-Navigation oder Response-Manipulation umgangen werden?
- [ ] Nein — direkte Navigation oder Parameter-Tampering können die Abfrage **nicht** umgehen  
- [ ] Ja — das direkte Navigieren zu internen Dashboard-URLs **ist möglich**, ohne die MFA-Abfrage abzuschließen  
- [ ] Ja — die Manipulation der Serverantwort (z. B. Ändern eines `401 Unauthorized` in `200 OK`) **ist möglich**, um Zugriff zu erhalten  

### Ist der Prozess der MFA-Codeverifizierung gegen Brute-Force-Angriffe geschützt?
- [ ] Ja — striktes Rate Limiting oder Account-Lockout **wird angewendet**, nachdem mehrere OTP-Versuche fehlgeschlagen sind  
- [ ] Ja — Rate Limiting existiert, aber eine Umgehung **ist möglich** durch IP-Rotation oder Header-Manipulation  
- [ ] Nein — es wird kein Rate Limiting erzwungen und automatisiertes Brute-Forcing von Codes **ist möglich**  

### Behält die Anwendung während des MFA-Übergangs einen sicheren Session-Status bei?
- [ ] Ja — hochprivilegierte Session-Token werden **erst nach** erfolgreichem Abschluss der MFA ausgegeben  
- [ ] Nein — ein voll funktionsfähiges Session-Cookie wird bereits **vor** Abschluss der MFA ausgegeben, was den Zugriff auf einige Funktionen ermöglicht  
- [ ] Nein — die Anwendung verwendet einen statischen Identifikator oder einen vorhersehbaren Session-Übergang, der übernommen (hijacked) werden **kann**  

### Sind alternative MFA-Faktoren (SMS, E-Mail, Backup-Codes) anfällig für eine Ausnutzung?
- [ ] Nein — alle Faktoren verwenden sichere, nicht vorhersehbare und zeitlich begrenzte Werte  
- [ ] Ja — Backup-Codes sind vorhersehbar oder **können** enumeriert werden  
- [ ] Ja — die „Code erneut senden“-Funktionalität kann missbraucht werden, um SMS/E-Mail-Flooding durchzuführen oder Teile der Kontaktdaten offenzulegen  

---