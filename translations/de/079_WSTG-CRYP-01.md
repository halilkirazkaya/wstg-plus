## WSTG-CRYP-01 — Prüfung auf schwache Transport Layer Security

Die Prüfung auf schwache Transport Layer Security umfasst die Analyse der Implementierung und Konfiguration von SSL/TLS, um die Vertraulichkeit und Integrität der Daten während der Übertragung zu gewährleisten. Angreifer zielen auf Fehlkonfigurationen wie veraltete Protokolle (SSLv2, TLS 1.0), schwache Cipher Suites (NULL, RC4 oder anonym) sowie ungültige oder schwach signierte Zertifikate ab, um Man-in-the-Middle (MitM)-Angriffe durchzuführen. Dieser Test richtet sich in der Regel gegen die Webserver- oder Load Balancer-Endpunkte der Anwendung, an denen die verschlüsselte Kommunikation aufgebaut wird. Eine erfolgreiche Ausnutzung ermöglicht es einem Angreifer, sensible Daten abzufangen, Benutzersitzungen zu übernehmen (Session Hijacking) oder Verbindungen durch Protokoll-Downgrades oder Entschlüsselung des Datenverkehrs in unsichere Zustände zu versetzen.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CRYP-01 |
| **CWE** | CWE-326 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Mittel* |

> *Der Schweregrad ist Hoch, wenn sensible Daten (PII, Anmeldedaten, Session-Tokens) übertragen werden; Mittel für allgemeinen Informationsverkehr.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/01-Testing_for_Weak_Transport_Layer_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `testssl.sh`, `sslyze`, `nmap`, `OpenSSL`, `Qualys SSL Labs`

### Werden veraltete oder unsichere TLS/SSL-Protokolle unterstützt?
- [ ] Nein — nur TLS 1.2 und TLS 1.3 sind **aktiviert**  
- [ ] Ja — TLS 1.0 oder TLS 1.1 sind **aktiviert**  
- [ ] Ja — SSLv2 oder SSLv3 sind **aktiviert** *(Kritisch)*  

### Werden schwache oder unsichere Cipher Suites vom Server akzeptiert?
- [ ] Nein — es werden nur starke, moderne Ciphers (z. B. AES-GCM, ChaCha20) **unterstützt**  
- [ ] Ja — Ciphers mit schwacher Verschlüsselung (z. B. 3DES, RC4) oder geringer Bitlänge werden **unterstützt**  
- [ ] Ja — NULL, anonyme (ADH) oder Export-Ciphers werden **unterstützt** *(Kritisch)*  

### Ist das Server-Zertifikat gültig und vertrauenswürdig?
- [ ] Ja — das Zertifikat ist gültig, stimmt mit der Domain überein und ist von einer **vertrauenswürdigen CA** signiert  
- [ ] Nein — das Zertifikat ist **abgelaufen** oder verwendet einen **schwachen Signaturalgorithmus** (z. B. SHA-1)  
- [ ] Nein — das Zertifikat ist **selbstsigniert** oder es liegt ein Domain-Name-**Mismatch** vor  

### Unterstützt der Server Secure Renegotiation und verhindert er Downgrade-Angriffe?
- [ ] Ja — Secure Renegotiation wird **unterstützt** und TLS Fallback SCSV ist **aktiviert**  
- [ ] Nein — Secure Renegotiation wird **nicht unterstützt**, was potenziell MitM-Injektionen ermöglicht  
- [ ] Nein — Der Server ist anfällig für **POODLE**- oder **ROBOT**-Angriffe  

### Ist HTTP Strict Transport Security (HSTS) ordnungsgemäß implementiert?

| Header | Vorhanden | Fehlt |
|--------|:-------:|:-------:|
| `Strict-Transport-Security` |  |  |

- [ ] Ja — der HSTS-Header ist vorhanden, weist eine lange `max-age` auf und `includeSubDomains` ist **aktiviert**  
- [ ] Ja — der HSTS-Header ist vorhanden, aber `includeSubDomains` fehlt oder die `max-age` ist **kurz**  
- [ ] Nein — der HSTS-Header ist **nicht vorhanden**  

---