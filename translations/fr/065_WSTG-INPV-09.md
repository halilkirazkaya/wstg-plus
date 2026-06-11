## WSTG-INPV-09 — Test d'injection XPath (XPath Injection)

L'injection XPath (XPath Injection) se produit lorsqu'une application utilise des informations fournies par l'utilisateur pour construire une requête XPath pour des données XML, permettant à un attaquant d'interférer avec la logique de la requête. En injectant une syntaxe spécifique telle que des guillemets simples, des crochets et des opérateurs logiques, un attaquant peut naviguer dans la structure du document XML, contourner l'authentification ou exfiltrer des nœuds de données sensibles. Cette vulnérabilité se trouve couramment dans les services web, les interfaces de gestion de configuration et les systèmes hérités qui s'appuient sur des magasins de données basés sur XML plutôt que sur des bases de données SQL traditionnelles. Du point de vue d'un attaquant, cela est souvent exploité à l'aide de techniques basées sur les booléens ou les erreurs pour cartographier systématiquement l'arborescence XML et récupérer des valeurs cachées du backend.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INPV-09 |
| **CWE** | CWE-643 |
| **Statut du Test** | Non réalisé |
| **Sévérité** | Élevée / Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/09-Testing_for_XPath_Injection  
* https://hacktricks.wiki/en/pentesting-web/xpath-injection.html  

**Outils :** `Burp Suite (Intruder)`, `XCat`, `XPath-Injection-Tool`, `FFUF`

### Les entrées utilisateur utilisées pour construire les requêtes XPath sont-elles correctement assainies ou paramétrées ?
- [ ] Oui — les entrées ne sont **pas** utilisées dans les requêtes XPath ou sont strictement paramétrées  
- [ ] Oui — une validation/un encodage robuste des entrées **est appliqué** et un contournement n'est **pas possible**  
- [ ] Oui — une certaine validation **est appliquée** mais un contournement **est possible** en utilisant des caractères encodés  
- [ ] Non — l'entrée utilisateur est directement concaténée dans les requêtes XPath *(Critique)*  

### L'application révèle-t-elle des détails structurels XML/XPath via des messages d'erreur ?
- [ ] Non — des pages d'erreur génériques sont affichées et **aucun** détail XML n'est divulgué  
- [ ] Oui — les messages d'erreur révèlent la présence d'un traitement XML mais **aucun** détail de chemin  
- [ ] Oui — des messages d'erreur verbeux révèlent la syntaxe XPath, les noms de nœuds ou les chemins de fichiers  

### Est-il possible de contourner la logique d'authentification ou d'autorisation en utilisant la logique XPath ?
- [ ] Non — la logique d'authentification ne repose **pas** sur XML/XPath ou est implémentée de manière sécurisée  
- [ ] Oui — les vérifications de connexion ou de permissions peuvent être contournées à l'aide de payloads basés sur la logique comme `' or 1=1 or '`  

### La structure ou les données du document XML peuvent-elles être énumérées via une injection aveugle (Blind Injection) ?
- [ ] Non — l'application n'est **pas** sensible à l'inférence basée sur les booléens ou sur le temps  
- [ ] Oui — les noms de nœuds et les données **peuvent** être exfiltrés en utilisant l'inférence basée sur les booléens  
- [ ] Oui — le parcours complet de l'arborescence XML et l'extraction de données **sont possibles**  

---