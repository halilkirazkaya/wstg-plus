## WSTG-CRYP-02 — Test de Padding Oracle

Les vulnérabilités de type Padding Oracle surviennent lorsque le processus de déchiffrement d'une application divulgue des informations indiquant si le remplissage (padding) d'un texte chiffré (ciphertext) déchiffré est valide ou non. En observant ces réponses par canaux auxiliaires (side-channel)—telles que des codes d'état HTTP distincts, des messages d'erreur spécifiques ou des variations de temps de réponse—un attaquant peut déchiffrer des données sans connaître la clé de chiffrement et potentiellement ré-encrypter du texte clair (plaintext) arbitraire. Cette vulnérabilité se manifeste généralement dans les cookies de session chiffrés, les jetons d'authentification ou les paramètres d'URL utilisant des chiffrements par blocs en mode Cipher Block Chaining (CBC) avec un remplissage PKCS#7. L'exploitation permet une perte totale de confidentialité et peut conduire à un compromis d'intégrité par la construction de blocs chiffrés forgés.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CRYP-02 |
| **CWE** | CWE-209 |
| **Statut du Test** | Non effectué |
| **Sévérité** | Élevée / Critique* |

> *La sévérité est Critique si la vulnérabilité réside dans la gestion de session, les jetons d'identité ou le chiffrement de données PII.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/02-Testing_for_Padding_Oracle  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `PadBuster`, `Burp Suite (Intruder/Padding Oracle Solver)`, `pkcs7-padbuster`, `CyberChef`

### Un chiffrement par blocs est-il utilisé en mode CBC pour les données sensibles ?
- [ ] Non — l'application utilise un chiffrement authentifié (AES-GCM/ChaCha20-Poly1305)  
- [ ] Oui — l'application utilise des chiffrements par blocs mais le remplissage (padding) **n'est pas** requis (ex : chiffrements de flux ou mode CTR)  
- [ ] Oui — l'application utilise le mode CBC et un remplissage (padding) **est appliqué**  

### Le serveur divulgue-t-il la validité du remplissage via des canaux auxiliaires ?
- [ ] Non — le serveur renvoie des messages d'erreur uniformes et des temps de réponse cohérents  
- [ ] Oui — le serveur renvoie différents codes d'état HTTP (ex : 200 vs 500) pour les erreurs de remplissage  
- [ ] Oui — le serveur renvoie des messages d'erreur spécifiques (ex : "Invalid Padding", "Decryption failed")  
- [ ] Oui — le serveur présente des différences de temps mesurables lors d'un échec de déchiffrement  

### Le déchiffrement automatisé du texte chiffré est-il possible ?
- [ ] Non — les canaux auxiliaires (side-channels) ne sont **pas** assez stables pour l'exploitation  
- [ ] Oui — le texte chiffré (ciphertext) **peut** être partiellement déchiffré (derniers blocs)  
- [ ] Oui — la récupération complète du texte clair (plaintext) **est possible** en utilisant des outils comme `PadBuster`  

### L'attaquant peut-il effectuer du bit-flipping ou forger des textes chiffrés valides ?
- [ ] Non — les contrôles d'intégrité (HMAC) sont vérifiés **avant** le déchiffrement  
- [ ] Oui — aucun contrôle d'intégrité n'est présent, mais la construction du Payload **n'est pas** réalisable  
- [ ] Oui — la construction de texte chiffré arbitraire **est possible**, permettant une élévation de privilèges ou une injection de données  

---