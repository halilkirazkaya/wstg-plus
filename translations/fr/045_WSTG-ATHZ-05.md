## WSTG-ATHZ-05 — Test des faiblesses OAuth

Les faiblesses OAuth englobent une variété de failles dans l'implémentation des protocoles OAuth 2.0 ou OpenID Connect (OIDC), entraînant souvent une prise de contrôle totale de compte (Account Takeover) ou un accès non autorisé aux données. Ces vulnérabilités se manifestent généralement lors du flux d'autorisation (Authorization Flow), spécifiquement concernant la validation de l'URI de redirection (`redirect_uri`), l'entropie et la vérification du paramètre `state`, et la manipulation sécurisée des jetons d'accès (Access Tokens) ou d'identité (ID Tokens). Les attaquants exploitent ces failles en interceptant les codes d'autorisation, en redirigeant les jetons vers des domaines contrôlés par l'attaquant via des redirections ouvertes (Open Redirects), ou en effectuant des falsifications de requête intersites (CSRF) pour lier la session d'une victime au compte d'un attaquant. Étant donné qu'OAuth sert souvent de mécanisme d'authentification principal, une seule mauvaise configuration peut compromettre l'intégrité de l'ensemble du système d'identité des utilisateurs.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-ATHZ-05 |
| **CWE** | CWE-287 |
| **État du test** | Non réalisé |
| **Sévérité** | Élevée / Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/05-Testing_for_OAuth_Weaknesses  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/oauth  

**Outils :** `Burp Suite (Repeater/Collaborator)`, `CyberChef`, `OIDC-Checker`, `OAuth-Scanner`

### Le paramètre `state` est-il implémenté et validé pour prévenir le CSRF ?
- [ ] Oui — `state` est obligatoire, unique par session et strictement validé sur le serveur  
- [ ] Oui — `state` est présent mais statique ou prévisible entre différentes sessions  
- [ ] Oui — `state` est présent mais l'application **ne le valide pas** lors du rappel (Callback)  
- [ ] Non — le paramètre `state` **n'est pas** utilisé dans la requête d'autorisation *(Critique)*  

### L'URI de redirection (`redirect_uri`) est-elle strictement validée par rapport à une liste blanche ?
- [ ] Oui — seules les correspondances exactes avec une liste blanche (Whitelist) pré-enregistrée sont **acceptées**  
- [ ] Oui — la validation est appliquée mais un contournement (Bypass) **est possible** via un parcours de répertoire (Path Traversal) ou une manipulation de sous-domaine  
- [ ] Oui — la validation est appliquée mais un contournement **est possible** via la pollution de paramètres (Parameter Pollution) ou l'injection de fragments  
- [ ] Non — l'application **autorise** des URL arbitraires dans le paramètre `redirect_uri` *(Critique)*  

### Le paramètre `response_type` peut-il être manipulé pour modifier le flux ?
- [ ] Non — l'application n'accepte que le flux prévu (par exemple, `code`) et rejette les autres  
- [ ] Oui — passer de `code` à `token` (Implicit flow) **est possible** et expose les jetons dans l'URL  
- [ ] Oui — les flux hybrides ou des valeurs de `response_type` non autorisées sont **activés** et traités  

### Les jetons d'accès ou codes d'autorisation sont-ils fuis via les en-têtes Referer ou l'historique du navigateur ?
- [ ] Non — les jetons/codes sont gérés de manière sécurisée et les pages sensibles utilisent une politique de référent (`Referrer-Policy`) appropriée  
- [ ] Oui — les jetons/codes **sont** fuis vers des domaines tiers via l'en-tête `Referer`  
- [ ] Oui — les jetons/codes **sont** visibles dans l'historique du navigateur en raison d'une mise en cache non sécurisée ou de requêtes GET  

### L'application applique-t-elle le principe du « Moindre Privilège » pour les étendues (Scopes) OAuth ?
- [ ] Oui — l'application ne demande que les privilèges (Scopes) minimaux nécessaires à la fonctionnalité  
- [ ] Non — l'application demande par défaut des privilèges excessifs ou administratifs  
- [ ] Non — l'utilisateur **ne peut pas** examiner ou refuser des privilèges spécifiques lors de la phase de consentement  

---