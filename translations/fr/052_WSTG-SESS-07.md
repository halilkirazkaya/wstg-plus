## WSTG-SESS-07 — Test du délai d'expiration de session

Le test du délai d'expiration de session (Session Timeout) évalue si une application met fin efficacement à la session d'un utilisateur après une période d'inactivité prédéfinie ou une durée totale. Ce contrôle est essentiel pour atténuer les risques de Session Hijacking, en particulier sur les postes de travail partagés ou dans les scénarios où un attaquant capture un identifiant de session via une interception réseau ou une XSS. Les Pentesters analysent à la fois les délais d'expiration d'inactivité (Idle Timeouts), qui se déclenchent après une période d'inactivité, et les délais d'expiration absolus (Absolute Timeouts), qui limitent la durée de vie totale d'une session indépendamment de l'activité. Du point de vue d'un attaquant, des sessions indéfinies ou excessivement longues offrent une fenêtre d'opportunité étendue pour maintenir un accès non autorisé et contourner la nécessité d'une ré-authentification.

| Champ | Valeur |
|---|---|
| **ID WSTG** | WSTG-SESS-07 |
| **CWE** | CWE-613 |
| **État du test** | Non effectué |
| **Sévérité** | Moyenne |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/07-Testing_Session_Timeout  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Browser DevTools`

### L'application implémente-t-elle un délai d'expiration de session d'inactivité ?
- [ ] Oui — la session expire après une courte période (ex : 15-30 minutes) d'inactivité  
- [ ] Oui — la session expire mais la durée du délai est **excessivement longue** (ex : > 24 heures)  
- [ ] Non — la session **reste active** indéfiniment pendant l'inactivité  

### Le délai d'expiration de la session est-il appliqué côté serveur ?
- [ ] Oui — le serveur rejette les requêtes avec des jetons expirés quel que soit l'état côté client  
- [ ] Non — le délai d'expiration est **uniquement** appliqué via JavaScript côté client ou meta-refresh  

### L'application implémente-t-elle un délai d'expiration de session absolu ?
- [ ] Oui — la session est terminée après une durée maximale fixe indépendamment de l'activité  
- [ ] Non — la session **peut** être prolongée indéfiniment tant qu'il y a une activité continue  

### L'identifiant de session est-il invalidé sur le serveur lors de l'expiration ?
- [ ] Oui — le jeton de session **ne peut pas** être réutilisé une fois le seuil d'expiration atteint  
- [ ] Non — le jeton de session **est toujours valide** sur le serveur même après que le client a été redirigé vers la page de connexion  

### La session peut-elle être maintenue active indéfiniment à l'aide de requêtes heartbeat automatisées ?
- [ ] Non — un délai d'expiration absolu ou des contrôles secondaires **empêchent** une extension de session infinie  
- [ ] Oui — des requêtes périodiques en arrière-plan (ex : AJAX) **peuvent** maintenir la session indéfiniment  

---