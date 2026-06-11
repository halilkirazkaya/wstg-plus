## WSTG-ERRH-02 — Test de Stack Traces

Les stack traces sont générées lorsqu'une application ne parvient pas à gérer une exception de manière appropriée, ce qui entraîne la divulgation de l'état d'exécution interne et de la hiérarchie d'appels à l'utilisateur final. Pour un testeur de pénétration, ces traces sont inestimables car elles révèlent le framework de l'application, les versions des bibliothèques, les chemins système internes et le flux logique, ce qui réduit considérablement l'effort requis pour la reconnaissance. L'exploitation consiste à déclencher intentionnellement des erreurs via des entrées malformées, des types de données inattendus ou des violations de limites pour forcer l'application dans un état instable et exfiltrer des informations sur la stack technologique sousjacente. Cette fuite fournit souvent le contexte nécessaire pour identifier des composants tiers vulnérables ou pour concevoir des exploits spécifiques pour des failles logiques découvertes dans les chemins de code divulgués.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-ERRH-02 |
| **CWE** | CWE-209 |
| **État du Test** | Non effectué |
| **Sévérité** | Faible / Moyenne* |

> *La sévérité passe à Moyenne si la stack trace révèle des détails architecturaux sensibles, des requêtes de base de données ou des paramètres de configuration interne.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/02-Testing_for_Stack_Traces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite`, `ffuf`, `Arjun`, `Wfuzz`, `Wappalyzer`

### Est-ce que des stack traces complètes sont renvoyées au client lors d'erreurs de l'application ?
- [ ] Non — l'application renvoie des pages d'erreur génériques ou des messages d'erreur personnalisés  
- [ ] Oui — les stack traces sont **activées** mais uniquement pour des modules spécifiques non sensibles  
- [ ] Oui — les stack traces complètes sont **activées** et visibles dans le corps de la réponse HTTP  

### Est-ce que les messages d'erreur divulguent des informations environnementales sensibles ?
- [ ] Non — les messages d'erreur ne contiennent aucun détail technique ou environnemental  
- [ ] Oui — les messages révèlent des **chemins de fichiers internes** ou des **structures de répertoires** côté serveur  
- [ ] Oui — les messages révèlent des **schémas de base de données**, des **requêtes SQL** ou des **versions de bibliothèques tierces**  

### Est-ce que les stack traces peuvent être déclenchées en manipulant les paramètres d'entrée ?
- [ ] Non — les entrées malformées sont gérées de manière appropriée ou rejetées par la validation  
- [ ] Oui — les traces peuvent être déclenchées par des **types de données inattendus** (ex : soumission d'un tableau là où une chaîne de caractères est attendue)  
- [ ] Oui — les traces sont déclenchées par des **octets nuls**, des **caractères spéciaux** ou des **valeurs limites**  

### Un gestionnaire d'erreurs global est-il appliqué de manière cohérente sur l'ensemble de l'application ?
- [ ] Oui — un gestionnaire d'exceptions global est **appliqué de manière cohérente** sur tous les points de terminaison testés  
- [ ] Oui — un gestionnaire est présent mais **n'est pas appliqué** aux endpoints hérités, aux API ou aux endpoints nouvellement ajoutés  
- [ ] No — aucun mécanisme de gestion d'erreurs global n'est **détecté**, ce qui entraîne des erreurs de serveur par défaut  

---