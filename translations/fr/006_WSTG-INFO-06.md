## WSTG-INFO-06 — Identification des points d'entrée de l'application

L'identification des points d'entrée de l'application consiste à cartographier l'intégralité de la surface d'attaque en énumérant toutes les manières possibles pour un utilisateur ou un système externe d'interagir avec l'application. Ce processus inclut la découverte de toutes les URL, des points de terminaison API (API endpoints), des paramètres (GET, POST, basés sur les cookies), des en-têtes HTTP (HTTP headers) et des protocoles spécialisés comme WebSockets ou gRPC. Pour un testeur de pénétration, cette étape est vitale car les vulnérabilités sont souvent trouvées dans des API "fantômes" (shadow APIs) non documentées, des points de terminaison hérités (legacy endpoints) ou des structures de paramètres complexes qui ont été négligées lors du développement. Une énumération approfondie garantit qu'aucun composant de l'application ne reste une boîte noire non testée, empêchant ainsi les attaquants de trouver des chemins non surveillés vers le système.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INFO-06 |
| **CWE** | CWE-1059 |
| **Statut du test** | Non effectué |
| **Sévérité** | Informationnel |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/06-Identify_Application_Entry_Points  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite (Target/Proxy)`, `OWASP ZAP`, `ffuf`, `Arjun`, `LinkFinder`, `GoSpider`, `KiteRunner`

### Est-ce que tous les paramètres GET et POST ont été énumérés à travers l'application ?
- [ ] Oui — une énumération exhaustive via un crawling automatisé et manuel **est terminée**  
- [ ] Oui — seuls les paramètres communs ont été identifiés via un crawling standard  
- [ ] Non — les paramètres sont en grande partie **non identifiés** ou non testés  

### Les paramètres non documentés ou cachés (ex: debug, admin) sont-ils recherchés par Fuzzing ?
- [ ] Non — la découverte de paramètres cachés n'est **pas nécessaire** pour ce périmètre spécifique  
- [ ] Oui — le Fuzzing de paramètres (ex: via `Arjun`) **a été effectué** sans résultat  
- [ ] Oui — des paramètres cachés **ont été découverts** et cartographiés pour des tests ultérieurs  
- [ ] Non — la découverte de paramètres cachés **n'a pas** été effectuée  

### Toutes les méthodes HTTP prises en charge (PUT, DELETE, PATCH, etc.) ont-elles été identifiées pour chaque point de terminaison ?
- [ ] Oui — la découverte de méthodes **est appliquée** sur tous les points de terminaison découverts  
- [ ] Oui — la découverte de méthodes **est appliquée** uniquement aux points de terminaison sensibles ou à fort intérêt  
- [ ] Non — seules les méthodes standard GET et POST **sont identifiées**  

### Les points d'entrée non standard tels que WebSockets, gRPC ou les en-têtes personnalisés sont-ils cartographiés ?
- [ ] Non — l'application **n'utilise pas** de protocoles non standard ou d'en-têtes personnalisés  
- [ ] Oui — tous les points d'entrée spécifiques aux protocoles et les en-têtes personnalisés **sont identifiés**  
- [ ] Non — des points d'entrée non standard **existent** mais n'ont pas été cartographiés  

### La surface de l'API (y compris les points de terminaison versionnés comme /v1/ ou /v2/) a-t-elle été entièrement cartographiée ?
- [ ] Non — l'application **n'expose pas** d'API  
- [ ] Oui — toutes les versions d'API et tous les points de terminaison **sont identifiés** et documentés  
- [ ] Oui — seule la version actuelle de l'API en production **est identifiée**  
- [ ] Non — les points de terminaison de l'API **restent non cartographiés** ou seulement partiellement découverts  

---