## WSTG-INPV-19 — Test de Server-Side Request Forgery (SSRF)

Le Server-Side Request Forgery (SSRF) survient lorsqu'un attaquant peut influencer une application web pour qu'elle effectue des requêtes non autorisées depuis le serveur vers des ressources internes ou externes. En manipulant des paramètres qui attendent des URL, des noms d'hôte ou des adresses IP, un attaquant peut contourner les contrôles au niveau du réseau tels que les pare-feu et les ACL pour sonder des segments de réseau interne, accéder à des services de métadonnées cloud (IMDS) ou interagir avec des API et des bases de données internes vulnérables. Cette vulnérabilité se manifeste généralement dans des fonctionnalités impliquant la récupération d'images, la génération de PDF, les webhooks ou les téléchargements de fichiers via URL. Du point de vue de l'attaquant, l'SSRF est une primitive puissante utilisée pour pivoter dans des environnements isolés, exfiltrer des données de configuration sensibles ou potentiellement réaliser une exécution de code à distance (Exploit RCE) via l'interaction avec des services internes comme Redis ou Memcached.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INPV-19 |
| **CWE** | CWE-918 |
| **Statut du Test** | Non réalisé |
| **Sévérité** | Élevée / Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/19-Testing_for_Server-Side_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/ssrf-server-side-request-forgery/index.html  
* https://portswigger.net/web-security/ssrf  

**Outils :** `Burp Suite (Collaborator/Interactsh)`, `Interact.sh`, `Gopherus`, `ffuf`, `cURL`, `DNSRebind Tool`

### L'application traite-t-elle des URL ou des noms d'hôte fournis par l'utilisateur ?
- [ ] Non — aucune fonctionnalité n'accepte d'URL ou de noms d'hôte externes  
- [ ] Oui — la fonctionnalité existe mais l'entrée **est strictement validée** par rapport à une liste d'autorisation (allow-list)  
- [ ] Oui — la fonctionnalité existe et accepte des URL fournies par l'utilisateur  

### Des contrôles de validation ou des listes de blocage sont-ils appliqués à la destination demandée ?
- [ ] Oui — une **liste d'autorisation** (allow-list) stricte de domaines/IP de confiance est appliquée  
- [ ] Oui — une **liste de blocage** (deny-list) est utilisée (ex: blocage de `127.0.0.1`, `169.254.169.254`)  
- [ ] Non — **aucune validation** ou filtrage n'est appliqué à l'entrée  

### Est-il possible de contourner les filtres via l'encodage, les redirections ou le DNS rebinding ?
- [ ] Non — les filtres sont robustes et gèrent les redirections et la résolution DNS de manière sécurisée  
- [ ] Oui — un contournement **est possible** en utilisant l'encodage URL ou des formats d'IP alternatifs (hexadécimal, octal)  
- [ ] Oui — un contournement **est possible** via des redirections HTTP 3xx vers des cibles internes  
- [ ] Oui — un contournement **est possible** via des attaques de DNS rebinding pour résoudre des IP internes  

### L'application peut-elle atteindre des services de métadonnées internes ou des interfaces locales ?
- [ ] Non — le réseau local et les points de terminaison de métadonnées cloud ne sont **pas accessibles**  
- [ ] Oui — l'accès à localhost (`127.0.0.1`) ou aux services de loopback **est possible**  
- [ ] Oui — l'accès aux services de métadonnées cloud (ex: AWS/GCP/Azure IMDS) **est possible** *(Critique)*  

### Quelle est la nature de la réponse SSRF ?
- [ ] Aveugle (Blind) — aucune réponse ni métadonnée n'est retournée à l'utilisateur  
- [ ] Partielle (Partial) — des métadonnées de réponse (en-têtes, taille, timing) sont retournées à l'utilisateur  
- [ ] Complète (Full) — le corps complet de la réponse de la ressource interne **est reflété** à l'utilisateur  

---