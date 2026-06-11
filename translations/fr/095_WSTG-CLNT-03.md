## WSTG-CLNT-03 — Injection HTML

L'Injection HTML (HTML Injection) se produit lorsqu'une application ne parvient pas à assainir les entrées fournies par l'utilisateur avant de les restituer dans le navigateur, permettant à un attaquant d'injecter des balises HTML arbitraires dans le Document Object Model (DOM) de la page web. Bien qu'elle soit similaire au Cross-Site Scripting (XSS), l'objectif principal de l'Injection HTML est de manipuler la présentation visuelle de la page ou de faciliter des attaques d'hameçonnage (Phishing) en injectant des formulaires malveillants et du contenu trompeur. Les attaquants ciblent les paramètres qui sont réfléchis dans l'UI (User Interface), tels que les termes de recherche, les champs de profil utilisateur ou les messages d'erreur, pour inciter les victimes à divulguer des identifiants sensibles ou à interagir avec des liens tiers non autorisés. Cette vulnérabilité représente un risque significatif pour l'intégrité de l'interface utilisateur de l'application et peut être le précurseur d'attaques d'ingénierie sociale (Social Engineering) ou de sessions plus complexes.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CLNT-03 |
| **CWE** | CWE-80 |
| **Statut du Test** | Non effectué |
| **Gravité** | Faible / Moyenne |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/03-Testing_for_HTML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite`, `OWASP ZAP`, `DOM Invader`, `Wfuzz`

### Les entrées contrôlées par l'utilisateur sont-elles réfléchies dans le DOM sans un encodage HTML approprié ?
- [ ] Non — toutes les entrées réfléchies sont strictement encodées en HTML avant d'être rendues  
- [ ] Oui — certains caractères **sont** encodés mais des contournements (bypasses) pour certaines balises **sont possibles**  
- [ ] Oui — l'entrée est réfléchie brute, et l'injection HTML **est possible**  

### Des éléments HTML structurels peuvent-ils être injectés pour modifier la mise en page ?
- [ ] Non — l'application filtre ou supprime les balises structurelles comme `<div>`, `<table>`, ou `<iframe>`  
- [ ] Oui — des éléments structurels **peuvent** être injectés mais n'ont **pas** d'impact significatif sur l'UI  
- [ ] Oui — une défiguration (defacement) significative de l'UI **est possible** via les balises injectées  

### Est-il possible d'injecter des éléments d'hameçonnage fonctionnels comme des formulaires ou des liens malveillants ?
- [ ] Non — les balises `<form>`, `<input>`, et `<a>` sont efficacement bloquées ou assainies  
- [ ] Oui — des liens malveillants **peuvent** être injectés pour rediriger les utilisateurs vers des sites externes  
- [ ] Oui — des éléments `<form>` fonctionnels **peuvent** être injectés pour collecter des identifiants *(Moyenne)*  

### Les en-têtes de sécurité ou les Content Security Policies (CSP) atténuent-ils l'impact de l'injection ?
- [ ] Oui — une CSP restrictive est en place et `form-action` ou `frame-src` **est appliqué**  
- [ ] No — des en-têtes de sécurité sont présents mais la CSP **est désactivée** ou contient des directives unsafe-inline/wildcards  
- [ ] No — aucune CSP ou en-tête de sécurité pertinent **n'est activé**  

---