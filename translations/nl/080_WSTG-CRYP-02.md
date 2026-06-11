## WSTG-CRYP-02 — Testen op Padding Oracle

Padding Oracle-kwetsbaarheden treden op wanneer het decryptieproces van een applicatie informatie lekt over de vraag of de padding van een gedecrypteerde cijfertekst (ciphertext) valide of invalide is. Door deze side-channel-responsen te observeren — zoals afwijkende HTTP-statuscodes, specifieke foutmeldingen of variaties in responstijd — kan een aanvaller gegevens ontsleutelen zonder de encryptiesleutel te kennen en potentieel willekeurige klare tekst (plaintext) opnieuw versleutelen. Deze kwetsbaarheid manifesteert zich doorgaans in versleutelde sessiecookies, authenticatietokens of URL-parameters die gebruikmaken van blokcijfers (block ciphers) in Cipher Block Chaining (CBC)-modus met PKCS#7-padding. Exploitatie leidt tot een volledig verlies van vertrouwelijkheid en kan leiden tot integriteitsschendingen door de constructie van vervalste versleutelde blokken.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CRYP-02 |
| **CWE** | CWE-209 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst (Severity)** | Hoog / Kritiek* |

> *De ernst is Kritiek als de kwetsbaarheid zich bevindt in sessiebeheer, identiteitstokens of PII-encryptie.

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/02-Testing_for_Padding_Oracle  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `PadBuster`, `Burp Suite (Intruder/Padding Oracle Solver)`, `pkcs7-padbuster`, `CyberChef`

### Wordt er een blokcijfer in CBC-modus gebruikt voor gevoelige gegevens?
- [ ] Nee — de applicatie gebruikt geauthenticeerde encryptie (AES-GCM/ChaCha20-Poly1305)  
- [ ] Ja — de applicatie gebruikt blokcijfers, maar padding **is niet** vereist (bijv. stroomcijfers of CTR-modus)  
- [ ] Ja — de applicatie gebruikt CBC-modus en padding **wordt toegepast**  

### Maakt de server de geldigheid van de padding bekend via side-channels?
- [ ] Nee — de server retourneert uniforme foutmeldingen en consistente responstijden  
- [ ] Ja — de server retourneert verschillende HTTP-statuscodes (bijv. 200 vs. 500) voor padding-fouten  
- [ ] Ja — de server retourneert specifieke foutmeldingen (bijv. "Invalid Padding", "Decryption failed")  
- [ ] Ja — de server vertoont meetbare tijdsverschillen tijdens een mislukte decryptie  

### Is geautomatiseerde decryptie van de cijfertekst mogelijk?
- [ ] Nee — side-channels zijn **niet** stabiel genoeg voor exploitatie  
- [ ] Ja — cijfertekst **kan** gedeeltelijk worden gedecrypteerd (de laatste paar blokken)  
- [ ] Ja — volledige herstel van de klare tekst (plaintext) **is mogelijk** met tools zoals `PadBuster`  

### Kan de aanvaller bit-flipping uitvoeren of valide cijferteksten vervalsen?
- [ ] Nee — integriteitscontroles (HMAC) worden geverifieerd **vóór** de decryptie  
- [ ] Ja — er is geen integriteitscontrole aanwezig, maar payload-constructie **is niet** haalbaar  
- [ ] Ja — de constructie van willekeurige cijferteksten **is mogelijk**, wat privilege-escalatie of data-injectie toestaat  

---