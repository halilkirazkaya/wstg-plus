## WSTG-ATHN-06 — Testing for Browser Cache Weakness

Une faiblesse de la mise en cache du navigateur (Browser cache weakness) survient lorsque des informations sensibles sont stockées dans le cache local du navigateur et peuvent être récupérées par un utilisateur non autorisé ayant accès à la même machine physique. Cette absence d'en-têtes de contrôle de cache appropriés permet à des données potentiellement sensibles, telles que des informations personnelles, des détails de compte ou des identifiants de session (Session identifiers), de persister après que l'utilisateur s'est déconnecté ou a fermé sa session. Des attaquants ou des utilisateurs ultérieurs sur des terminaux partagés ou publics peuvent exploiter cela en naviguant dans l'historique du navigateur, en utilisant le bouton « Retour » ou en inspectant les fichiers du cache local pour exfiltrer les réponses mises en cache. S'assurer de la mise en œuvre de directives strictes telles que `Cache-Control: no-store` et `Pragma: no-cache` est critique pour toute application gérant du contenu authentifié afin de garantir que les données sensibles ne soient jamais écrites sur le disque.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-ATHN-06 |
| **CWE** | CWE-525 |
| **État du test** | Non effectué |
| **Sévérité** | Faible / Moyenne* |

> *La sévérité devient Moyenne si l'application est fréquemment consultée depuis des bornes partagées/publiques ou si elle contient des données PII/PHI hautement sensibles.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/06-Testing_for_Browser_Cache_Weaknesses  

**Outils :** `Burp Suite (Proxy/Repeater)`, `Browser Developer Tools (Network Tab)`, `Zed Attack Proxy (ZAP)`

### Les en-têtes de contrôle de cache appropriés sont-ils présents sur les pages authentifiées ou sensibles ?
- [ ] Oui — `Cache-Control: no-store, no-cache` et `Pragma: no-cache` sont **présents** et **correctement configurés**  
- [ ] Oui — certains en-têtes sont présents mais `no-store` est **manquant**, permettant la mise en cache sur disque  
- [ ] Non — aucun en-tête lié au cache n'est **appliqué**, reposant sur le comportement par défaut du navigateur  

### L'application utilise-t-elle la directive `private` pour les données spécifiques à l'utilisateur ?
- [ ] Oui — `Cache-Control: private` est **activé** pour tout le contenu personnalisé afin d'empêcher la mise en cache par des proxys  
- [ ] Non — le contenu est marqué comme `public` ou ne contient pas la directive `private`, ce qui le rend **possible à mettre en cache** par des proxys intermédiaires  

### Les informations sensibles sont-elles accessibles via le bouton « Retour » du navigateur après une déconnexion réussie ?
- [ ] Non — le navigateur demande une réauthentification ou la page n'est **pas chargée** depuis le cache local  
- [ ] Oui — les informations sensibles sont **toujours visibles** via le bouton retour après la fin de la session  

### Les fichiers non-HTML sensibles (ex. PDF, exports CSV, réponses API JSON) sont-ils protégés contre la mise en cache ?
- [ ] Non — aucun fichier non-HTML sensible n'est généré ou géré par l' application  
- [ ] Oui — des en-têtes de contrôle de cache stricts sont **appliqués** à tous les téléchargements de fichiers sensibles et aux points de terminaison API (API endpoints)  
- [ ] Non — les téléchargements sensibles ou les réponses API sont **stockés** dans le cache local du navigateur ou dans des répertoires temporaires  

### L'application utilise-t-elle `Expires: 0` ou une date passée pour empêcher l'utilisation de données périmées ?
- [ ] Oui — l'en-tête `Expires` est défini sur `0` ou une date historique, **garantissant** que le navigateur traite le contenu comme immédiatement périmé  
- [ ] Non — l'en-tête `Expires` est **manquant** ou défini sur une date future pour les pages sensibles  

---