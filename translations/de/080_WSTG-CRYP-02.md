## WSTG-CRYP-02 — Prüfung auf Padding Oracle

Padding-Oracle-Schwachstellen treten auf, wenn der Entschlüsselungsprozess einer Anwendung Informationen darüber preisgibt, ob das Padding eines entschlüsselten Chiffretexts (Ciphertext) gültig oder ungültig ist. Durch die Beobachtung dieser Seitenkanal-Antworten – wie unterschiedliche HTTP-Statuscodes, spezifische Fehlermeldungen oder Variationen in der Antwortzeit – kann ein Angreifer Daten entschlüsseln, ohne den Verschlüsselungsschlüssel (Encryption Key) zu kennen, und potenziell beliebigen Klartext (Plaintext) neu verschlüsseln. Diese Schwachstelle tritt typischerweise in verschlüsselten Session-Cookies, Authentifizierungs-Token oder URL-Parametern auf, die Blockchiffren im Cipher Block Chaining (CBC) Modus mit PKCS#7-Padding verwenden. Eine Ausnutzung (Exploitation) ermöglicht den vollständigen Verlust der Vertraulichkeit und kann durch die Konstruktion gefälschter verschlüsselter Blöcke zu einer Beeinträchtigung der Integrität führen.

| Feld | Wert |
|---|---|
| **WSTG ID** | WSTG-CRYP-02 |
| **CWE** | CWE-209 |
| **Teststatus** | Nicht durchgeführt |
| **Schweregrad** | Hoch / Kritisch* |

> *Der Schweregrad ist kritisch, wenn sich die Schwachstelle in der Sitzungsverwaltung, in Identitäts-Token oder in der Verschlüsselung von PII (personenbezogenen Daten) befindet.

**Referenzen:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/02-Testing_for_Padding_Oracle  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `PadBuster`, `Burp Suite (Intruder/Padding Oracle Solver)`, `pkcs7-padbuster`, `CyberChef`

### Wird eine Blockchiffre im CBC-Modus für sensible Daten verwendet?
- [ ] Nein — Die Anwendung verwendet authentifizierte Verschlüsselung (AES-GCM/ChaCha20-Poly1305)  
- [ ] Ja — Die Anwendung verwendet Blockchiffren, aber ein Padding ist **nicht** erforderlich (z. B. Stromchiffren oder CTR-Modus)  
- [ ] Ja — Die Anwendung verwendet den CBC-Modus und ein Padding **wird angewendet**  

### Gibt der Server die Gültigkeit des Paddings über Seitenkanäle preis?
- [ ] Nein — Der Server liefert einheitliche Fehlermeldungen und konsistente Antwortzeiten  
- [ ] Ja — Der Server gibt unterschiedliche HTTP-Statuscodes (z. B. 200 vs. 500) bei Padding-Fehlern zurück  
- [ ] Ja — Der Server gibt spezifische Fehlermeldungen zurück (z. B. "Invalid Padding", "Decryption failed")  
- [ ] Ja — Der Server weist messbare Zeitunterschiede bei Entschlüsselungsfehlern auf  

### Ist eine automatisierte Entschlüsselung des Chiffretexts möglich?
- [ ] Nein — Die Seitenkanäle sind **nicht** stabil genug für eine Ausnutzung  
- [ ] Ja — Der Chiffretext **kann** teilweise entschlüsselt werden (die letzten Blöcke)  
- [ ] Ja — Eine vollständige Klartext-Wiederherstellung (Plaintext Recovery) **ist möglich** unter Verwendung von Tools wie `PadBuster`  

### Kann der Angreifer Bit-Flipping durchführen oder gültige Chiffretexte fälschen?
- [ ] Nein — Integritätsprüfungen (HMAC) werden **vor** der Entschlüsselung verifiziert  
- [ ] Ja — Es ist keine Integritätsprüfung vorhanden, aber die Konstruktion des Payloads **ist nicht** praktikabel  
- [ ] Ja — Die Konstruktion beliebiger Chiffretexte **ist möglich**, was eine Rechteausweitung (Privilege Escalation) oder Dateninjektion erlaubt  

---