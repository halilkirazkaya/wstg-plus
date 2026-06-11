## WSTG-SESS-06 — Test de la fonctionnalité de déconnexion

La fonctionnalité de déconnexion est un contrôle de sécurité critique conçu pour mettre fin à la session d'un utilisateur et invalider les identifiants de session associés, tant du côté client que du côté serveur. Un échec dans l'implémentation correcte de la déconnexion permet des attaques de type Session Fixation ou Hijacking, car un attaquant accédant à une machine après qu'un utilisateur se soit "déconnecté" pourrait potentiellement réutiliser le jeton de session (Session Token) toujours actif. Les Pentesters évaluent cela en vérifiant si le cookie de session est effacé du navigateur, si l'état de la session côté serveur est explicitement détruit, et si la navigation via le bouton "retour" permet d'accéder à des données sensibles mises en cache. Une implémentation sécurisée de la déconnexion garantit qu'une fois l'action déclenchée, aucune requête ultérieure utilisant l'ancien jeton de session n'est autorisée par l'application.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-SESS-06 |
| **CWE** | CWE-613 |
| **État du Test** | Non réalisé |
| **Sévérité** | Moyenne |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/06-Testing_for_Logout_Functionality  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**Outils :** `Burp Suite (Repeater/Proxy)`, `Browser Developer Tools`, `OWASP ZAP`

### Un mécanisme de déconnexion fonctionnel est-il présent et accessible ?
- [ ] Oui — le bouton de déconnexion existe et déclenche une requête de terminaison  
- [ ] Non — le bouton de déconnexion est **manquant** ou ne **déclenche pas** d'événement de terminaison  

### L'identifiant de session est-il invalidé côté serveur ?
- [ ] Oui — le serveur rejette toutes les requêtes ultérieures utilisant l'ancien jeton de session  
- [ ] Non — le serveur **continue d'accepter** l'ancien jeton de session après la déconnexion  

### L'application efface-t-elle les cookies de session dans le navigateur lors de la déconnexion ?
- [ ] Oui — les cookies sont écrasés avec une date d'expiration passée ou une valeur vide  
- [ ] Oui — les cookies subsistent mais ne sont **plus valides** sur le serveur  
- [ ] Non — les cookies restent dans le navigateur et **conservent** leurs valeurs d'origine  

### Des contenus authentifiés sensibles peuvent-ils être consultés via le bouton 'Retour' du navigateur après la déconnexion ?
- [ ] Non — l'en-tête `Cache-Control: no-store` ou des en-têtes similaires empêchent la consultation de pages sensibles après la déconnexion  
- [ ] Oui — les pages sensibles sont mises en cache et **peuvent** être consultées par navigation après la terminaison de la session  

---