## WSTG-ATHZ-04 — Testing for Insecure Direct Object References

Les Insecure Direct Object References (IDOR) se produisent lorsqu'une application fournit un accès direct aux objets en se basant sur une entrée fournie par l'utilisateur sans effectuer de contrôle d'autorisation pour vérifier les permissions du demandeur. Cette vulnérabilité impacte de manière significative la confidentialité et l'intégrité, car elle permet à des attaquants de consulter, modifier ou supprimer des données appartenant à d'autres utilisateurs ou au système. Elle se manifeste généralement dans les paramètres d'URL, les données du corps POST ou les clés JSON qui font référence à des clés de base de données internes, des noms de fichiers ou des identifiants de compte. Du point de vue d'un attaquant, l'exploitation implique la manipulation de ces identifiants — souvent par l'incrémentation d'entiers ou le fuzzing d'UUID — pour accéder à des ressources en dehors de leur périmètre autorisé.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-ATHZ-04 |
| **CWE** | CWE-639 |
| **Statut du test** | Non effectué |
| **Sévérité** | Élevée |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/04-Testing_for_Insecure_Direct_Object_References  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/access-control  

**Outils :** `Burp Suite (Autorize extension)`, `Caido`, `Postman`, `ffuf`, `Intruder`

### L'application utilise-t-elle des identifiants d'objets directs dans les paramètres de requête ?
- [ ] Non — l'application utilise des références indirectes ou des mappages chiffrés *(Plus sécurisé)*  
- [ ] Oui — l'application utilise des identifiants non prévisibles (ex. UUID longs/hashs)  
- [ ] Oui — l'application utilise des identifiants prévisibles (ex. entiers incrémentaux ou noms d'utilisateur)  

### Un contrôle d'autorisation côté serveur est-il effectué pour chaque requête impliquant un identifiant d'objet ?
- [ ] Oui — les contrôles côté serveur **ne peuvent pas** être contournés pour aucun identifiant  
- [ ] Oui — les contrôles côté serveur sont présents mais un contournement **est possible** via la parameter pollution ou la manipulation d'en-têtes  
- [ ] Non — aucun contrôle d'autorisation **n'est appliqué** pour vérifier la propriété de l'objet demandé  

### Un attaquant peut-il accéder aux objets appartenant à d'autres utilisateurs ou les modifier (IDOR horizontal) ?
- [ ] Non — l'accès aux données d'autres utilisateurs **n'est pas possible**  
- [ ] Oui — la consultation des données d'autres utilisateurs **est possible** mais la modification **ne l'est pas**  
- [ ] Oui — la consultation et la modification des données d'autres utilisateurs **sont possibles** *(Critique)*  

### Est-il possible d'accéder à des objets de niveau administratif ou système (IDOR vertical) ?
- [ ] Non — l'accès à des objets dotés de privilèges supérieurs **n'est pas possible**  
- [ ] Oui — l'accès aux configurations administratives ou aux fichiers système **est possible**  

### L'application divulgue-t-elle des identifiants d'objets via la recherche, les métadonnées ou d'autres points de terminaison ?
- [ ] Non — les identifiants **ne sont pas exposés** involontairement  
- [ ] Oui — les identifiants d'autres utilisateurs/objets **peuvent** être énumérés via des points de terminaison secondaires ou des profils publics  

---