## WSTG-APIT-02 — API Broken Object Level Authorization

Le Broken Object Level Authorization (BOLA), également connu sous le nom de Insecure Direct Object Reference (IDOR), survient lorsqu'une API ne parvient pas à valider si un utilisateur dispose des autorisations appropriées pour accéder à une ressource spécifique identifiée par un ID ou pour la manipuler. Les attaquants exploitent cette faille en énumérant systématiquement ou en devinant les identifiants — tels que des ID numériques ou des UUID — dans les chemins de requête, les paramètres de requête (query parameters) ou les corps JSON afin d'accéder à des données appartenant à d'autres utilisateurs. Cette vulnérabilité est le problème le plus courant et le plus impactant dans la sécurité des API modernes, pouvant mener à une exfiltration massive de données, à des modifications non autorisées ou à une prise de contrôle totale de compte. Du point de vue de l'attaquant, l'objectif est d'identifier les endpoints qui acceptent des identifiants d'objet et de tester si la logique d'autorisation côté serveur est absente ou peut être contournée en manipulant ces ID.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-APIT-02 |
| **CWE** | CWE-285 |
| **État du test** | Non réalisé |
| **Sévérité** | Élevée / Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/02-API_Broken_Object_Level_Authorization  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/api-testing  

**Outils :** `Burp Suite (Intruder/Repeater)`, `Autorize`, `Postman`, `ffuf`, `Arjun`

### Les identifiants d'objet sont-ils prévisibles ou énumérables au sein des requêtes API ?
- [ ] Non — les identifiants sont longs, aléatoires et sécurisés cryptographiquement (ex: UUIDv4)  
- [ ] Oui — les identifiants sont des entiers séquentiels (ex: `101`, `102`)  
- [ ] Oui — les identifiants suivent un modèle prévisible ou sont dérivés d'informations publiques  

### L'API effectue-t-elle une validation côté serveur de la propriété de l'objet pour chaque requête ?
- [ ] Oui — les contrôles d'autorisation sont appliqués pour chaque requête et aucun contournement n'est **possible**  
- [ ] Oui — les contrôles sont appliqués mais un contournement **est possible** via Parameter Pollution ou Method Tunneling  
- [ ] Non — l'application se repose uniquement sur le fait que l'utilisateur soit authentifié sans vérifier la propriété spécifique de la ressource  

### Est-il possible d'accéder à la ressource d'un autre utilisateur ou de la modifier en changeant l'identifiant ?
- [ ] Non — l'accès non autorisé aux ressources d'autres utilisateurs n'est **pas possible**  
- [ ] Oui — l'accès non autorisé en **lecture** (BOLA horizontal) **est possible**  
- [ ] Oui — la **modification** ou la **suppression** non autorisée (BOLA horizontal) **est possible**  

### L'API permet-elle d'accéder à des objets de niveau administratif ou système en substituant les ID ?
- [ ] Non — les ressources administratives sont protégées par des couches d'autorisation secondaires  
- [ ] Oui — l'accès ou la modification de ressources de niveau système (BOLA vertical) **est possible**  

### Le contrôle d'autorisation peut-il être contourné en déplaçant l'identifiant vers une autre partie de la requête ?
- [ ] Non — la logique d'autorisation est cohérente quel que soit l'emplacement du paramètre  
- [ ] Oui — un contournement **est possible** lorsque l'ID est déplacé du chemin URL vers le corps JSON ou les en-têtes  
- [ ] Oui — un contournement **est possible** lorsque plusieurs ID sont fournis et que le serveur traite celui qui n'est pas autorisé  

---