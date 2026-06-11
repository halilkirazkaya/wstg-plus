## WSTG-INFO-05 — Examen du contenu des pages web pour détecter les fuites d'informations

L'examen du contenu des pages web implique l'inspection manuelle et automatisée des réponses de l'application, y compris les fichiers HTML, JavaScript et CSS, afin d'identifier les informations sensibles exposées involontairement aux utilisateurs. Les attaquants scrutent le code source côté client à la recherche de commentaires de développeurs, de champs de formulaire masqués, de chemins de serveurs internes, de clés API codées en dur (hardcoded) et d'informations de débogage (debugging) pouvant faciliter les phases d'exploitation ultérieures. Cette fuite d'informations (Information Leakage) survient souvent lorsque les développeurs oublient de supprimer les métadonnées ou le code de débogage avant le passage en production, révélant potentiellement des failles logiques ou des détails de l'infrastructure backend. Du point de vue d'un attaquant, cela constitue une méthode nécessitant peu d'efforts pour cartographier la structure interne de l'application et découvrir des points d'entrée négligés sans déclencher d'alertes de sécurité ni interagir avec la logique backend du serveur.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INFO-05 |
| **CWE** | CWE-200 |
| **État du test** | Non réalisé |
| **Sévérité** | Faible / Moyenne* |

> *La sévérité devient Élevée si des identifiants (credentials) codés en dur, des clés privées ou des variables d'environnement internes sensibles sont découverts.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/05-Review_Web_Page_Content_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**Outils :** `Burp Suite (Target/Proxy)`, `Browser Developer Tools`, `SecretFinder`, `grep`, `truffleHog`, `LinkFinder`

### Des commentaires de développeurs contenant des informations sensibles sont-ils présents dans le code source ?
- [ ] Non — aucun commentaire ou seulement des commentaires génériques et non sensibles trouvés  
- [ ] Oui — des commentaires existent mais ne contiennent aucune logique ou donnée sensible  
- [ ] Oui — les commentaires révèlent des chemins internes, des requêtes SQL ou des instructions administratives  
- [ ] Oui — les commentaires révèlent des identifiants (credentials) ou des notes de développeurs sensibles *(Élevée)*  

### Les champs de saisie HTML masqués contiennent-ils des données sensibles ou des informations d'état ?
- [ ] Non — aucun champ masqué trouvé ou ils ne contiennent que des jetons (tokens) non sensibles  
- [ ] Oui — des champs masqués sont utilisés pour l'état mais sont chiffrés ou signés  
- [ ] Oui — les champs masqués contiennent en clair (plaintext) des mots de passe, des rôles d'utilisateur ou des identifiants internes  

### Des secrets codés en dur, des clés API ou des adresses IP internes sont-ils présents dans les fichiers JavaScript ?
- [ ] Non — aucune chaîne sensible ou secret trouvé dans les fichiers JS  
- [ ] Oui — les fichiers JS contiennent des clés API publiques avec des restrictions appropriées  
- [ ] Oui — les fichiers JS contiennent des clés API non protégées, des adresses IP internes ou des points de terminaison (endpoints) privés  
- [ ] Oui — les fichiers JS contiennent des identifiants administratifs ou des clés privées codés en dur *(Critique)*  

### L'application révèle-t-elle des versions de framework ou des chemins de fichiers internes dans la source ?
- [ ] Non — les informations sur le framework et les chemins de fichiers sont minifiés ou obfusqués  
- [ ] Oui — les noms et versions de frameworks sont visibles mais à jour (patchés)  
- [ ] Oui — des chemins de fichiers internes ou des chemins de serveurs absolus sont exposés dans le code source  

### Le code source est-il sujet aux fuites en raison d'un manque de minification ou d'obfuscation ?
- [ ] Non — le code de production est entièrement minifié et expurgé des commentaires  
- [ ] Oui — le code est minifié mais les commentaires subsistent dans la source  
- [ ] Oui — les fichiers source maps (`.map`) sont accessibles publiquement, permettant une reconstruction complète du code source  

---