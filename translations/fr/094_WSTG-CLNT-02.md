## WSTG-CLNT-02 — Test de l'exécution de JavaScript

Le test d'exécution JavaScript se concentre sur l'identification des cas où une application traite de manière incorrecte les données fournies par l'utilisateur, entraînant l'exécution de scripts non autorisés dans le contexte du navigateur de la victime. Cette vulnérabilité se traduit généralement par un Cross-Site Scripting (XSS), qui permet aux attaquants d'exfiltrer des cookies de session, de manipuler le Document Object Model (DOM) ou d'effectuer des actions au nom de l'utilisateur. Les testeurs d'intrusion examinent les points de sortie (sinks) tels que `innerHTML`, `document.write()` et `eval()` où des données non assainies provenant de sources telles que les fragments d'URL, le `localStorage` ou `window.name` peuvent être traitées. Une exploitation réussie compromet l'intégrité et la confidentialité de la session de l'utilisateur et peut servir de pivot pour des attaques côté client plus complexes.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CLNT-02 |
| **CWE** | CWE-79 |
| **État du test** | Non réalisé |
| **Sévérité** | Élevée |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/02-Testing_for_JavaScript_Execution  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/dom-based  
* https://portswigger.net/web-security/prototype-pollution  

**Outils :** `Burp Suite`, `Browser Developer Tools`, `XSStrike`, `DOMPurify`, `KNOXSS`

### L'application traite-t-elle des données fournies par l'utilisateur dans des points de sortie (sinks) JavaScript dangereux ?
- [ ] Non — toutes les données sont assainies ou utilisées dans des points de sortie sécurisés comme `textContent`  
- [ ] Oui — les données sont traitées dans des points de sortie dangereux mais l'assainissement ou l'encodage **est appliqué**  
- [ ] Oui — les données sont traitées dans des points de sortie dangereux et l'assainissement **n'est pas appliqué**  

### Une Content Security Policy (CSP) est-elle présente pour atténuer l'exécution de scripts ?
- [ ] Oui — une CSP restrictive est **activée** et aucun contournement **n'est possible**  
- [ ] Oui — une CSP est **activée** mais un contournement **est possible** via `unsafe-inline` ou des CDN non sécurisés  
- [ ] Non — aucune CSP n'est **activée**  

### Le JavaScript peut-il être exécuté via des paramètres d'URL ou des fragments (DOM-based XSS) ?
- [ ] Non — l'entrée est correctement encodée et l'exécution **n'est pas possible**  
- [ ] Oui — l'entrée est reflétée dans le DOM mais l'exécution **est empêchée** par les protections des frameworks modernes  
- [ ] Oui — l'entrée est directement exécutée dans le navigateur et l'exploitation **est possible**  

### Les gestionnaires d'événements (event handlers) ou les schémas URI sont-ils vulnérables à l'injection de scripts ?
- [ ] Non — l'entrée est assainie pour supprimer les attributs d'événement et les schémas `javascript:`  
- [ ] Oui — les gestionnaires d'événements **peuvent** être injectés mais des filtres limitent la complexité du payload  
- [ ] Oui — l'exécution de JavaScript arbitraire via les attributs `onerror`, `onload` ou `href` **est possible**  

---