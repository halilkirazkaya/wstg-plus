## WSTG-ERRH-01 — Test de la gestion incorrecte des erreurs

Une gestion incorrecte des erreurs se produit lorsqu'une application web divulgue des détails techniques sensibles—tels que des traces de pile d'exécution (stack traces), des informations sur le schéma de la base de données ou des chemins de fichiers internes—à travers ses réponses d'erreur. Les attaquants déclenchent ces erreurs en soumettant des entrées malformées, en accédant à des ressources inexistantes ou en provoquant des exceptions côté serveur afin de cartographier l'architecture sous-jacente de l'application et d'identifier des vecteurs potentiels pour une exploitation ultérieure. Cette fuite d'informations sert souvent de précurseur à des attaques plus graves, notamment l'SQL Injection ou le Path Traversal, en fournissant à l'attaquant des spécifications précises sur l'environnement. Une implémentation sécurisée doit fournir des messages génériques conviviaux pour l'utilisateur tout en capturant des diagnostics détaillés uniquement dans des journaux (logs) sécurisés côté serveur.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-ERRH-01 |
| **CWE** | CWE-209 |
| **État du Test** | Non effectué |
| **Sévérité** | Basse / Moyenne |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/01-Testing_For_Improper_Error_Handling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite (Repeater/Intruder)`, `ffuf`, `wfuzz`, `Arjun`, `curl`

### L'application utilise-t-elle un gestionnaire d'erreurs global et générique pour les exceptions non gérées ?
- [ ] Oui — des pages d'erreur génériques sont **activées** et ne révèlent **aucun** détail technique  
- [ ] Oui — des pages d'erreur personnalisées sont utilisées mais une fuite **est possible** via des en-têtes spécifiques  
- [ ] Non — les pages d'erreur par défaut du serveur (ex: Tomcat, IIS, Nginx) sont **visibles**  

### Des informations techniques peuvent-elles être énumérées via des entrées malformées ou des cas limites ?
- [ ] Non — les erreurs sont gérées de manière appropriée avec des identifiants de référence uniques pour la journalisation  
- [ ] Oui — des traces de pile (stack traces) ou des requêtes de base de données **sont divulguées** dans les réponses  
- [ ] Oui — des chemins de fichiers internes, des variables d'environnement ou des chaînes de version de serveur **sont divulgués**  

### Les réponses d'API divulguent-elles des objets d'erreur verbeux ou des informations de débogage ?
- [ ] Non — les API renvoient des codes d'erreur standardisés et des messages JSON assainis  
- [ ] Oui — les API renvoient des objets de débogage verbeux ou des détails complets sur l'exception dans le corps de la réponse  
- [ ] Oui — des traces de pile (stack traces) sont incluses dans les champs `details` ou `exception` de la réponse JSON  

### L'application se comporte-t-elle différemment (basé sur le temps ou le contenu) lorsque des erreurs surviennent ?
- [ ] Non — les signatures de réponse et les délais sont cohérents quel que soit le type d'erreur  
- [ ] Oui — des réponses différenciées **peuvent** être utilisées pour énumérer des états valides vs invalides (ex: énumération d'utilisateurs / User Enumeration)  

---