## WSTG-CRYP-04 — Testing for Weak Cryptographic Primitives

Schwache kryptografische Primitive umfassen die Verwendung veralteter oder unsicherer Algorithmen wie MD5, SHA-1, DES oder RC4, die anfällig für Kollisionsangriffe (Collision Attacks), Frequenzanalysen oder Brute Force Entschlüsselung sind. Diese Schwachstellen treten typischerweise beim Passwort-Hashing, der Verschlüsselung von ruhenden Daten (Data at Rest), der Generierung von Session-Token oder der Verifizierung digitaler Signaturen innerhalb des Anwendungs-Backends auf. Aus der Sicht eines Angreifers ermöglicht die Identifizierung dieser Primitiven die Kompromittierung der Datenintegrität und Vertraulichkeit, wie etwa das Cracken abgefangener Hashes zur Wiederherstellung von Klartext-Passwörtern oder das Fälschen von Authentifizierungs-Token. Moderne Anwendungen sollten stattdessen industriestandardisierte, kollisionsresistente und hoch-entropische Primitive wie Argon2, Bcrypt oder AES-GCM einsetzen, um einen robusten Schutz gegen Kryptanalyse zu gewährleisten.


| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CRYP-04 |
| **CWE** | CWE-327 |
| **Test Status** | Nicht durchgeführt |
| **Schweregrad** | Hoch |

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/04-Testing_for_Weak_Cryptographic_Primitives  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Werkzeuge:** `hashcat`, `John the Ripper`, `Burp Suite`, `CyberChef`, `OpenSSL`, `nmap (ssl-enum-ciphers)`

### Werden veraltete Hashing-Algorithmen wie MD5 oder SHA-1 für die Speicherung sensibler Daten verwendet?
- [ ] Nein — Die Anwendung verwendet moderne kollisionsresistente Algorithmen wie Argon2 oder Bcrypt  
- [ ] Ja — Veraltete Algorithmen werden verwendet, aber **nur** für nicht sicherheitsrelevante Integritätsprüfungen  
- [ ] Ja — Veraltete Algorithmen **werden** für Passwörter oder sensible Token verwendet *(Hoch)*  

### Kommen derzeit schwache symmetrische Verschlüsselungsverfahren wie DES, 3DES oder RC4 zum Einsatz?
- [ ] Nein — AES (128-Bit oder höher) in einem sicheren Modus wie GCM oder CBC wird verwendet  
- [ ] Ja — Legacy-Verschlüsselungsverfahren sind für Abwärtskompatibilität **aktiviert**, aber **nicht** der Standard  
- [ ] Ja — Legacy-Verschlüsselungsverfahren sind die primäre Methode zum Schutz von Daten im Ruhezustand oder bei der Übertragung  

### Implementiert die Anwendung eigenentwickelte ("roll-your-own") oder nicht standardisierte kryptografische Logik?
- [ ] Nein — Die Anwendung hält sich strikt an standardisierte, von Experten geprüfte kryptografische Bibliotheken  
- [ ] Ja — Eigene Wrapper existieren, aber die zugrunde liegenden Primitiven **sind** Industriestandard  
- [ ] Ja — Proprietäre kryptografische Algorithmen oder benutzerdefinierte Logik **wird** auf sensible Daten angewendet *(Kritisch)*  

### Werden kryptografische Salts und Initialisierungsvektoren (IVs) gemäß Best Practices verwaltet?
- [ ] Ja — Salts sind pro Benutzer eindeutig und IVs sind für jede Operation unvorhersehbar  
- [ ] Ja — Salts werden verwendet, sind aber **nicht** über alle Datensätze hinweg eindeutig  
- [ ] Nein — Salts sind statisch, hartkodiert oder fehlen ganz, und IVs **werden** über mehrere Sitzungen hinweg wiederverwendet  

### Ist die Bit-Länge asymmetrischer Schlüssel (RSA, Diffie-Hellman) für moderne Standards ausreichend?
- [ ] Ja — RSA/DH-Schlüssel haben 2048 Bit oder mehr, oder es wird Elliptic Curve (ECC) verwendet  
- [ ] Nein — Schlüssel haben weniger als 2048 Bit, aber ein Bypass ist **nicht** trivial  
- [ ] Nein — Schlüssel haben 1024 Bit oder weniger, was sie **anfällig** für Faktorisierung oder Rechenangriffe macht  

---