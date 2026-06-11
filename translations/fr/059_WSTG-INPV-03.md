## WSTG-INPV-03 — Testing for HTTP Verb Tampering

Le HTTP Verb Tampering (manipulation de verbe HTTP) exploite les faiblesses dans la manière dont les serveurs web et les frameworks d'application autorisent l'accès à des ressources spécifiques en fonction de la méthode HTTP utilisée. Les attaquants tentent de contourner les contraintes de sécurité en substituant des méthodes standards comme `GET` ou `POST` par des alternatives telles que `HEAD`, `PUT`, `OPTIONS`, ou même des chaînes de caractères arbitraires et non-standards que le backend pourrait traiter de manière incohérente. Cette vulnérabilité survient généralement lorsque les configurations de sécurité — telles que les filtres web.xml de Java EE ou les règles d'autorisation .NET — énumèrent explicitement les méthodes autorisées mais ne parviennent pas à prendre en compte les autres, ou lorsqu'une approche "deny-by-method" (refus par méthode) est utilisée au lieu d'un "deny-all" (tout refuser). Une exploitation réussie peut entraîner un accès non autorisé à des fonctions d'administration, une modification de données ou une divulgation d'informations concernant la configuration interne du serveur.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INPV-03 |
| **CWE** | CWE-288 |
| **Statut du test** | Non effectué |
| **Sévérité** | Moyenne / Haute* |

> *La sévérité devient Haute si des fonctions administratives ou des modifications de données (PUT/DELETE) peuvent être effectuées sans authentification.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/03-Testing_for_HTTP_Verb_Tampering  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite (Repeater/Intruder)`, `curl`, `nmap`, `ffuf`

### L'application répond-elle à des méthodes HTTP non-standards ou arbitraires ?
- [ ] Non — l'application rejette toutes les méthodes à l'exception de celles explicitement requises  
- [ ] Oui — l'application répond aux méthodes standards (OPTIONS, TRACE) mais **ne peut pas** contourner la sécurité  
- [ ] Oui — l'application accepte des chaînes arbitraires (ex : FOO, TEST) et les traite comme des requêtes `GET` ou `POST`  

### L'authentification/autorisation peut-elle être contournée en modifiant la méthode HTTP ?
- [ ] Non — les contrôles de sécurité sont appliqués globalement quel que soit le verbe HTTP  
- [ ] Oui — des contrôles sont en place mais un contournement **est possible** en utilisant la méthode `HEAD`  
- [ ] Oui — les contrôles de sécurité ne sont **pas appliqués** aux méthodes alternatives, permettant un accès non autorisé aux points de terminaison (endpoints) restreints  

### Des méthodes dangereuses telles que `PUT` ou `DELETE` sont-elles activées sur le serveur ?
- [ ] Non — les méthodes dangereuses sont **désactivées** ou renvoient un code 405 Method Not Allowed  
- [ ] Oui — les méthodes sont activées mais nécessitent une authentification valide avec des privilèges élevés  
- [ ] Oui — les méthodes sont **activées** et permettent le téléversement de fichiers non autorisé ou la suppression de ressources *(Critique)*  

### La méthode `OPTIONS` révèle-t-elle des informations sensibles sur les verbes autorisés ?
- [ ] Non — `OPTIONS` est **désactivée** ou renvoie une réponse générique  
- [ ] Oui — `OPTIONS` renvoie l'en-tête `Allow` mais n'expose pas de méthodes internes restreintes  
- [ ] Oui — `OPTIONS` révèle des méthodes internes uniquement ou administratives qui aident à une exploitation ultérieure  

### La méthode `TRACE` est-elle activée, permettant potentiellement du Cross-Site Tracing (XST) ?
- [ ] Non — les méthodes `TRACE` et `TRACK` sont **désactivées**  
- [ ] Oui — `TRACE` est **activée** et renvoie les en-têtes HTTP, y compris les cookies/jetons sensibles  

---