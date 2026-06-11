## WSTG-CONF-03 — Test de la gestion des extensions de fichiers pour les informations sensibles

Le test de la gestion des extensions de fichiers consiste à identifier si le serveur web ou le serveur d'application révèle des informations sensibles en servant des fichiers qui devraient être restreints ou exécutés. Les attaquants recherchent fréquemment des fichiers de sauvegarde (backup), des fichiers de configuration et des fragments de code source (ex. : `.bak`, `.old`, `.env`, `.inc`) qui pourraient être laissés à la racine du site web (web root) et servis en texte brut en raison d'une absence de mappage de gestionnaire (handler mapping) spécifique. Cette vulnérabilité est importante car elle peut mener à l'exposition d'identifiants de base de données, de clés d'API et de la logique métier interne, facilitant une exploitation plus profonde de l'infrastructure. Ces expositions se produisent généralement dans les répertoires publics, les dossiers de téléchargement (upload) ou via des paramètres de type MIME mal configurés sur le serveur.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CONF-03 |
| **CWE** | CWE-552 |
| **État du test** | Non effectué |
| **Sévérité** | Moyenne / Haute* |

> *La sévérité devient Haute si des fichiers de configuration contenant des identifiants ou l'intégralité du code source sont accessibles.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/03-Test_File_Extensions_Handling_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `ffuf`, `gobuster`, `dirsearch`, `Burp Suite (Intruder)`, `Wfuzz`

### Les extensions de fichiers de sauvegarde ou temporaires sensibles (ex. : `.bak`, `.old`, `.swp`, `~`) sont-elles accessibles ?
- [ ] Non — le serveur renvoie 403 Forbidden ou 404 Not Found pour les extensions de sauvegarde courantes  
- [ ] Oui — le serveur autorise le téléchargement de fichiers de sauvegarde mais ils **ne contiennent pas** de données sensibles  
- [ ] Oui — le serveur autorise le téléchargement de fichiers de sauvegarde contenant des données sensibles ou du code source *(Haute)*  

### Le serveur expose-t-il des fichiers d'environnement ou de configuration système (ex. : `.env`, `.config`, `.yml`, `.ini`) ?
- [ ] Non — les fichiers de configuration restreints **ne peuvent pas** être consultés et renvoient 403/404  
- [ ] Oui — les fichiers de configuration sensibles **sont** accessibles et divulguent des secrets internes ou des identifiants  

### Le code source peut-il être récupéré en ajoutant des extensions alternatives (ex. : `.php.txt`, `.jsp.old`, `.aspx.bak`) ?
- [ ] Non — le code de l'application n'est **pas** rendu en texte brut quelle que soit la manipulation de l'extension  
- [ ] Oui — le code source est révélé car le serveur **ne possède pas** de gestionnaire (handler) pour l'extension ajoutée  

### Les répertoires d'administration ou de métadonnées (ex. : `.git/`, `.svn/`, `.DS_Store`) sont-ils bloqués pour l'accès public ?
- [ ] Non — ces répertoires/fichiers n'existent **pas** à la racine du site web  
- [ ] Oui — l'accès n'est **pas possible** en raison de règles de réécriture côté serveur ou de permissions  
- [ ] Oui — les répertoires de métadonnées **sont** accessibles et permettent une reconstruction complète du code source  

### Comment le serveur gère-t-il les requêtes pour des fichiers ayant plusieurs extensions (ex. : `file.php.jpg`) ?
- [ ] Non — le serveur donne correctement la priorité à la sécurité de l'extension interne ou bloque la requête  
- [ ] Oui — le serveur traite le fichier selon la première extension, mais un contournement (bypass) n'est **pas possible** pour l'exécution  
- [ ] Oui — le serveur ignore l'extension finale, rendant l'exécution de code ou la divulgation de source **possible**  

---