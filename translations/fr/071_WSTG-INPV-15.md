## WSTG-INPV-15 — HTTP Splitting Smuggling

Les vulnérabilités de type HTTP Splitting et Smuggling surviennent suite à des divergences dans la manière dont les proxys frontend et les serveurs backend interprètent et traitent les limites des requêtes HTTP, plus particulièrement en ce qui concerne les en-têtes `Content-Length` et `Transfer-Encoding`. En concevant des requêtes ambiguës, un attaquant peut « smuggler » une requête cachée vers le backend ou injecter des séquences CRLF pour diviser une réponse, ce qui entraîne un Cache Poisoning, un Request Hijacking ou un contournement des contrôles de sécurité. Ces failles se manifestent généralement dans des environnements complexes utilisant des reverse proxies, des load balancers ou des CDNs qui possèdent des logiques de parsing incohérentes. Du point de vue d'un attaquant, cela permet la redirection du trafic utilisateur, le vol de jetons de session sensibles et l'exécution d'actions non autorisées dans le contexte des sessions d'autres utilisateurs.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INPV-15 |
| **CWE** | CWE-444 |
| **État du test** | Non effectué |
| **Sévérité** | Élevée / Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/15-Testing_for_HTTP_Response_Splitting  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  

**Outils :** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `smuggler.py`, `curl`

### L'environnement est-il sensible au Request Smuggling via des divergences CL.TE ou TE.CL ?
- [ ] Non — les serveurs frontend et backend gèrent les limites de requêtes de manière cohérente  
- [ ] Oui — des divergences existent mais l'exploitation n'est **pas possible** en raison des mesures d'atténuation de l'infrastructure  
- [ ] Oui — le smuggling CL.TE ou TE.CL **est possible**, permettant à des requêtes cachées d'atteindre le backend *(Élevée)*  
- [ ] Oui — le TE.TE (double encodage/obfuscation) **est possible** pour contourner les filtres frontend *(Critique)*  

### Le HTTP Response Splitting peut-il être réalisé via une injection CRLF dans les en-têtes ?
- [ ] Non — l'application assainit correctement les séquences CRLF dans toutes les entrées d'en-tête  
- [ ] Oui — les séquences CRLF sont réfléchies dans les en-têtes mais le Response Splitting n'est **pas possible**  
- [ ] Oui — l'injection CRLF **est possible**, permettant l'injection d'en-tête ou le Cache Poisoning  

### Les contrôles de sécurité (WAF/ACLs) sont-ils contournés à l'aide de requêtes « smuggled » ?
- [ ] Non — les contrôles de sécurité s'appliquent à la fois aux requêtes externes et aux requêtes passées en fraude  
- [ ] Oui — les requêtes passées en fraude **peuvent** contourner les règles WAF du frontend ou les ACL basées sur l'IP  

### Est-il possible de détourner les sessions d'autres utilisateurs ou de rediriger leur trafic ?
- [ ] Non — les flux de requête/réponse sont isolés et ne peuvent pas être croisés  
- [ ] Oui — le Request Smuggling **est possible** et permet de capturer les requêtes d'autres utilisateurs *(Critique)*  
- [ ] Oui — le Response Splitting **est possible** et permet un Cache Poisoning côté navigateur ou une XSS  

---