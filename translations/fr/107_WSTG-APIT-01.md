## WSTG-APIT-01 — Reconnaissance d'API

La reconnaissance d'API est le processus systématique consistant à identifier et cartographier la surface d'attaque de l'API afin de découvrir les points de terminaison (endpoints), les méthodes supportées et les structures de données sous-jacentes. Les attaquants réalisent cette phase pour découvrir des API non documentées (shadow APIs), des versions obsolètes présentant des vulnérabilités héritées, et des fichiers de documentation exposés tels que les définitions Swagger ou OpenAPI qui révèlent la logique métier interne. En analysant les modèles d'URL prévisibles, les fichiers JavaScript côté client et les résultats du brute-force de répertoires, un adversaire peut construire une cartographie complète de l'API pour identifier des cibles pour des attaques de type Broken Object Level Authorization (BOLA) ou Mass Assignment. Cette reconnaissance cible généralement des chemins communs tels que `/api/v1/`, `/swagger.json` ou `/graphql` pour obtenir une feuille de route des fonctionnalités backend de l'application.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-APIT-01 |
| **CWE** | CWE-200 |
| **État du test** | Non effectué |
| **Sévérité** | Informationnel / Faible |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/01-API_Reconnaissance  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/api-testing  

**Outils :** `Kiterunner`, `ffuf`, `Arjun`, `Burp Suite (Logger++)`, `Postman`, `Swagger-ez`

### Les fichiers de documentation d'API (Swagger, OpenAPI, WSDL) sont-ils accessibles publiquement ?
- [ ] Non — La documentation de l'API **n'est pas** accessible ou est restreinte par une authentification  
- [ ] Oui — La documentation est accessible mais nécessite des identifiants valides  
- [ ] Oui — La documentation de l'API (par exemple, `/swagger-ui.html`) est **accessible publiquement** sans authentification  

### Des points de terminaison d'API non documentés ou « shadow » peuvent-ils être découverts via fuzzing ?
- [ ] Non — Le fuzzing de répertoires et de points de terminaison n'a révélé aucun chemin non documenté  
- [ ] Oui — Des points de terminaison non documentés ont été trouvés mais ils **ne peuvent pas** être accédés sans autorisation  
- [ ] Oui — Des points de terminaison non documentés ont été trouvés et **peuvent** être accédés sans autorisation valide  

### L'application révèle-t-elle plusieurs versions d'API (par exemple, /v1/, /v2/, /beta/) ?
- [ ] Non — Seule la version actuelle et sécurisée de l'API est accessible  
- [ ] Oui — Des versions héritées existent mais implémentent les **mêmes** contrôles de sécurité que la version actuelle  
- [ ] Oui — Des versions héritées (par exemple, `/v1/`) sont accessibles et **manquent** de contrôles de sécurité par rapport aux versions plus récentes  

### Les ressources côté client (JavaScript / Applications mobiles) divulguent-elles des structures de points de terminaison d'API ou des clés ?
- [ ] Non — Le code côté client ne contient **pas** de chemins d'API ou de clés sensibles codés en dur  
- [ ] Oui — Le code côté client contient des cartographies de points de terminaison d'API mais **aucune** clé sensible  
- [ ] Oui — Des clés d'API, des secrets ou des URL de points de terminaison internes **sont** codés en dur dans les ressources côté client  

### Des paramètres ou des en-têtes cachés sont-ils découvrables sur les points de terminaison connus ?
- [ ] Non — Le fuzzing de paramètres (par exemple, via `Arjun`) n'a **pas** révélé d'entrées cachées  
- [ ] Oui — Des paramètres cachés (par exemple, `debug=true`, `admin=1`) ont été découverts mais ne sont **pas** fonctionnels  
- [ ] Oui — Des paramètres cachés ont été découverts et **peuvent** être utilisés pour modifier le comportement de l'application  

---