## WSTG-SESS-03 — Session Fixation

La fixation de session (Session Fixation) se produit lorsqu'une application ne parvient pas à invalider ou à renouveler l'identifiant de session après qu'un utilisateur s'est authentifié avec succès, permettant ainsi à un attaquant d'imposer un jeton de session connu à une victime. Si l'application conserve l'ID de session pré-authentification après la connexion, un attaquant peut fournir à une victime un lien spécifiquement conçu contenant un ID de session fixe et, par la suite, détourner la session authentifiée. Cette vulnérabilité se manifeste généralement dans les formulaires de connexion ou via des identifiants de session acceptés par des paramètres d'URL et des cookies. Du point de vue de l'attaquant, l'exploitation consiste à « fixer » la session via un lien malveillant ou une vulnérabilité d'injection d'en-tête (Header Injection) et à attendre que la victime fournisse ses identifiants, contournant ainsi la nécessité de voler un jeton (token) actif.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-SESS-03 |
| **CWE** | CWE-384 |
| **État du test** | Non effectué |
| **Sévérité** | Haute |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/03-Testing_for_Session_Fixation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite (Proxy/Repeater)`, `OWASP ZAP`, `EditThisCookie`, `Curl`

### L'identifiant de session change-t-il après une authentification réussie ?
- [ ] Oui — un nouvel identifiant de session **est émis** et l'ancien est invalidé *(Plus sécurisé)*  
- [ ] Oui — un nouvel identifiant de session **est émis** mais l'ancien **reste valide**  
- [ ] Non — l'identifiant de session **reste le même** avant et après l'authentification *(Critique)*  

### L'application accepte-t-elle les identifiants de session fournis via des paramètres d'URL ?
- [ ] Non — les IDs de session sont gérés **exclusivement** via des cookies  
- [ ] Oui — les IDs de session sont acceptés dans l'URL mais sont **ignorés** ou **renouvelés** lors de la connexion  
- [ ] Oui — les IDs de session dans l'URL sont **acceptés** et **conservés** après l'authentification  

### Un ID de session défini par l'attaquant peut-il être imposé à l'application ?
- [ ] Non — l'application **rejette** les IDs de session invalides ou inexistants fournis par l'utilisateur  
- [ ] Oui — l'application **accepte** et initialise une session en utilisant n'importe quel ID arbitraire fourni dans un cookie  
- [ ] Oui — l'application **accepte** et initialise une session en utilisant n'importe quel ID arbitraire fourni dans un paramètre d'URL  

### Les sessions pré-authentification sont-elles correctement terminées ?
- [ ] Oui — la session anonyme est **entièrement détruite** lors de l'événement de connexion  
- [ ] Non — la session anonyme n'est **pas détruite**, permettant une récolte potentielle de sessions  

### Le cookie de session est-il protégé contre l'injection côté client ?
- [ ] Oui — les flags `HttpOnly` et `Secure` sont **appliqués** pour empêcher la fixation basée sur des scripts  
- [ ] Non — les flags ne sont **pas appliqués**, permettant de définir les IDs de session via Cross-Site Scripting (XSS)  

---