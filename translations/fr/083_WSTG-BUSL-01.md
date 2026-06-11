## WSTG-BUSL-01 — Test de validation des données de la logique métier

Les tests de validation des données de la logique métier se concentrent sur la capacité de l'application à identifier et à rejeter les données qui sont syntaxiquement correctes mais logiquement invalides dans le contexte du domaine métier. Les attaquants exploitent ces failles en soumettant des valeurs inattendues — telles que des quantités négatives dans un panier d'achat, des dates passées pour des événements futurs ou des entiers extrêmement grands — pour contourner les contraintes et manipuler l'état de l'application. Ces vulnérabilités résident souvent dans des workflows en plusieurs étapes, des processus de paiement et des modules de gestion de compte où la logique côté serveur ne parvient pas à vérifier l'intégrité contextuelle des données fournies par l'utilisateur. Une exploitation réussie peut entraîner des pertes financières, une compromission de l'intégrité et un accès non autorisé à des fonctionnalités ou des données restreintes.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-BUSL-01 |
| **CWE** | CWE-20 |
| **Statut du test** | Non effectué |
| **Sévérité** | Élevée / Moyenne |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/01-Test_Business_Logic_Data_Validation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**Outils :** `Burp Suite (Repeater/Intruder)`, `Postman`, `Python (Requests)`, `OWASP ZAP`

### L'application impose-t-elle des limites de plage logique sur les entrées numériques ?
- [ ] Oui — l'application rejette correctement les valeurs négatives, nulles ou excessivement grandes lorsqu'elles sont inappropriées *(Le plus sécurisé)*  
- [ ] Oui — l'application rejette certaines plages invalides mais **est** vulnérable à un dépassement d'entier (integer overflow) ou à un contournement de limite (boundary bypass)  
- [ ] Non — l'application accepte n'importe quelle valeur numérique sans tenir compte du contexte métier *(Critique)*  

### Les contrôles de validation côté serveur sont-ils cohérents à travers toutes les couches de l'application ?
- [ ] Oui — la validation est appliquée de manière cohérente au niveau des couches API/service et base de données  
- [ ] Oui — la validation **est appliquée** au niveau de l'UI/Frontend mais un contournement via une requête API directe **est possible**  
- [ ] Non — la validation n'est **pas appliquée** ou est incohérente, permettant une corruption logique des données  

### La logique métier peut-elle être détournée en manipulant les données dans des workflows en plusieurs étapes ?
- [ ] Non — l'état est maintenu de manière sécurisée sur le serveur et les données des étapes précédentes **ne peuvent pas** être altérées  
- [ ] Oui — la validation a lieu lors des premières étapes, mais la soumission finale **ne vérifie pas** à nouveau l'intégrité des données  
- [ ] Oui — le contournement du workflow **est possible** en sautant des étapes ou en soumettant des données dans le désordre  

### L'application gère-t-elle correctement les données de type "cas limites" (edge cases) qui contournent les contraintes métier ?
- [ ] Oui — les contraintes sont robustes et gèrent les entrées inhabituelles mais syntaxiquement valides (ex. : années bissextiles, changements de fuseau horaire)  
- [ ] Oui — certains cas limites sont gérés, mais des combinaisons spécifiques **peuvent** entraîner un comportement inattendu  
- [ ] Non — les cas limites entraînent directement des échecs logiques, tels que des achats gratuits ou des changements de statut non autorisés  

---