## WSTG-ATHZ-03 — Test de l'élévation de privilèges

L'élévation de privilèges (Privilege Escalation) se produit lorsqu'un attaquant exploite des vulnérabilités pour accéder à des ressources ou des fonctionnalités réservées à des utilisateurs disposant de niveaux d'autorisation plus élevés ou d'identités différentes. Dans l'escalade verticale, un utilisateur standard tente d'accéder à des fonctions administratives, tandis que l'escalade horizontale implique l'accès à des données appartenant à un autre utilisateur ayant le même niveau de privilège. Ces failles se manifestent généralement par des listes de contrôle d'accès (ACL) mal implémentées, des références directes non sécurisées à des objets (IDOR) ou la manipulation de paramètres au sein des jetons de session ou d'identité. Du point de vue d'un attaquant, cela est souvent réalisé en manipulant des identifiants numériques, en modifiant des paramètres basés sur les rôles dans les cookies ou les JWT, ou en appelant directement des points de terminaison d'API (API endpoints) masqués qui ne font l'objet d'aucun contrôle d'autorisation côté serveur.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-ATHZ-03 |
| **CWE** | CWE-269 |
| **Statut du Test** | Non réalisé |
| **Sévérité** | Élevée / Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/03-Testing_for_Privilege_Escalation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**Outils :** `Burp Suite`, `Authorize`, `AuthMatrix`, `Authz`, `Postman`, `Ffuf`

### L'application implémente-t-elle plusieurs niveaux de privilèges ou le multi-tenancy ?
- [ ] Non — l'application est un système à utilisateur unique ou à rôle unique  
- [ ] Oui — plusieurs rôles ou locataires (tenants) **sont** définis et nécessitent des tests d'autorisation  

### Une élévation de privilèges horizontale (accès de même niveau) est-elle possible ?
- [ ] Non — les contrôles d'autorisation **sont appliqués** à tous les identifiants de ressources et propriétaires de sessions  
- [ ] Oui — l'accès aux données d'autres utilisateurs **est possible** via IDOR ou manipulation de paramètres  
- [ ] Oui — la fuite de données **est possible** mais la modification des données d'autres utilisateurs **n'est pas possible**  

### Une élévation de privilèges verticale (bas vers haut) est-elle possible ?
- [ ] Non — les points de terminaison administratifs appliquent strictement les contrôles d'accès basés sur les rôles (RBAC)  
- [ ] Oui — les fonctions administratives **peuvent** être accédées par des utilisateurs peu privilégiés via un accès direct par URL  
- [ ] Oui — les fonctions administratives **peuvent** être accédées en manipulant des en-têtes liés aux rôles, des cookies ou des revendications JWT  

### Les rôles ou permissions des utilisateurs peuvent-ils être modifiés via Mass Assignment ou Parameter Pollution ?
- [ ] Non — les champs basés sur les rôles sont strictement en lecture seule et **ne peuvent pas** être modifiés par les utilisateurs  
- [ ] Oui — les permissions des utilisateurs **peuvent** être élevées en incluant des paramètres cachés (ex: `role=admin`) lors des mises à jour de profil ou de l'inscription  

### Les contrôles d'autorisation sont-ils appliqués de manière cohérente sur toutes les versions d'API et méthodes HTTP ?
- [ ] Oui — les contrôles **sont appliqués** de manière cohérente sur toutes les versions et méthodes  
- [ ] Non — les versions d'API héritées (ex: `/v1/`) ou des méthodes HTTP spécifiques (ex: `PUT`, `DELETE`, `PATCH`) **contournent** l'autorisation  
- [ ] Non — les fonctions administratives sont masquées dans l'interface utilisateur mais **activées** au niveau de l'API pour tous les utilisateurs  

---