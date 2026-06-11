## WSTG-SESS-11 — Test de sessions simultanées (Concurrent Sessions)

Le test de sessions simultanées (Concurrent Sessions) évalue si une application permet à un seul compte utilisateur de maintenir plusieurs sessions actives simultanément à travers différents navigateurs, appareils ou adresses IP. L'absence de restriction sur les sessions simultanées augmente la fenêtre d'opportunité pour les attaquants d'utiliser des jetons de session (Session Tokens) détournés ou des identifiants compromis sans alerter l'utilisateur légitime ou mettre fin aux sessions existantes. Ce comportement est généralement évalué en s'authentifiant depuis plusieurs sources et en surveillant la stabilité de la session, ce qui révèle souvent des faiblesses dans la logique de gestion de session (Session Management) ou une absence de règles métier axées sur la sécurité. Du point de vue d'un attaquant, l'absence de contrôle de simultanéité permet une persistance furtive après un compromis initial et facilite les attaques automatisées en parallèle.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-SESS-11 |
| **CWE** | CWE-613 |
| **État du test** | Non réalisé |
| **Sévérité** | Faible / Moyenne |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/11-Testing_for_Concurrent_Sessions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite (Repeater/Containers)`, `Firefox Multi-Account Containers`, `Postman`, `cURL`

### Les sessions simultanées sont-elles limitées par la politique de l'application ?
- [ ] Oui — une seule session **est autorisée** par utilisateur ; les nouvelles connexions invalident les anciennes *(Le plus sécurisé)*  
- [ ] Oui — un nombre fixe de sessions simultanées **est imposé** et ne peut être dépassé  
- [ ] Non — un nombre illimité de sessions simultanées **est possible**  

### Comment l'application réagit-elle lorsque la limite de sessions est atteinte ?
- [ ] La session active la plus ancienne **est automatiquement terminée** (atténuation de la fixation de session/détournement de session)  
- [ ] L'utilisateur **est invité** à mettre fin aux sessions existantes avant que la nouvelle session ne soit autorisée  
- [ ] La nouvelle tentative de connexion **est bloquée** jusqu'à ce qu'une déconnexion manuelle soit effectuée depuis un autre appareil  
- [ ] Aucune action n'est entreprise — plusieurs sessions restent **activées** simultanément  

### L'application fournit-elle une interface de gestion de session pour l'utilisateur ?
- [ ] Oui — les utilisateurs **peuvent** consulter les sessions actives (IP, appareil, heure) et les terminer à distance  
- [ ] Oui — les utilisateurs **peuvent** consulter les sessions actives mais **ne peuvent pas** les terminer à distance  
- [ ] Non — les utilisateurs **ne peuvent pas** voir ni gérer d'autres sessions actives associées à leur compte  

### L'utilisateur est-il averti lorsqu'une activité de connexion simultanée est détectée ?
- [ ] Oui — une alerte (e-mail, SMS ou in-app) **est déclenchée** pour les connexions provenant de nouveaux appareils ou emplacements  
- [ ] Non — aucune notification **n'est fournie** lorsque des sessions simultanées sont établies  

---