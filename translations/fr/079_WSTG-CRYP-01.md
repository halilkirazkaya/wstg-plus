## WSTG-CRYP-01 — Test des faiblesses de la sécurité de la couche de transport

Le test des faiblesses de la sécurité de la couche de transport (Transport Layer Security) implique l'analyse de l'implémentation et de la configuration de SSL/TLS pour garantir la confidentialité et l'intégrité des données pendant le transit. Les attaquants ciblent les mauvaises configurations telles que les protocoles obsolètes (SSLv2, TLS 1.0), les suites de chiffrement (Cipher Suites) faibles (NULL, RC4 ou anonymes) et les certificats invalides ou faiblement signés pour mener des attaques de type Man-in-the-Middle (MitM). Ce test cible généralement les points de terminaison du serveur web de l'application ou de l'équilibreur de charge (Load Balancer) où la communication chiffrée est établie. Une exploitation réussie permet à un adversaire d'intercepter des données sensibles, de détourner des sessions utilisateur ou de rétrograder les connexions vers des états non sécurisés via un Downgrade de protocole ou le déchiffrement du trafic.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CRYP-01 |
| **CWE** | CWE-326 |
| **Statut du Test** | Non effectué |
| **Sévérité** | Élevée / Moyenne* |

> *La sévérité est Élevée si des données sensibles (PII, identifiants, jetons de session) sont transmises ; Moyenne pour le trafic informatif général.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/01-Testing_for_Weak_Transport_Layer_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `testssl.sh`, `sslyze`, `nmap`, `OpenSSL`, `Qualys SSL Labs`

### Les protocoles TLS/SSL obsolètes ou non sécurisés sont-ils supportés ?
- [ ] Non — seuls TLS 1.2 et TLS 1.3 sont **activés**  
- [ ] Oui — TLS 1.0 ou TLS 1.1 sont **activés**  
- [ ] Oui — SSLv2 ou SSLv3 sont **activés** *(Critique)*  

### Des suites de chiffrement (cipher suites) faibles ou non sécurisées sont-elles acceptées par le serveur ?
- [ ] Non — seuls des chiffrements forts et modernes (ex : AES-GCM, ChaCha20) sont **supportés**  
- [ ] Oui — des chiffrements avec un cryptage faible (ex : 3DES, RC4) ou une faible longueur de bits sont **supportés**  
- [ ] Oui — les chiffrements NULL, anonymes (ADH) ou d'exportation sont **supportés** *(Critique)*  

### Le certificat du serveur est-il valide et fiable ?
- [ ] Oui — le certificat est valide, correspond au domaine et est signé par une **CA de confiance**  
- [ ] Non — le certificat a **expiré** ou utilise un **algorithme de signature faible** (ex : SHA-1)  
- [ ] Non — le certificat est **auto-signé** ou un **mismatch** de nom de domaine se produit  

### Le serveur supporte-t-il la Secure Renegotiation et empêche-t-il les attaques de type Downgrade ?
- [ ] Oui — la Secure Renegotiation est **supportée** et TLS Fallback SCSV est **activé**  
- [ ] Non — la Secure Renegotiation n'est **pas supportée**, permettant potentiellement une injection MitM  
- [ ] Non — le serveur est vulnérable aux attaques **POODLE** ou **ROBOT**  

### La sécurité HSTS (HTTP Strict Transport Security) est-elle correctement implémentée ?

| En-tête | Présent | Manquant |
|--------|:-------:|:-------:|
| `Strict-Transport-Security` |  |  |

- [ ] Oui — l'en-tête HSTS est présent avec un `max-age` long et `includeSubDomains` **activé**  
- [ ] Oui — l'en-tête HSTS est présent mais l'option `includeSubDomains` est manquante ou le `max-age` est **court**  
- [ ] Non — l'en-tête HSTS n'est **pas présent**  

---