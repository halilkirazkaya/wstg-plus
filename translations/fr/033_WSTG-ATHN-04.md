## WSTG-ATHN-04 — Test de contournement du schéma d'authentification

Le contournement du schéma d'authentification consiste à identifier et à exploiter des failles dans la logique applicative permettant à un attaquant d'accéder à des ressources protégées sans fournir d'identifiants valides. Cette vulnérabilité survient généralement lorsque l'application s'appuie sur des contrôles côté client, ne parvient pas à appliquer l'autorisation côté serveur pour chaque requête, ou laisse des « backdoors » administratives et des points de terminaison (endpoints) de débogage exposés en production. Les attaquants exploitent ces faiblesses en effectuant du Forced Browsing vers des URL sensibles, en manipulant des paramètres HTTP tels que `authenticated=true`, ou en falsifiant des en-têtes (Spoofing) pour tromper l'application et lui faire croire qu'une session est déjà établie. Une exploitation réussie entraîne une rupture complète du périmètre d'authentification, pouvant conduire à une exfiltration de données non autorisée ou à un compromis total du système.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-ATHN-04 |
| **CWE** | CWE-287 |
| **Statut du Test** | Non effectué |
| **Sévérité** | Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/04-Testing_for_Bypassing_Authentication_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**Outils :** `Burp Suite`, `Ffuf`, `Gobuster`, `Dirsearch`, `Postman`

### Les pages internes sensibles peuvent-elles être consultées directement sans session active ?
- [ ] Non — l'accès direct entraîne une redirection vers la page de connexion ou une réponse 403/404  
- [ ] Oui — certaines pages sensibles sont accessibles mais fournissent des données limitées ou non sensibles  
- [ ] Oui — des pages administratives ou spécifiques à l'utilisateur sensibles **sont** entièrement accessibles sans authentification *(Critique)*  

### L'application s'appuie-t-elle sur des indicateurs ou des paramètres côté client pour déterminer le statut d'authentification ?
- [ ] Non — le statut d'authentification est géré strictement via l'état de session côté serveur  
- [ ] Oui — des indicateurs côté client existent mais leur modification **ne donne pas** accès aux zones protégées  
- [ ] Oui — la modification de paramètres (ex. : `is_authenticated=true`, `user_role=admin`) **permet** le contournement de l'authentification  

### L'authentification peut-elle être contournée en falsifiant ou en manipulant des en-têtes HTTP spécifiques ?
- [ ] Non — les en-têtes tels que `X-Forwarded-For`, `Referer` ou les en-têtes personnalisés n'ont aucun impact sur l'authentification  
- [ ] Oui — la manipulation d'en-têtes (ex. : `X-Custom-IP-Authorization`, `X-Remote-User`) **est possible** et accorde un accès non autorisé  

### Existe-t-il des chemins alternatifs ou des artefacts de développement qui contournent le flux de connexion standard ?
- [ ] Non — toutes les ressources protégées sont filtrées par un contrôle d'authentification centralisé et prêt pour la production  
- [ ] Oui — des points de terminaison hérités (legacy), des routes de débogage (ex. : `/debug/login`) ou des versions d'API oubliées **sont** accessibles sans identifiants  

### L'application omet-elle de ré-authentifier ou de vérifier les sessions pour les actions à hauts privilèges ?
- [ ] Non — les actions à hauts privilèges ou sensibles nécessitent un jeton de session récent ou valide  
- [ ] Oui — une fois qu'un contournement est trouvé sur un point de terminaison, il **peut** être utilisé pour effectuer des actions administratives à travers toute l'application  

---