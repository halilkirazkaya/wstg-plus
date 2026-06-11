## WSTG-SESS-08 — Session Puzzling

Le Session Puzzling, également connu sous le nom de Surcharge de Variables de Session (Session Variable Overloading), se produit lorsqu'une application utilise la même variable de session pour plusieurs objectifs à travers différents modules ou ne parvient pas à réinitialiser correctement les états de session lors des transitions de flux de travail. Les attaquants exploitent ce comportement en alimentant des variables de session dans un contexte donné — tel qu'un aperçu de profil non authentifié ou une inscription en plusieurs étapes — puis en naviguant vers une zone protégée où l'application fait incorrectement confiance à ces variables préexistantes. Cela peut entraîner des impacts critiques, notamment le contournement de l'authentification (Authentication Bypass), l'escalade de privilèges (Privilege Escalation) ou l'accès non autorisé à des données sensibles en trompant l'application pour lui faire croire qu'une session se trouve dans un état de privilège plus élevé qu'elle ne l'est réellement. Cette vulnérabilité est particulièrement présente dans les applications complexes avec état (stateful) où la logique de gestion de session est appliquée de manière incohérente entre les différents composants fonctionnels.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-SESS-08 |
| **CWE** | CWE-621 |
| **Statut du test** | Non réalisé |
| **Sévérité** | Élevée / Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/08-Testing_for_Session_Puzzling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite (Repeater/Comparator)`, `OWASP ZAP`, `Postman`, `Cookie Editor`

### L'application utilise-t-elle les mêmes noms de variables de session à des fins différentes à travers des modules distincts ?
- [ ] Non — les noms de variables sont uniques ou les portées (scopes) sont isolées  
- [ ] Oui — des noms de variables partagés existent mais l'état est effacé entre les modules  
- [ ] Oui — des noms de variables partagés existent et l'état **n'est pas** effacé  

### Des actions non authentifiées peuvent-elles influencer des variables de session utilisées dans des contextes authentifiés ?
- [ ] Non — les variables de session sont initialisées uniquement **après** une authentification réussie  
- [ ] Oui — les variables de session peuvent être définies avant la connexion mais n'affectent **pas** l'état post-connexion  
- [ ] Oui — les variables de session définies pendant la pré-authentification **sont** approuvées après l'authentification *(Critique)*  

### L'application réinitialise-t-elle l'identifiant de session et ses variables associées lors d'un changement de niveau de privilège ?
- [ ] Oui — la session est entièrement réinitialisée et un nouvel ID **est émis**  
- [ ] Partiel — un nouvel ID est émis mais certaines variables de session **persistent**  
- [ ] Non — l'ID de session et toutes les variables **restent** inchangés  

### Des privilèges administratifs peuvent-ils être obtenus en manipulant des variables de session via un compte non privilégié ?
- [ ] Non — les variables de privilège **ne peuvent pas** être modifiées par les utilisateurs  
- [ ] Oui — les variables peuvent être modifiées mais l'application les **valide** par rapport à la base de données backend  
- [ ] Oui — les variables peuvent être modifiées et l'application **fait confiance** à l'état de la session de manière implicite *(Critique)*  

### L'état de la session est-il effacé immédiatement après l'achèvement d'un flux de travail spécifique (ex : réinitialisation de mot de passe, paiement) ?
- [ ] Oui — les variables de session du flux de travail sont détruites une fois celui-ci terminé  
- [ ] Non — les variables du flux de travail **persistent** et peuvent être réutilisées dans d'autres zones de l'application  

---