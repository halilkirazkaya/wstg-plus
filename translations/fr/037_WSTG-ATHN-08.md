## WSTG-ATHN-08 — Test des réponses aux questions de sécurité faibles

Le test des réponses aux questions de sécurité faibles consiste à évaluer les mécanismes d'authentification basée sur la connaissance (KBA - Knowledge-Based Authentication) utilisés pour vérifier l'identité d'un utilisateur, généralement lors des flux de récupération de mot de passe ou d'authentification multi-facteurs. Cela est important car les questions de sécurité reposent souvent sur des informations statiques et non secrètes qui peuvent être facilement découvertes via le renseignement d'origine source ouverte (OSINT) ou l'ingénierie sociale, menant à une prise de contrôle de compte (Account Takeover) non autorisée. Ces vulnérabilités surviennent généralement dans les modules « Mot de passe oublié » ou « Déverrouillage de compte » où les utilisateurs fournissent des réponses à des questions prédéfinies. Du point de vue d'un attaquant, il s'agit d'une cible de choix pour le Brute Force automatisé ou la recherche ciblée, car de nombreux utilisateurs fournissent des réponses prévisibles ou vérifiables publiquement à des questions courantes telles que « Quel est le nom de jeune fille de votre mère ? » ou « Quel lycée avez-vous fréquenté ? ».


| Champ | Valeur |
|---|---|
| **ID WSTG** | WSTG-ATHN-08 |
| **CWE** | CWE-640 |
| **Statut du test** | Non réalisé |
| **Sévérité** | Moyenne / Haute* |

> *La sévérité devient Haute si la question de sécurité est l'unique condition pour une réinitialisation complète du mot de passe et une prise de contrôle du compte sans vérification supplémentaire.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/08-Testing_for_Weak_Security_Question_Answer  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**Outils :** `Burp Suite (Intruder)`, `ffuf`, `Maltego`, `Sherlock`, `Social Engineering Toolkit (SET)`

### L'application implémente-t-elle des questions de sécurité pour la vérification de l'identité ou la récupération de mot de passe ?
- [ ] Non — les questions de sécurité ne sont **pas utilisées** pour l'authentification ou la récupération  
- [ ] Oui — les questions de sécurité sont **utilisées** comme facteur secondaire optionnel  
- [ ] Oui — les questions de sécurité sont **utilisées** comme mécanisme de récupération principal/unique *(Risque élevé)*  

### Les questions de sécurité sont-elles sélectionnées dans une liste fixe ou sont-elles définies par l'utilisateur ?
- [ ] Non — les questions sont entièrement définies par l'utilisateur et nécessitent une entropie élevée  
- [ ] Oui — les questions sont fixes mais incluent des options non découvrables par OSINT  
- [ ] Oui — les questions proviennent d'une liste fixe de sujets courants et découvrables par OSINT *(Vulnérable)*  

### Un Rate Limiting ou un verrouillage de compte est-il appliqué aux tentatives de réponse aux questions de sécurité ?
- [ ] Oui — un Rate Limiting strict et un verrouillage de compte **sont appliqués** pour empêcher le Brute Force  
- [ ] Oui — le Rate Limiting **est appliqué** mais un contournement **est possible** via la rotation d'IP ou la manipulation d'en-têtes  
- [ ] Non — **aucun Rate Limiting** n'est appliqué sur les tentatives de réponse  

### L'application impose-t-elle une complexité ou une validation sur le contenu des réponses ?
- [ ] Oui — la validation empêche les mots courants et garantit la complexité/l'unicité des réponses  
- [ ] Oui — une validation de base est présente mais **peut** être contournée avec des listes de mots courantes ou des attaques par dictionnaire  
- [ ] Non — **aucune validation** n'est effectuée ; les réponses simples ou à un seul caractère **sont autorisées**  

### Le défi de la question de sécurité peut-il être contourné par des failles logiques ?
- [ ] Non — le flux de récupération nécessite strictement une réponse valide pour continuer  
- [ ] Oui — le flux de récupération **peut** être contourné en manipulant les codes de réponse HTTP ou les paramètres de session  
- [ ] Oui — la réponse est reflétée dans le code source ou dans des champs cachés de la page  

---