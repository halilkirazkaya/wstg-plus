## WSTG-CONF-13 — Path Confusion

La confusion de chemin (Path Confusion) provient de divergences dans la manière dont les différents composants Web, tels que les proxys inverses (reverse proxies), les répartiteurs de charge (load balancers) et les serveurs d'application backend, analysent et interprètent les chemins URL. Les attaquants exploitent ces incohérences en injectant des caractères spécifiques tels que des points-virgules, des slashes encodés ou des segments de points (dot-segments) pour tromper les filtres de sécurité et accéder à des points de terminaison (endpoints) restreints ou à des ressources statiques. Cette vulnérabilité peut entraîner des défaillances de sécurité critiques, notamment le contournement d'authentification (authentication bypass), l'accès non autorisé aux données et l'empoisonnement du cache Web (Web Cache Poisoning), car le frontal (front-end) peut appliquer des règles de sécurité à un chemin que le backend interprète différemment. Une exploitation réussie se produit généralement à la frontière architecturale où le routage des requêtes et la logique de normalisation divergent à travers la pile technologique (tech stack).


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CONF-13 |
| **CWE** | CWE-444 |
| **Statut du Test** | Non réalisé |
| **Sévérité** | Moyenne / Élevée* |

> *La sévérité devient Élevée si la confusion de chemin entraîne un contournement d'authentification ou du contrôle d'accès pour des points de terminaison administratifs sensibles.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/13-Test_for_Path_Confusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite`, `Dirsearch`, `FFUF`, `ParamMiner`, `Arjun`

### Les différentes couches architecturales interprètent-elles les délimiteurs de chemin (ex : `;`, `#`, `?`) de manière cohérente ?
- [ ] Oui — toutes les couches normalisent et interprètent les délimiteurs de chemin de manière identique  
- [ ] Non — des incohérences existent mais **ne peuvent pas** être utilisées pour accéder à des ressources restreintes  
- [ ] Non — les divergences permettent à un attaquant de contourner les filtres front-end et d'atteindre la logique backend **est possible**  

### Les contrôles d'accès peuvent-ils être contournés à l'aide de séquences de traversée de chemin (path traversal) ou de caractères encodés ?
- [ ] Non — la normalisation est appliquée de manière cohérente avant les contrôles de sécurité  
- [ ] Oui — le contournement est **possible** via des séquences de segments de points (ex : `/admin/..;/`)  
- [ ] Oui — le contournement est **possible** via des caractères encodés en URL (ex : `%2f`, `%2e%2e%2f`)  

### L'application est-elle vulnérable à l'empoisonnement du cache Web (Web Cache Poisoning) via la confusion de chemin ?
- [ ] Non — la logique de mise en cache n'est pas affectée par l'ambiguïté du chemin ou les informations de chemin supplémentaires  
- [ ] Oui — du contenu malveillant **peut** être mis en cache pour des chemins légitimes en raison de divergences de mappage  
- [ ] Oui — des informations sensibles **peuvent** être mises en cache dans des répertoires publics via des techniques `RCD` (Relative Path Overwrite)  

### Le serveur gère-t-il les « informations de chemin supplémentaires » (PathInfo) de manière sécurisée ?
- [ ] Non — la fonctionnalité est **désactivée** ou n'existe pas  
- [ ] Oui — `PathInfo` est géré et n'interfère **pas** avec les filtres de sécurité  
- [ ] Non — `PathInfo` permet à un attaquant d'ajouter des données qui confondent la logique de routage de l'application  

---