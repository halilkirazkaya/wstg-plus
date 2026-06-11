## WSTG-CLNT-04 — Test de redirection d'URL côté client (Client-Side URL Redirect)

Une redirection d'URL côté client se produit lorsqu'une application web accepte une entrée contrôlée par l'utilisateur—généralement via des paramètres d'URL ou des fragments de hachage—et l'utilise au sein d'un récepteur (sink) de redirection côté client tel que `window.location` ou `document.location.href`. Cette vulnérabilité est principalement exploitée par des attaquants pour mener des campagnes de phishing sophistiquées, car la victime fait initialement confiance au domaine légitime avant d'être redirigée silencieusement vers un site malveillant. Au-delà du phishing, les redirections côté client peuvent souvent être combinées pour exfiltrer des données sensibles telles que des jetons d'accès OAuth (OAuth access tokens) ou des identifiants de session si la redirection se produit alors que ces valeurs sont présentes dans l'URL ou dans l'en-tête `Referer`. Dans les Single Page Applications (SPA) modernes, cette vulnérabilité apparaît fréquemment dans la logique de routage, les paramètres "return to" après l'authentification ou les fonctionnalités de changement de langue.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CLNT-04 |
| **CWE** | CWE-601 |
| **État du Test** | Non effectué |
| **Sévérité** | Moyenne* |

> *La sévérité devient Élevée si la redirection peut être utilisée pour exfiltrer des jetons OAuth, des identifiants de session, ou pour contourner des contrôles de sécurité dans un flux d'authentification.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/04-Testing_for_Client-side_URL_Redirect  
* https://hacktricks.wiki/en/pentesting-web/open-redirect.html  

**Outils :** `Burp Suite (DOM Invader)`, `Browser DevTools`, `LinkFinder`, `Arjun`, `B-Config`

### L'application utilise-t-elle des scripts côté client pour gérer les redirections basées sur les entrées utilisateur ?
- [ ] Non — la redirection est gérée exclusivement côté serveur via des codes d'état HTTP `3xx`  
- [ ] Oui — l'application utilise des récepteurs (sinks) JavaScript tels que `window.location` pour naviguer en fonction des paramètres d'URL  
- [ ] Oui — l'application utilise un routeur de framework côté client (ex: React Router, Vue Router) qui traite des chemins fournis par l'utilisateur  

### Des contrôles de validation d'entrée ou de liste blanche (whitelisting) sont-ils implémentés pour l'URL de destination ?
- [ ] Oui — une **liste blanche** (whitelist) stricte de domaines/chemins autorisés est appliquée et le contournement (bypass) n'est **pas possible**  
- [ ] Oui — une validation est présente mais repose sur des regex faibles ou des listes noires (blacklists), et le contournement **est possible**  
- [ ] Non — l'application accepte n'importe quelle chaîne arbitraire comme cible de redirection  

### La logique de redirection peut-elle être contournée en utilisant des techniques d'offuscation courantes ?
- [ ] Non — les contrôles gèrent correctement les caractères encodés, les URL relatives au protocole (`//`) et les barres obliques inverses (backslashes)  
- [ ] Oui — le contournement est **possible** via l'encodage URL ou le double-encodage  
- [ ] Oui — le contournement est **possible** en utilisant des URL relatives au protocole ou des caractères `@` (ex: `https://expected.com@attacker.com`)  
- [ ] Oui — le contournement est **possible** en exploitant des comportements spécifiques au navigateur ou une injection d'octet nul (null byte injection)  

### Le processus de redirection expose-t-il des données sensibles au site de destination ?
- [ ] Non — aucune donnée sensible n'est présente dans l'URL ni transmise via les en-têtes pendant la redirection  
- [ ] Oui — des données sensibles (ex: jetons de session, clés API) **sont divulguées** via l'en-tête `Referer` au site externe  
- [ ] Oui — les données sensibles présentes dans le fragment d'URL (`#`) **sont accessibles** aux scripts du site de destination  

### Est-il possible d'exécuter du JavaScript via le récepteur de redirection (DOM-based XSS) ?
- [ ] Non — le récepteur (sink) autorise uniquement la navigation vers les protocoles `http` ou `https`  
- [ ] Oui — le récepteur de redirection autorise les URI `javascript:` ou `data:`, rendant une vulnérabilité Cross-Site Scripting (XSS) basée sur le DOM **possible**  

---