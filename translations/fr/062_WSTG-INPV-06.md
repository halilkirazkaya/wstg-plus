## WSTG-INPV-06 — Testing for LDAP Injection

L'Injection LDAP (LDAP Injection) survient lorsqu'une application intègre des données fournies par l'utilisateur dans des filtres LDAP (Lightweight Directory Access Protocol) sans assainissement ou échappement adéquat, permettant ainsi à un attaquant de manipuler la logique de la requête. En injectant des caractères spéciaux tels que des astérisques, des parenthèses et des opérateurs logiques, les attaquants peuvent modifier le filtre de recherche pour contourner les mécanismes d'authentification ou exfiltrer des informations sensibles de l'annuaire, notamment des noms d'utilisateur, des adhésions à des groupes et des attributs organisationnels. Cette vulnérabilité se manifeste généralement dans des fonctionnalités telles que les recherches d'annuaires d'entreprise, les portails d'employés ou les systèmes d'authentification unique (SSO) qui s'interfacent avec Active Directory ou OpenLDAP. Du point de vue de l'attaquant, une exploitation réussie implique souvent des techniques de Blind Injection pour énumérer la structure de l'annuaire ou les valeurs des attributs bit par bit lorsque la sortie directe de la requête est supprimée par l'application.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INPV-06 |
| **CWE** | CWE-90 |
| **Statut du test** | Non réalisé |
| **Sévérité** | Haute |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/06-Testing_for_LDAP_Injection  
* https://hacktricks.wiki/en/pentesting-web/ldap-injection.html  

**Outils :** `Burp Suite (Intruder/Repeater)`, `ldapsearch`, `JNDIExploit`, `Wfuzz`, `nmap`

### L'application utilise-t-elle LDAP pour l'authentification ou les recherches d'annuaire ?
- [ ] Non — LDAP n'est **pas utilisé** dans l'architecture de l'application  
- [ ] Oui — LDAP est utilisé et toutes les entrées sont strictement assainies ou paramétrées  
- [ ] Oui — LDAP est utilisé et les entrées utilisateur sont concaténées dans les filtres **sans** échappement approprié  

### Les méta-caractères LDAP sont-ils correctement échappés ou filtrés dans les entrées utilisateur ?
- [ ] Oui — les caractères tels que `(`, `)`, `&`, `|`, `*`, et `\` ne sont **pas autorisés** ou sont correctement échappés  
- [ ] Non — les méta-caractères **peuvent** être injectés pour modifier la structure du filtre LDAP  

### Le mécanisme d'authentification peut-il être contourné via une injection ?
- [ ] Non — la logique de connexion n'est **pas susceptible** d'être manipulée par une requête  
- [ ] Oui — l'authentification **peut** être contournée en utilisant une injection de OU logique (`|`) ou de caractère générique (`*`) dans les champs de nom d'utilisateur ou de mot de passe  

### Une exfiltration aveugle (blind) de données est-elle possible depuis le service d'annuaire ?
- [ ] Non — les résultats de recherche sont limités et aucune différence de temps ou de réponse booléenne n'est observée  
- [ ] Oui — les attributs de l'annuaire **peuvent** être énumérés bit par bit via des techniques de Boolean-based Blind Injection  
- [ ] Oui — les enregistrements complets de l'annuaire **sont** reflétés dans la réponse de l'application en raison de la manipulation de la requête  

### Les composants secondaires de l'application (ex : serveurs de messagerie, SSO) sont-ils vulnérables à l'injection d'attributs LDAP ?
- [ ] Non — les attributs LDAP sont gérés de manière sécurisée dans tous les composants  
- [ ] Oui — un attaquant **peut** modifier ses propres attributs (ex : email, appartenance à un groupe) via une injection pour obtenir une escalade de privilèges ou rediriger les communications  

---