## WSTG-IDNT-04 — Test d'énumération de comptes et de comptes utilisateurs devinables

L'énumération de comptes se produit lorsqu'une application révèle l'existence ou l'inexistence d'un compte utilisateur à travers des variations dans les réponses HTTP, les messages d'erreur ou des différences de temps mesurables (timing differences). Les attaquants exploitent ce comportement en soumettant systématiquement des listes de noms d'utilisateur pour identifier les comptes valides, qui sont ensuite ciblés pour des attaques de type Credential Stuffing, Brute Force ou d'ingénierie sociale. Cette vulnérabilité se manifeste couramment dans les portails de connexion, les fonctionnalités de réinitialisation de mot de passe, les formulaires d'inscription et les points de terminaison d'API qui gèrent des identifiants spécifiques à l'utilisateur. Du point de vue d'un attaquant, une énumération réussie réduit considérablement la surface d'attaque en transformant une tentative de Brute Force aveugle en une exploitation ciblée d'identités connues et valides.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-IDNT-04 |
| **CWE** | CWE-204 |
| **État du test** | Non réalisé |
| **Sévérité** | Faible / Moyenne |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/04-Testing_for_Account_Enumeration_and_Guessable_User_Account  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite (Intruder/Comparer)`, `ffuf`, `Wfuzz`, `GoBuster`, `Hashcat`

### Les messages d'erreur d'authentification sont-ils cohérents dans tous les scénarios ?
- [ ] Oui — des messages d'erreur génériques sont utilisés à la fois pour les noms d'utilisateur invalides et les mots de passe invalides  
- [ ] Oui — les messages sont similaires, mais de légères variations de ponctuation ou de casse **sont présentes**  
- [ ] Non — les messages d'erreur indiquent explicitement "Utilisateur non trouvé" ou "Mot de passe incorrect" *(Vulnérable)*  

### Le flux de réinitialisation de mot de passe ou "mot de passe oublié" laisse-t-il fuiter l'existence d'un compte ?
- [ ] Non — l'application renvoie un message générique, que l'e-mail/nom d'utilisateur existe ou non  
- [ ] Oui — des contrôles sont en place, mais un contournement (bypass) **est possible** via la longueur de la réponse ou les codes d'état  
- [ ] Oui — l'application confirme explicitement si un e-mail de réinitialisation a été envoyé ou si le compte **n'existe pas**  

### Le processus d'inscription est-il protégé contre l'énumération d'utilisateurs ?
- [ ] Non — l'inscription ne laisse pas fuiter d'informations ou utilise des CAPTCHA/Rate Limiting pour empêcher les vérifications en masse  
- [ ] Oui — des contrôles sont en place, mais un contournement **est possible** via le timing ou les différences de réponse de l'API  
- [ ] Oui — l'application informe immédiatement l'utilisateur si le nom d'utilisateur ou l'e-mail **est déjà utilisé**  

### Des différences de timing sont-elles mesurables lors de la comparaison de noms d'utilisateur valides vs invalides ?
- [ ] Non — les temps de réponse sont cohérents quelle que soit la validité du compte grâce à des comparaisons en temps constant  
- [ ] Oui — des différences de timing existent mais sont trop faibles pour être mesurées de manière fiable sur le réseau  
- [ ] Oui — des divergences de timing mesurables **sont présentes** (par exemple, en raison de l'exécution du hachage du mot de passe uniquement pour les utilisateurs valides)  

### L'application utilise-t-elle des conventions de nommage prévisibles ou devinables pour les comptes ?
- [ ] Non — les identifiants de compte sont aléatoires (UUID) ou définis par l'utilisateur et complexes  
- [ ] Oui — les identifiants de compte suivent un modèle (par exemple, `emp001`, `emp002`) et l'énumération **est possible**  
- [ ] Oui — les identifiants sont des entiers séquentiels, rendant l'énumération complète de la base de données **possible**  

---