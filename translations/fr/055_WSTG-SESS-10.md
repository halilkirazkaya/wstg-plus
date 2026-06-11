## WSTG-SESS-10 — Testing JSON Web Tokens

Les JSON Web Tokens (JWT) sont couramment utilisés pour la gestion de session sans état (stateless) et la propagation d'identité, mais leur sécurité repose entièrement sur l'implémentation correcte des algorithmes de signature et sur une vérification stricte des claims. Les attaquants tentent fréquemment de contourner l'authentification en exploitant les failles de l'algorithme "none", en effectuant un brute-force sur des clés secrètes HS256 faibles, ou en réalisant des attaques par changement d'algorithme (algorithm-switching) où une clé publique asymétrique est utilisée comme un secret HMAC symétrique. Ces vulnérabilités se manifestent généralement dans les cookies de session ou les en-têtes Authorization, permettant potentiellement à un attaquant d'élever ses privilèges ou d'usurper l'identité de n'importe quel utilisateur en modifiant des claims tels que `role` ou `user_id` sans invalider l'intégrité cryptographique du token.


| Champ | Valeur |
|---|---|
| **ID WSTG** | WSTG-SESS-10 |
| **CWE** | CWE-347 |
| **État du test** | Non effectué |
| **Sévérité** | Élevée / Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/10-Testing_JSON_Web_Tokens  
* https://hacktricks.wiki/en/pentesting-web/hacking-jwt-json-web-tokens.html  
* https://portswigger.net/web-security/jwt  

**Outils :** `jwt_tool`, `Burp Suite (JWT Editor Extension)`, `hashcat`, `john`, `jose`

### L'algorithme "none" est-il accepté par le serveur ?
- [ ] Non — le serveur **rejette** les tokens utilisant `alg: none` dans l'en-tête  
- [ ] Oui — le serveur **accepte** les tokens avec `alg: none` et une signature vide *(Critique)*  

### Comment la signature JWT est-elle vérifiée et protégée contre le brute-force ?
- [ ] Oui — des clés asymétriques fortes (RS256/ES256) ou des secrets HMAC à haute entropie sont utilisés  
- [ ] Oui — HS256 est utilisé mais le secret **peut** être cassé par brute-force en raison d'une faible entropie ou de valeurs par défaut  
- [ ] No — la vérification de la signature **n'est pas appliquée** ou **peut** être contournée via un changement d'algorithme (de RS256 à HS256)  

### Les claims standard (exp, nbf, iat) sont-ils strictement appliqués par le backend ?
- [ ] Oui — le claim `exp` (expiration) est présent et **ne peut pas** être contourné  
- [ ] Oui — `exp` est présent mais le serveur **ne l'applique pas** lors de la validation  
- [ ] Non — les claims d'expiration sont **manquants** ou ignorés, permettant un rejeu de session indéfini  

### L'implémentation empêche-t-elle l'injection de clés via les en-têtes (kid, jku, x5u) ?
- [ ] Oui — les en-têtes sont assainis et seules les clés/sources internes de confiance sont **autorisées**  
- [ ] Non — l'en-tête `kid` (Key ID) est vulnérable à une traversée de répertoire (directory traversal) ou à une injection SQL pour pointer vers un fichier connu  
- [ ] Non — les en-têtes `jku` ou `x5u` **peuvent** pointer vers des URL contrôlées par l'attaquant pour charger des clés malveillantes  

---