## WSTG-CRYP-04 — Testen op Zwakke Cryptografische Primitives

Zwakke cryptografische primitives omvatten het gebruik van verouderde of onveilige algoritmen, zoals MD5, SHA-1, DES of RC4, die vatbaar zijn voor collision attacks, frequentieanalyse of brute-force decryptie. Deze zwakheden treden doorgaans op bij password hashing, data encryption at rest, generatie van session tokens of verificatie van digitale handtekeningen binnen de backend van de applicatie. Vanuit het perspectief van een aanvaller maakt het identificeren van deze primitives de inbreuk op data-integriteit en vertrouwelijkheid mogelijk, zoals het kraken van onderschepte hashes om cleartext wachtwoorden te achterhalen of het vervalsen van authenticatie-tokens. Moderne applicaties moeten in plaats daarvan gebruikmaken van industriestandaard, collision-resistant en high-entropy primitives zoals Argon2, Bcrypt of AES-GCM om robuuste bescherming tegen cryptanalyse te garanderen.

| Veld | Waarde |
|---|---|
| **WSTG ID** | WSTG-CRYP-04 |
| **CWE** | CWE-327 |
| **Teststatus** | Niet uitgevoerd |
| **Ernst** | Hoog |

**Referenties:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/04-Testing_for_Weak_Cryptographic_Primitives  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `hashcat`, `John the Ripper`, `Burp Suite`, `CyberChef`, `OpenSSL`, `nmap (ssl-enum-ciphers)`

### Worden verouderde hashing-algoritmen zoals MD5 of SHA-1 gebruikt voor de opslag van gevoelige gegevens?
- [ ] Nee — applicatie maakt gebruik van moderne collision-resistant algoritmen zoals Argon2 of Bcrypt  
- [ ] Ja — verouderde algoritmen worden gebruikt, maar **alleen** voor niet-beveiligingsgevoelige integriteitscontroles  
- [ ] Ja — verouderde algoritmen **worden gebruikt** voor wachtwoorden of gevoelige tokens *(Hoog)*  

### Worden er momenteel zwakke symmetrische encryptie-ciphers zoals DES, 3DES of RC4 toegepast?
- [ ] Nee — AES (128-bit of hoger) in een veilige modus zoals GCM of CBC wordt gebruikt  
- [ ] Ja — legacy ciphers zijn **ingeschakeld** voor achterwaartse compatibiliteit, maar zijn **niet** de standaard  
- [ ] Ja — legacy ciphers zijn de primaire methode voor het beschermen van data at rest of in transit  

### Implementeert de applicatie "roll-your-own" of niet-standaard cryptografische logica?
- [ ] Nee — applicatie houdt zich strikt aan standaard, peer-reviewed cryptografische libraries  
- [ ] Ja — er bestaan aangepaste wrappers, maar de onderliggende primitives **zijn** industriestandaard  
- [ ] Ja — eigen cryptografische algoritmen of aangepaste logica **wordt toegepast** op gevoelige gegevens *(Kritiek)*  

### Worden cryptografische salts en initialization vectors (IVs) beheerd volgens best practices?
- [ ] Ja — salts zijn uniek per gebruiker en IVs zijn onvoorspelbaar voor elke operatie  
- [ ] Ja — salts worden gebruikt, maar zijn **niet** uniek over alle records  
- [ ] Nee — salts zijn statisch, hardcoded of afwezig, en IVs **worden hergebruikt** over meerdere sessies  

### Is de bit-lengte van asymmetrische sleutels (RSA, Diffie-Hellman) voldoende voor moderne standaarden?
- [ ] Ja — RSA/DH-sleutels zijn 2048 bits of hoger, of Elliptic Curve (ECC) wordt gebruikt  
- [ ] Nee — sleutels zijn minder dan 2048 bits, maar bypass **is niet** triviaal  
- [ ] Nee — sleutels zijn 1024 bits of kleiner, waardoor ze **kwetsbaar** zijn voor factorisatie of computationele aanvallen  

---