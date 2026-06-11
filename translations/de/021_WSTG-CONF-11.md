## WSTG-CONF-11 — Cloud-Speicher prüfen

Die Prüfung auf Fehlkonfigurationen von Cloud-Speichern umfasst die Identifizierung und Überprüfung öffentlich zugänglicher Object Storage Services wie Amazon S3, Azure Blobs oder Google Cloud Storage. Diese Ressourcen enthalten häufig sensible Daten wie Backups, Quellcode, PII (Personally Identifiable Information) oder Konfigurationsdateien, die aufgrund zu permissiver Access Control Lists (ACLs) oder Identity and Access Management (IAM) Policies offengelegt werden. Angreifer enumerieren Bucket-Namen durch Reconnaissance von JavaScript-Dateien, HTML-Quellcode und Brute-Force-Techniken, um Einstiegspunkte für Data Exfiltration oder unbefugte Dateiänderungen zu finden. Die Ausnutzung (Exploitation) kann erhebliche Auswirkungen haben, einschließlich vollständiger Datenschutzverletzungen (Data Breaches) oder der Möglichkeit, bösartige Dateien über eine vertrauenswürdige Domain bereitzustellen.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CONF-11 |
| **CWE** | CWE-922 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Kritisch* |

> *Der Schweregrad wird als Kritisch eingestuft, wenn PII, Anmeldedaten oder Produktions-Backups zugänglich sind oder wenn ein nicht authentifizierter Schreibzugriff aktiviert ist.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/11-Test_Cloud_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `aws-cli`, `gsutil`, `azure-cli`, `S3Scanner`, `CloudEnum`, `FFUF`, `Burp Suite`

### Werden Cloud-Storage-Endpunkte (Buckets/Container) identifiziert und enumeriert?
- [ ] Nein — es werden keine Cloud-Storage-Endpunkte verwendet oder diese sind nicht auffindbar  
- [ ] Ja — Storage-Endpunkte werden durch Code-Analyse oder Traffic-Monitoring identifiziert  
- [ ] Ja — Storage-Endpunkte werden durch Brute-Force auf Namenskonventionen entdeckt  

### Können nicht authentifizierte Benutzer den Inhalt des Cloud-Speichers auflisten (Listing)?
- [ ] Nein — Bucket-Listing ist **deaktiviert** und gibt 403 Forbidden oder 404 Not Found zurück  
- [ ] Ja — Listing ist **aktiviert**, aber es sind keine sensiblen Dateien vorhanden  
- [ ] Ja — Listing ist **aktiviert** und sensible Dateinamen/Metadaten sind sichtbar  

### Ist das Herunterladen sensibler Dateien von identifizierten Buckets möglich?
- [ ] Nein — Dateien sind durch IAM-Policies geschützt oder eine gültige Authentifizierung ist erforderlich  
- [ ] Ja — Dateien sind zugänglich, erfordern jedoch eine Signed URL, die **nicht** einfach erraten werden kann  
- [ ] Ja — sensible Dateien (Backups, `.env`, PII) sind für den Download **öffentlich zugänglich** *(Kritisch)*  

### Ist ein nicht authentifizierter Dateiupload oder eine Dateiänderung zulässig?
- [ ] Nein — nicht authentifizierte Uploads oder Änderungen sind **nicht möglich**  
- [ ] Ja — ein authentifizierter Upload ist erforderlich, aber ein Bypass **ist möglich** über schwache Policy-Bedingungen  
- [ ] Ja — nicht authentifiziertes Schreiben, Überschreiben oder Löschen von Dateien ist **möglich** *(Kritisch)*  

### Sind die Cross-Origin Resource Sharing (CORS) Policies am Storage-Endpunkt restriktiv?
- [ ] Ja — CORS ist **deaktiviert** oder auf bestimmte vertrauenswürdige Ursprünge (Origins) beschränkt  
- [ ] Nein — CORS ist mit einer Wildcard `*` Origin **aktiviert**, was unbefugten webbasierten Zugriff ermöglicht  

---