## WSTG-APIT-99 — Testing GraphQL

GraphQL est un langage de requête pour les API qui permet aux clients de demander des structures de données spécifiques, mais les mauvaises configurations entraînent souvent des risques de sécurité importants, notamment la divulgation d'informations et l'épuisement des ressources. Les vulnérabilités proviennent généralement de l'activation de l'introspection, de l'absence de limites de profondeur ou de complexité des requêtes, et du Broken Object-Level Authorization (BOLA) au sein des fonctions de résolution (resolvers). Les attaquants exploitent ces points de terminaison (endpoints) en utilisant l'introspection pour cartographier l'ensemble du schéma, en exploitant des fragments circulaires pour déclencher un Denial of Service (DoS), ou en contournant l'autorisation par l'accès à des champs non autorisés via des requêtes imbriquées. Les tests se concentrent sur les endpoints `/graphql`, `/v1/graphql`, ou `/graphiql` afin de s'assurer que l'implémentation impose des contrôles d'accès et une gestion des ressources stricts.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-APIT-99 |
| **CWE** | CWE-200 |
| **État du test** | Non réalisé |
| **Sévérité** | Élevée / Critique* |

> *La sévérité devient Critique si le Broken Object-Level Authorization (BOLA) permet un accès non autorisé à des PII ou à des mutations administratives.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/99-Testing_GraphQL  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/graphql  

**Outils :** `InQL`, `Graphw00f`, `GraphQL Voyager`, `Burp Suite`, `Altair GraphQL Client`, `Clairvoyance`

### Le système d'introspection GraphQL est-il activé ?
- [ ] Non — l'introspection est **désactivée** et le schéma ne peut pas être cartographié  
- [ ] Oui — l'introspection est **activée** mais nécessite une authentification administrative  
- [ ] Oui — l'introspection est **activée** et accessible publiquement *(Élevée)*  

### Des limites de ressources (profondeur et complexité) sont-elles imposées sur les requêtes ?
- [ ] Oui — des limites strictes de profondeur et de complexité de requête sont **appliquées** et imposées  
- [ ] Oui — des limites sont **appliquées** mais peuvent être contournées en utilisant des alias ou des fragments  
- [ ] Non — aucune limite n'est **appliquée**, permettant des requêtes récursives et des DoS  

### Le Broken Object-Level Authorization (BOLA) est-il présent dans les résolveurs ?
- [ ] Non — l'autorisation est **appliquée** au niveau des champs et des objets pour chaque requête  
- [ ] Oui — l'autorisation est **appliquée** à certains champs mais des objets sensibles sont exposés  
- [ ] Oui — l'autorisation n'est **pas appliquée**, permettant l'accès à n'importe quel objet via la manipulation d'ID  

### Les mutations GraphQL sont-elles correctement protégées contre une utilisation non autorisée ?
- [ ] Non — les mutations ne sont **pas** présentes dans le schéma  
- [ ] Oui — les mutations nécessitent une authentification valide et des contrôles d'autorisation stricts  
- [ ] Oui — les mutations sont **possibles** pour des utilisateurs non authentifiés ou manquent d'autorisation  

### Les messages d'erreur divulguent-ils des détails sensibles sur l'implémentation ou le schéma ?
- [ ] Non — les messages d'erreur sont génériques et ne **révèlent pas** de logique interne  
- [ ] Oui — les messages d'erreur révèlent les types de base de données sous-jacents ou des suggestions via « Did you mean...? »  
- [ ] Oui — des traces d'appels (stack traces) complètes et des données de configuration sensibles sont **révélées** dans les réponses  

---