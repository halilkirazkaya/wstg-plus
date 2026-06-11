## WSTG-INPV-05 — SQL Injection

L'Injection SQL (SQL Injection) se produit lorsque des entrées fournies par l'utilisateur sont incorporées dans des requêtes SQL sans paramétrage ou assainissement adéquat, permettant aux attaquants de manipuler la logique de la requête. Une exploitation réussie peut entraîner une exfiltration de données non autorisée, un contournement de l'authentification (Authentication Bypass), une modification de données et, dans certains cas, une exécution de code à distance (Remote Code Execution) via des fonctionnalités de base de données telles que `xp_cmdshell` ou `LOAD_FILE()`. Cette vulnérabilité affecte couramment les formulaires de connexion, les fonctionnalités de recherche, les paramètres d'API REST, les en-têtes HTTP et tout point de terminaison (endpoint) construisant des requêtes SQL dynamiques à partir d'entrées contrôlées par l'utilisateur. Du point de vue d'un attaquant, l'identification d'un point d'injection est la porte d'entrée principale vers une compromission complète de la base de données et un mouvement latéral potentiel au sein de l'infrastructure.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INPV-05 |
| **CWE** | CWE-89 |
| **Statut du test** | Non effectué |
| **Sévérité** | Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/05-Testing_for_SQL_Injection  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html  
* https://portswigger.net/web-security/sql-injection  
* https://portswigger.net/web-security/nosql-injection  

**Outils :** `sqlmap`, `Burp Suite (Intruder/Repeater)`, `Ghauri`, `jSQL Injection`, `BBQSQL`

### Les paramètres contrôlables par l'utilisateur sont-ils traités à l'aide de méthodes d'accès aux bases de données sécurisées ?
- [ ] Oui — l'application utilise exclusivement un ORM ou des requêtes paramétrées et un contournement n'est **pas possible**  
- [ ] Oui — des requêtes paramétrées sont utilisées mais un contournement **est possible** via des cas limites (ex : clauses `ORDER BY`)  
- [ ] Non — la concaténation de chaînes brutes est utilisée pour construire des requêtes SQL **sans** paramétrage  

### Le mécanisme d'authentification peut-il être contourné via une injection SQL ?
- [ ] Non — les paramètres de connexion ne sont **pas** vulnérables à l'injection  
- [ ] Oui — un contournement de l'authentification **est possible** à l'aide de payloads basés sur des tautologies (ex : `' OR 1=1 --`)  

### L'exfiltration de données est-elle possible via des techniques en bande (in-band) ou hors bande (out-of-band) ?
- [ ] Non — aucune exfiltration de données identifiée  
- [ ] Oui — une exfiltration basée sur UNION ou sur des erreurs (error-based) **est possible**  
- [ ] Oui — une exfiltration aveugle (Blind), qu'elle soit basée sur le booléen (Boolean-based) ou sur le temps (Time-based), **est possible**  
- [ ] Oui — une exfiltration hors bande (Out-of-band, DNS/HTTP) **est possible**  

### Les fonctions de l'application présentent-elles des vulnérabilités d'injection SQL de second ordre (Second-order SQL injection) ?
- [ ] Non — les données stockées sont manipulées de manière sécurisée lorsqu'elles sont récupérées pour des requêtes ultérieures  
- [ ] Oui — une entrée utilisateur est stockée puis utilisée ultérieurement dans une requête SQL non sécurisée **sans** validation  

### Les privilèges du compte de service de la base de données sont-ils restreints au minimum requis ?
- [ ] Oui — le compte de service dispose du **moindre privilège** (ex : limité à des tables/actions spécifiques)  
- [ ] Non — le compte de service dispose de privilèges élevés (ex : `DROP TABLE`, privilèges `FILE`, ou `xp_cmdshell` **activé**)  

---