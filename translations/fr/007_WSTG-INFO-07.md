## WSTG-INFO-07 — Cartographier les chemins d'exécution au travers de l'application

La cartographie des chemins d'exécution implique l'identification systématique de tous les points de terminaison (endpoints) accessibles, des flux fonctionnels et des branches de prise de décision au sein d'une application web. Ce processus est essentiel pour garantir qu'aucune fonctionnalité "fantôme" — telle que du code hérité (legacy code), des interfaces de débogage (debug) ou des routes d'API non documentées — ne reste cachée à l'examen de sécurité, car celles-ci manquent souvent de contrôles de sécurité modernes. Les attaquants utilisent une combinaison de spidering automatisé et d'exploration manuelle pour visualiser la structure de l'application, découvrant souvent des vulnérabilités telles que l'accès administratif non autorisé ou des failles de logique métier (business logic flaws) au cours du processus. En corrélant les entrées aux réponses spécifiques du serveur, les testeurs peuvent identifier la logique de traitement sensible qui nécessite des tests approfondis ciblés pour assurer une couverture robuste.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INFO-07 |
| **CWE** | CWE-200 |
| **Statut du test** | Non réalisé |
| **Sévérité** | Informationnel |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/07-Map_Execution_Paths_Through_Application  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite Professional`, `OWASP ZAP`, `Katana`, `FFUF`, `LinkFinder`, `GoBuster`, `ParamMiner`

### L'application a-t-elle été entièrement indexée à l'aide d'un crawling automatisé et manuel ?
- [ ] Oui — une carte complète de toutes les ressources visibles et liées **est terminée**  
- [ ] Oui — le crawling automatisé **est terminé** mais l'exploration manuelle est en attente  
- [ ] Non — l'application n'a **pas** été indexée  

### Les endpoints cachés ou non documentés sont-ils identifiés via l'analyse JS ou le brute-forcing de répertoires ?
- [ ] Non — aucun chemin non documenté découvert via l'analyse par canaux auxiliaires (side-channel analysis)  
- [ ] Oui — des chemins cachés ont été découverts mais ils **ne peuvent pas** être consultés sans autorisation  
- [ ] Oui — des endpoints non documentés **sont** accessibles et fournissent des fonctionnalités sensibles *(Critique / Élevé)*  

### Les chemins d'exécution peuvent-ils être modifiés via des paramètres ou des en-têtes non standard ?
- [ ] Non — les paramètres tels que `debug`, `test` ou `admin` n'affectent **pas** le flux d'exécution  
- [ ] Oui — les paramètres modifient la sortie mais **ne contournent pas** les contrôles de sécurité  
- [ ] Oui — des paramètres modifiant le chemin **sont appliqués** et permettent de contourner la logique prévue  

### Le flux de la logique métier multi-étapes (ex : authentification multi-facteurs, paiement) est-il entièrement cartographié ?
- [ ] Oui — tous les états et transitions dans les processus multi-étapes **sont identifiés**  
- [ ] Non — les transitions complexes de la machine à états (state machine) **ne peuvent pas** être entièrement cartographiées  

### Les fichiers de documentation d'API (Swagger/OpenAPI) ou les cartes côté client (Source Maps) sont-ils exposés ?
- [ ] Non — aucune carte d'architecture ou fichier de documentation n'est exposé publiquement  
- [ ] Oui — la documentation est présente mais **est protégée** par une authentification  
- [ ] Oui — Swagger UI, OpenAPI JSON ou les JS Source Maps **sont activés** et accessibles publiquement  

---