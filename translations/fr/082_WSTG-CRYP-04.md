## WSTG-CRYP-04 — Test des primitives cryptographiques faibles

Les primitives cryptographiques faibles impliquent l'utilisation d'algorithmes obsolètes ou non sécurisés, tels que MD5, SHA-1, DES ou RC4, qui sont sensibles aux attaques par collision, à l'analyse fréquentielle ou au déchiffrement par Brute Force. Ces faiblesses surviennent généralement dans le hachage de mots de passe, le chiffrement des données au repos, la génération de jetons de session ou la vérification de signatures numériques au sein du backend de l'application. Du point de vue d'un attaquant, l'identification de ces primitives permet de compromettre l'intégrité et la confidentialité des données, par exemple en cassant des condensats (hashes) interceptés pour récupérer des mots de passe en clair ou en forgeant des jetons d'authentification. Les applications modernes devraient plutôt utiliser des primitives standards de l'industrie, résistantes aux collisions et à haute entropie, comme Argon2, Bcrypt ou AES-GCM pour garantir une protection robuste contre la cryptanalyse.

| Champ | Valeur |
|---|---|
| **ID WSTG** | WSTG-CRYP-04 |
| **CWE** | CWE-327 |
| **Statut du test** | Non effectué |
| **Sévérité** | Haute |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/04-Testing_for_Weak_Cryptographic_Primitives  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `hashcat`, `John the Ripper`, `Burp Suite`, `CyberChef`, `OpenSSL`, `nmap (ssl-enum-ciphers)`

### Des algorithmes de hachage dépréciés tels que MD5 ou SHA-1 sont-ils utilisés pour le stockage de données sensibles ?
- [ ] Non — l'application utilise des algorithmes modernes résistants aux collisions tels que Argon2 ou Bcrypt  
- [ ] Oui — des algorithmes dépréciés sont utilisés mais **uniquement** pour des vérifications d'intégrité non sensibles à la sécurité  
- [ ] Oui — des algorithmes dépréciés **sont utilisés** pour des mots de passe ou des jetons sensibles *(Haute)*  

### Des algorithmes de chiffrement symétrique faibles comme DES, 3DES ou RC4 sont-ils actuellement employés ?
- [ ] Non — AES (128 bits ou plus) dans un mode sécurisé tel que GCM ou CBC est utilisé  
- [ ] Oui — des algorithmes hérités (legacy) sont **activés** pour la compatibilité descendante mais ne sont **pas** ceux par défaut  
- [ ] Oui — des algorithmes hérités sont la méthode principale pour protéger les données au repos ou en transit  

### L'application implémente-t-elle une logique cryptographique "maison" (roll-your-own) ou non standard ?
- [ ] Non — l'application adhère strictement à des bibliothèques cryptographiques standards et examinées par les pairs  
- [ ] Oui — des wrappers personnalisés existent mais les primitives sous-jacentes **sont** des standards de l'industrie  
- [ ] Oui — des algorithmes cryptographiques propriétaires ou une logique personnalisée **sont appliqués** aux données sensibles *(Critique)*  

### Les sels (salts) cryptographiques et les vecteurs d'initialisation (IV) sont-ils gérés selon les meilleures pratiques ?
- [ ] Oui — les sels sont uniques par utilisateur et les IV sont imprévisibles pour chaque opération  
- [ ] Oui — des sels sont utilisés mais ne sont **pas** uniques pour l'ensemble des enregistrements  
- [ ] Non — les sels sont statiques, codés en dur ou absents, et les IV **sont réutilisés** sur plusieurs sessions  

### La longueur de bit des clés asymétriques (RSA, Diffie-Hellman) est-elle suffisante selon les standards modernes ?
- [ ] Oui — les clés RSA/DH font 2048 bits ou plus, ou l'Elliptic Curve (ECC) est utilisée  
- [ ] Non — les clés font moins de 2048 bits mais le contournement n'est **pas** trivial  
- [ ] Non — les clés font 1024 bits ou moins, ce qui les rend **vulnérables** à la factorisation ou à des attaques computationnelles  

---