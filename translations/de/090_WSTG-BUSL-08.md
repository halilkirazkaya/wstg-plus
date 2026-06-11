## WSTG-BUSL-08 — Test auf das Hochladen unerwarteter Dateitypen

Das Hochladen unerwarteter Dateitypen ermöglicht es Angreifern, Geschäftslogik-Einschränkungen zu umgehen, indem sie Dateien einreichen, die von der Anwendung nicht verarbeitet werden sollen, was potenziell zu Remote Code Execution, Cross-Site Scripting oder Denial of Service führen kann. Angreifer untersuchen diese Schwachstellen, indem sie Dateierweiterungen, MIME-Typen und Magic Bytes modifizieren, um die serverseitige Validierungslogik zur Annahme bösartiger Inhalte zu verleiten. Dies geschieht typischerweise bei Profilbild-Uploads, Dokumentenmanagementsystemen oder Anhängen für Support-Tickets, wenn im Back-End eine unzureichende Validierung vorliegt. Eine erfolgreiche Ausnutzung kann den Host-Server gefährden, wenn die hochgeladene Datei ausgeführt wird, oder zu clientseitigen Angriffen führen, wenn die Datei anderen Benutzern mit einem falschen Content-Type ausgeliefert wird.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-BUSL-08 |
| **CWE** | CWE-434 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Kritisch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/08-Test_Upload_of_Unexpected_File_Types  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**Tools:** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`

### Beschränkt die Anwendung Datei-Uploads auf eine bestimmte Auswahl an Erweiterungen?
- [ ] Ja — eine strikte Whitelist von Erweiterungen wird erzwungen und ein Bypass ist **nicht möglich**  
- [ ] Ja — eine Whitelist für Erweiterungen ist vorhanden, aber ein Bypass ist über doppelte Dateierweiterungen oder Null Bytes **möglich**  
- [ ] Nein — jede Dateierweiterung **kann** hochgeladen werden  

### Wird der Dateiinhalt über die Dateierweiterung hinaus validiert?
- [ ] Ja — die serverseitige Logik verifiziert Magic Bytes und führt eine tiefgehende Inhaltsprüfung durch  
- [ ] Ja — die serverseitige Logik verifiziert den MIME-Typ ausschließlich über den `Content-Type`-Header  
- [ ] Nein — nach der Prüfung der Erweiterung **wird keine** Inhaltsvalidierung angewendet  

### Kann ein Angreifer durch das Hochladen einer Web Shell oder eines Skripts Code ausführen?
- [ ] Nein — hochgeladene Dateien werden in einem nicht ausführbaren Verzeichnis oder einem Storage Bucket (S3/Azure Blob) gespeichert  
- [ ] Ja — Dateien werden in einem öffentlich zugänglichen Verzeichnis gespeichert, aber die Ausführung **ist** über die Serverkonfiguration **deaktiviert**  
- [ ] Ja — hochgeladene bösartige Skripte **können** direkt über einen vorhersehbaren URL-Pfad ausgeführt werden *(Kritisch)*  

### Sind clientseitige Angriffe wie XSS über hochgeladene Dateitypen möglich?
- [ ] Nein — Dateien werden mit `Content-Disposition: attachment` und `X-Content-Type-Options: nosniff` ausgeliefert  
- [ ] Ja — SVG- oder HTML-Dateien **können** hochgeladen und im Browser gerendert werden, was zu XSS führt  
- [ ] Ja — Bildmetadaten (EXIF) **werden verarbeitet** und ohne Sanitization in der Benutzeroberfläche angezeigt  

---