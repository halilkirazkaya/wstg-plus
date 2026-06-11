## WSTG-INPV-20 — Mass Assignment

Le Mass Assignment, également connu sous le nom d'Overposting ou de désérialisation non sécurisée de paramètres, se produit lorsqu'une application web lie automatiquement les paramètres de requête HTTP entrante aux propriétés d'objets internes sans filtrage approprié des attributs. Cette vulnérabilité est fréquente dans les frameworks MVC modernes et les APIs RESTful où les données JSON, XML ou encodées par formulaire sont directement désérialisées dans les modèles de données côté application. Les attaquants exploitent ce comportement en injectant des paramètres supplémentaires non répertoriés dans une requête — tels que `isAdmin`, `role` ou `account_balance` — pour modifier des champs sensibles que les développeurs n'avaient pas l'intention d'exposer au contrôle de l'utilisateur. Une exploitation réussie entraîne généralement une élévation de privilèges (Privilege Escalation), une modification de données non autorisée ou le contournement de workflows critiques de la logique métier.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INPV-20 |
| **CWE** | CWE-915 |
| **Statut du Test** | Non réalisé |
| **Sévérité** | Haute / Moyenne* |

> *La sévérité devient Haute si des attributs administratifs, des niveaux de permission ou des champs de facturation peuvent être modifiés.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/20-Testing_for_Mass_Assignment  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Arjun`, `Param Miner`, `Burp Suite (Repeater)`, `Postman`, `ffuf`

### L'application utilise-t-elle la liaison automatique (automatic binding) pour les paramètres de requête entrante ?
- [ ] Non — les paramètres sont mappés manuellement à des variables spécifiques ou à des objets sécurisés  
- [ ] Oui — la liaison automatique est utilisée mais des **listes blanches** (white-lists) strictes ou des DTO (Data Transfer Objects) sont appliqués  
- [ ] Oui — la liaison automatique est utilisée et des attributs internes sensibles **peuvent** être modifiés  

### Des attributs administratifs ou liés aux permissions peuvent-ils être injectés avec succès ?
- [ ] Non — les paramètres injectés sont ignorés, supprimés ou provoquent une erreur de validation  
- [ ] Oui — les paramètres supplémentaires sont acceptés mais n'affectent **pas** l'état de l'objet backend  
- [ ] Oui — l'injection de paramètres tels que `is_admin`, `role` ou `permissions` permet **avec succès** une élévation de privilèges *(Critique)*  

### Est-il possible de modifier la logique métier sensible ou des champs financiers via l'overposting ?
- [ ] Non — les champs tels que `price`, `balance` ou `status` sont protégés contre le Mass Assignment  
- [ ] Oui — les attributs métier sensibles **peuvent** être modifiés en les ajoutant au corps de la requête JSON ou du formulaire  

### L'application révèle-t-elle des structures d'objets internes facilitant la devinette de paramètres ?
- [ ] Non — les réponses incluent uniquement les champs publics prévus et la documentation est restreinte  
- [ ] Oui — les champs d'objets internes sont visibles dans les réponses GET, rendant la découverte de paramètres **possible**  
- [ ] Oui — la documentation de l'API (Swagger/OpenAPI) énumère explicitement les propriétés internes qui **peuvent** être affectées  

---