## WSTG-CONF-06 — Test des Méthodes HTTP

Le test des méthodes HTTP consiste à identifier les verbes supportés par le serveur web et l'application afin de s'assurer que seules les fonctionnalités nécessaires sont exposées. Au-delà des méthodes standards `GET` et `POST`, les serveurs peuvent activer par inadvertance des méthodes dangereuses telles que `PUT`, `DELETE` ou `TRACE`, qui peuvent permettre à des attaquants de télécharger des fichiers malveillants, de supprimer du contenu existant ou d'exfiltrer des cookies de session via le Cross-Site Tracing (XST). Les pentesteurs examinent les en-têtes de réponse tels que `Allow` ou `Public` et tentent de contourner les restrictions en utilisant des techniques de substitution de méthode (Method Overriding) ou de falsification de verbes (Verb Tampering) pour accéder à des fonctionnalités non autorisées. Ce test est crucial pour le durcissement de la surface d'attaque et la prévention des mauvaises configurations pouvant mener à un compromis du serveur ou à une manipulation non autorisée des données.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CONF-06 |
| **CWE** | CWE-650 |
| **Statut du Test** | Non réalisé |
| **Sévérité** | Faible / Moyenne* |

> *La sévérité devient Élevée si `PUT` ou `DELETE` permettent une manipulation de fichiers non autorisée ou si `TRACE` facilite l'exfiltration de jetons de session.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/06-Test_HTTP_Methods  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `curl`, `nmap`, `Burp Suite (Repeater)`, `ZAP`, `Metasploit`

### Les méthodes HTTP dangereuses comme `PUT` ou `DELETE` sont-elles désactivées sur l'ensemble de l'application ?
- [ ] Oui — seules les méthodes sûres (GET, POST, HEAD) **sont activées**  
- [ ] Non — `PUT` ou `DELETE` **sont activées** mais nécessitent une authentification valide  
- [ ] Non — `PUT` ou `DELETE` **sont activées** et accessibles sans authentification *(Critique)*  

### La méthode `TRACE` est-elle désactivée pour empêcher le Cross-Site Tracing (XST) ?
- [ ] Oui — la méthode `TRACE` **est désactivée** ou renvoie un code 405 Method Not Allowed  
- [ ] Non — la méthode `TRACE` **est activée** mais ne reflète pas les en-têtes sensibles  
- [ ] Non — la méthode `TRACE` **est activée** et reflète les en-têtes `Cookie` ou `Authorization` *(Moyenne)*  

### Le contournement de méthode HTTP (ex : `X-HTTP-Method-Override`) est-il supporté par le serveur ?
- [ ] Non — le serveur **ne traite pas** les en-têtes de substitution de méthode  
- [ ] Oui — le serveur traite les en-têtes de substitution mais les contrôles d'accès **sont appliqués**  
- [ ] Oui — le serveur traite les en-têtes de substitution et un contournement du contrôle d'accès **est possible**  

### Le serveur répond-il de manière sécurisée aux méthodes HTTP arbitraires ou malformées ?
- [ ] Oui — le serveur renvoie un code 405 Method Not Allowed ou 501 Not Implemented  
- [ ] Non — le serveur renvoie un code 200 OK ou 500 Error pour des méthodes arbitraires comme `JEFF` ou `TEST`  
- [ ] Non — l'utilisation de méthodes arbitraires permet de contourner les filtres d'authentification ou d'autorisation  

### La méthode `OPTIONS` est-elle restreinte ou configurée pour minimiser la divulgation d'informations ?
- [ ] Oui — `OPTIONS` est désactivée ou renvoie un en-tête `Allow` minimal  
- [ ] Non — `OPTIONS` révèle une large gamme de méthodes activées, y compris des verbes DAV ou de débogage  

---