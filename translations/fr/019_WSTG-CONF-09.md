## WSTG-CONF-09 — Test des permissions de fichiers

Le test des permissions de fichiers consiste à auditer les niveaux de contrôle d'accès assignés aux fichiers et répertoires sur le serveur web afin de s'assurer que les ressources sensibles ne sont pas sur-exposées à des utilisateurs ou processus non autorisés. Des permissions mal configurées peuvent permettre à des attaquants de lire des fichiers de configuration sensibles contenant des identifiants de base de données, de visualiser du code source propriétaire ou de modifier des scripts côté serveur pour parvenir à une exécution de code à distance (Remote Code Execution). Cette vulnérabilité survient généralement lors de la phase de déploiement lorsque les drapeaux par défaut "world-readable" ou "world-writable" sont laissés sur des répertoires critiques de l'application, tels que les chemins de configuration, les dossiers de logs ou les dépôts d'upload. Du point de vue d'un attaquant, la découverte d'un fichier mal sécurisé sert souvent de catalyseur principal pour une escalade de privilèges (Privilege Escalation) ou un compromis complet du système.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CONF-09 |
| **CWE** | CWE-732 |
| **Statut du test** | Non réalisé |
| **Sévérité** | Moyenne / Haute* |

> *La sévérité devient Haute si des fichiers de configuration sensibles (ex: `.env`, `web.config`) sont lisibles ou si un accès en écriture à la racine web (web root) est possible.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/09-Test_File_Permission  
* https://hacktricks.wiki/en/linux-hardening/privilege-escalation/index.html  

**Outils :** `ls`, `find`, `LinPeas`, `WinPeas`, `ffuf`, `dirsearch`, `Nmap`

### Les fichiers de configuration sensibles de l'application sont-ils protégés contre les accès non autorisés ?
- [ ] Oui — les fichiers de configuration (ex : `.env`, `config.php`) ne sont **pas accessibles** via des requêtes web  
- [ ] Oui — les fichiers sont présents mais l'accès est **restreint** aux seuls utilisateurs locaux autorisés  
- [ ] Non — les fichiers de configuration sensibles **sont accessibles** et divulguent des identifiants ou des secrets  

### Un attaquant peut-il modifier des fichiers ou des répertoires au sein de la racine web (web root) ?
- [ ] Non — tous les répertoires de la racine web sont en **lecture seule** pour l'utilisateur du serveur web  
- [ ] Oui — des répertoires accessibles en écriture existent mais l'exécution de scripts est **désactivée**  
- [ ] Oui — des répertoires accessibles en écriture par tous (world-writable) existent et **permettent** le téléchargement et l'exécution de scripts *(Critique)*  

### Les métadonnées de contrôle de version ou les fichiers de sauvegarde sont-ils exposés en raison de permissions laxistes ?
- [ ] Non — `.git`, `.svn` et les fichiers de sauvegarde (ex : `.bak`, `~`) ne sont **pas présents** ou sont protégés  
- [ ] Oui — des répertoires de métadonnées existent mais le listage de répertoire (directory listing) est **désactivé**  
- [ ] Oui — les métadonnées sensibles ou les sauvegardes sont **entièrement accessibles**, permettant la récupération du code source  

### L'utilisateur du serveur web fonctionne-t-il selon le principe du moindre privilège ?
- [ ] Oui — le processus du serveur web s'exécute en tant qu'utilisateur **dédié à bas privilèges**  
- [ ] Non — le processus du serveur web s'exécute avec des **privilèges inutiles** (ex : `root` ou `Administrator`)  

### Les fichiers de logs sont-ils sécurisés contre la lecture ou la modification non autorisée ?
- [ ] Oui — les fichiers de logs sont **restreints** aux utilisateurs administratifs  
- [ ] Oui — les fichiers de logs sont lisibles mais ne contiennent **pas** de jetons de session ou de PII  
- [ ] Non — les fichiers de logs sont **lisibles publiquement** et divulguent des données de transaction sensibles ou des informations utilisateur  

---