## WSTG-SESS-04 — Test de l'exposition des variables de session

L'exposition des variables de session survient lorsque des identifiants de session sensibles ou des données relatives à l'état sont transmis via des canaux non sécurisés tels que les chaînes de requête (query strings) d'URL, les en-têtes Referer ou les journaux système. Cette exposition augmente considérablement le risque de détournement de session (session hijacking), car les identifiants peuvent être mis en cache dans l'historique du navigateur, enregistrés par des proxys intermédiaires ou divulgués à des sites tiers via l'en-tête Referer. Les Pentesters identifient ces expositions en surveillant toutes les requêtes sortantes et en inspectant les journaux de l'application ou les configurations du serveur pour détecter d'éventuelles fuites de données par inadvertance. Dans le monde réel, les attaquants exploitent ces fuites en collectant des jetons de session (session tokens) valides à partir de machines partagées ou en analysant les modèles de trafic pour contourner l'authentification et obtenir un accès non autorisé aux comptes d'utilisateurs.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-SESS-04 |
| **CWE** | CWE-598 |
| **État du test** | Non réalisé |
| **Sévérité** | Moyenne / Haute* |

> *La sévérité devient Haute si la variable exposée est un jeton de session permettant une prise de contrôle immédiate du compte (account takeover).

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/04-Testing_for_Exposed_Session_Variables  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Outils :** `Burp Suite (Proxy/Logger)`, `OWASP ZAP`, `Browser DevTools`, `grep`

### L'identifiant de session est-il transmis dans la chaîne de requête (query string) de l'URL ?
- [ ] Non — les identifiants de session **ne sont pas** présents dans la chaîne de requête de l'URL  
- [ ] Oui — les identifiants sont présents mais la session est de courte durée ou présente un risque faible  
- [ ] Oui — des IDs de session uniques **sont** présents dans la chaîne de requête de l'URL *(Critique)*  

### L'application divulgue-t-elle des jetons de session via l'en-tête Referer ?
- [ ] Non — la politique `Referrer-Policy` empêche la fuite ou aucun lien vers des tiers n'existe  
- [ ] Oui — les jetons de session (session tokens) **sont** transmis à des domaines tiers via l'en-tête Referer  

### Les variables de session sont-elles stockées dans les journaux (logs) du serveur ou de l'application ?
- [ ] Non — les variables de session sont masquées ou exclues des journaux  
- [ ] Oui — les variables de session **sont** enregistrées en clair dans les journaux de l'application ou du serveur web  

### L'application utilise-t-elle des requêtes GET pour des opérations modifiant l'état ?
- [ ] Non — l'application utilise `POST` ou d'autres méthodes non-idempotentes pour les opérations sensibles  
- [ ] Oui — `GET` est utilisé, provoquant la mise en cache des données de session dans l'historique du navigateur et les proxys intermédiaires  

### L'en-tête `Cache-Control` est-il configuré pour empêcher la mise en cache des données relatives à la session ?
- [ ] Oui — `Cache-Control: no-store` (ou similaire) **est appliqué** aux réponses sensibles  
- [ ] Oui — des contrôles sont en place mais un contournement (bypass) **est possible** via des cas particuliers du navigateur  
- [ ] Non — les réponses sensibles contenant des données de session **peuvent** être mises en cache par le navigateur  

---