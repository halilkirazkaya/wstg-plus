## WSTG-CLNT-12 — Test Browser Storage

Le test du stockage du navigateur consiste à analyser comment une application utilise les mécanismes côté client tels que le LocalStorage, SessionStorage, IndexedDB et WebSQL pour la persistance des données. Les informations sensibles stockées de manière non sécurisée — y compris les PII, les jetons d'authentification ou les identifiants de session — peuvent être compromises si un attaquant parvient à exploiter une vulnérabilité de Cross-Site Scripting (XSS) ou obtient un accès physique à l'appareil de l'utilisateur. Les Pentesters doivent déterminer si les données sont stockées en clair, si elles restent accessibles après la déconnexion et si le cycle de vie du stockage est géré de manière appropriée pour prévenir les fuites de données. Du point de vue d'un attaquant, ces mécanismes de stockage sont des cibles lucratives pour l'exfiltration d'informations de maintien d'état permettant le détournement de session (session hijacking) ou une persistance de compte à long terme.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CLNT-12 |
| **CWE** | CWE-922 |
| **État du test** | Non effectué |
| **Sévérité** | Moyenne / Haute* |

> *La sévérité devient Haute si des jetons de session ou des PII sensibles sont stockés dans le LocalStorage et sont accessibles via XSS.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/12-Testing_Browser_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Browser DevTools`, `Storage Explorer`, `Burp Suite`, `OWASP ZAP`

### L'application stocke-t-elle des informations sensibles dans le stockage du navigateur ?
- [ ] Non — l'application n'utilise pas le stockage du navigateur ou ne stocke que l'état de l'interface utilisateur (UI) non sensible  
- [ ] Oui — les données sensibles sont stockées mais sont chiffrées en utilisant une cryptographie côté client robuste  
- [ ] Oui — des données sensibles (jetons, PII, secrets) sont stockées en clair *(Moyenne)*  

### Les données stockées sont-elles accessibles à des scripts non autorisés (risque XSS) ?
- [ ] Non — les données sont stockées dans des cookies HttpOnly (pas dans le stockage du navigateur) ou le stockage n'est pas utilisé  
- [ ] Oui — LocalStorage/SessionStorage est utilisé, rendant les données complètement accessibles via n'importe quel Payload XSS *(Haute)*  

### Le stockage du navigateur est-il correctement vidé lors de la déconnexion de l'utilisateur ?
- [ ] Oui — tout le stockage lié à l'application est explicitement vidé pendant le processus de déconnexion  
- [ ] Non — le stockage persiste après la déconnexion, mais ne contient pas de données de session sensibles  
- [ ] Non — les jetons d'authentification ou les PII persistent dans le stockage après la fin de la session *(Moyenne)*  

### L'application utilise-t-elle IndexedDB ou WebSQL pour des jeux de données volumineux ou sensibles ?
- [ ] Non — ces mécanismes de stockage ne sont pas utilisés  
- [ ] Oui — les données sont stockées et correctement assainies avant d'être rendues dans le DOM  
- [ ] Oui — des jeux de données sensibles sont stockés en clair au sein des structures IndexedDB/WebSQL  

### L'intégrité des données stockées peut-elle être manipulée pour affecter la logique de l'application ?
- [ ] Non — l'application valide ou signe les données avant de les traiter à partir du stockage  
- [ ] Oui — la modification des valeurs de stockage (ex : rôles d'utilisateur, drapeaux) est possible et entraîne une altération du comportement de l'application  

---