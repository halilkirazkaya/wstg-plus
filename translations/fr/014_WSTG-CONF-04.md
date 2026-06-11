## WSTG-CONF-04 — Revue des anciens fichiers de sauvegarde et non référencés pour les informations sensibles

La revue des anciens fichiers de sauvegarde et des fichiers non référencés consiste à identifier les fichiers oubliés, temporaires ou cachés sur un serveur web qui ne sont pas destinés à un accès public. Ces fichiers incluent souvent des sauvegardes de code source (`.zip`, `.bak`), des fichiers temporaires d'éditeurs de texte (`.swp`, `~`), ou des métadonnées de contrôle de version (`.git`, `.svn`) susceptibles de divulguer des informations sensibles. Les attaquants utilisent des listes de mots (wordlists) automatisées et des outils de découverte pour effectuer du Brute Force sur les conventions de nommage courantes, dans le but d'exfiltrer des identifiants de base de données, des clés d'API (API keys) codées en dur ou une logique facilitant une exploitation ultérieure. Cette vulnérabilité provient généralement d'une maintenance manuelle du serveur ou de pipelines CI/CD défaillants qui ne nettoient pas la racine web de production.


| Champ | Valeur |
|---|---|
| **ID WSTG** | WSTG-CONF-04 |
| **CWE** | CWE-530 |
| **Statut du Test** | Non effectué |
| **Sévérité** | Moyenne / Haute* |

> *La sévérité devient Haute si des fichiers de configuration, des archives de code source ou des identifiants sont découverts.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/04-Review_Old_Backup_and_Unreferenced_Files_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `ffuf`, `dirsearch`, `gobuster`, `GitTools`, `Burp Suite (Engagement Tools)`, `Wfuzz`

### Le listage de répertoires (directory listing) est-il activé sur le serveur web ?
- [ ] Non — le listage de répertoires est **désactivé** et renvoie une erreur 403 Forbidden ou une erreur personnalisée  
- [ ] Oui — le listage de répertoires est **activé** sur des répertoires non sensibles  
- [ ] Oui — le listage de répertoires est **activé** sur des répertoires contenant du code source ou des fichiers sensibles *(Critique)*  

### Des fichiers de sauvegarde avec des extensions courantes (ex : .bak, .old, .save) sont-ils découvrables ?
- [ ] Non — les extensions de sauvegarde courantes ne sont **pas trouvées** ou sont bloquées par la configuration du serveur  
- [ ] Oui — des fichiers de sauvegarde existent mais ne contiennent **pas** d'informations sensibles  
- [ ] Oui — des fichiers de sauvegarde de configuration ou de code source **sont** accessibles *(Haute)*  

### Des répertoires de contrôle de version (ex : .git, .svn) ou des métadonnées sont-ils exposés ?
- [ ] Non — les métadonnées de contrôle de version ne sont **pas présentes** ou sont correctement bloquées  
- [ ] Oui — des métadonnées existent mais le dépôt complet **ne peut pas** être reconstruit  
- [ ] Oui — l'intégralité du dépôt de code source **peut** être exfiltrée via les métadonnées exposées  

### Des archives compressées (ex : .zip, .tar.gz, .rar) de l'application sont-elles présentes à la racine web ?
- [ ] Non — aucun fichier d'archive sensible n'a été découvert via Brute Force ou énumération  
- [ ] Oui — des archives ont été trouvées mais elles sont protégées par mot de passe ou contiennent des ressources publiques  
- [ ] Oui — des archives non chiffrées contenant le code source ou les données de l'application **sont** accessibles publiquement  

### L'application laisse-t-elle fuiter des fichiers temporaires créés par des éditeurs de texte ou des IDE ?
- [ ] Non — les fichiers temporaires tels que `.swp`, `~` ou `.DS_Store` ne sont **pas présents**  
- [ ] Oui — des fichiers temporaires non sensibles sont **présents**  
- [ ] Oui — les fichiers temporaires révèlent des fragments de code source ou des chemins internes  

---