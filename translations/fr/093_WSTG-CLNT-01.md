## WSTG-CLNT-01 — Test du Cross-Site Scripting basé sur le DOM

Le Cross-Site Scripting basé sur le DOM (DOM XSS) se produit lorsqu'une application contient du JavaScript côté client qui traite des données provenant d'une source non fiable de manière non sécurisée, généralement en réinjectant les données dans le Document Object Model (DOM) via un sink dangereux. Contrairement au XSS réfléchi ou stocké, la vulnérabilité réside entièrement dans le code côté client ; le Payload n'est souvent pas envoyé au serveur, car il est traité par des sinks tels que `innerHTML`, `eval()`, ou `document.write()`. Les attaquants exploitent cela en créant des URL avec des fragments ou des paramètres malveillants qui, lorsqu'ils sont chargés par le navigateur d'une victime, exécutent du JavaScript arbitraire dans le contexte de la session de l'utilisateur. Cela peut mener à l'exfiltration de jetons de session (Session Tokens), au vol de données sensibles et à des actions non autorisées effectuées au nom de l'utilisateur authentifié.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CLNT-01 |
| **CWE** | CWE-79 |
| **État du test** | Non effectué |
| **Sévérité** | Élevée |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/01-Testing_for_DOM-based_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  
* https://portswigger.net/web-security/cross-site-scripting  
* https://portswigger.net/web-security/dom-based  

**Outils :** `Burp Suite (DOM Invader)`, `Browser DevTools`, `KNOXSS`, `XSStrike`, `Snyk (SAST)`

### Des sources non fiables sont-elles utilisées pour capturer des données contrôlées par l'utilisateur en JavaScript ?
- [ ] Non — JavaScript n'utilise pas `location.hash`, `location.search`, ou `document.referrer`  
- [ ] Oui — des sources non fiables sont utilisées mais les données ne sont transmises à aucun sink d'exécution  
- [ ] Oui — des sources non fiables sont utilisées et les données circulent directement dans la logique côté client  

### Les données contrôlées par l'utilisateur sont-elles transmises à des sinks DOM dangereux ?
- [ ] Non — les sinks tels que `innerHTML`, `outerHTML`, `document.write()`, ou `eval()` ne sont pas utilisés  
- [ ] Oui — des sinks dangereux sont présents mais les données sont strictement assainies (Sanitized) à l'aide d'une bibliothèque sécurisée comme `DOMPurify`  
- [ ] Oui — des sinks dangereux sont présents et les données sont traitées avec un filtrage par expressions régulières (Regex) insuffisant ou personnalisé  
- [ ] Oui — des sinks dangereux sont présents et les données sont transmises sans aucun assainissement ni encodage *(Critique)*  

### Le sink d'exécution peut-il être atteint via un Payload basé sur une URL ?
- [ ] Non — le flux de la source vers le sink ne peut pas être déclenché via une entrée externe  
- [ ] Oui — le flux est possible mais nécessite une interaction complexe de l'utilisateur  
- [ ] Oui — le flux est possible et peut être déclenché via un simple fragment d'URL ou un paramètre de requête (Query Parameter)  

### Des en-têtes de sécurité modernes ou des mesures d'atténuation basées sur le navigateur neutralisent-ils le risque ?
- [ ] Oui — une `Content-Security-Policy` (CSP) stricte avec `script-src` et `trusted-types` est activée et empêche l'exécution  
- [ ] Oui — une CSP est présente mais `unsafe-inline` ou des hachages (Hashes) faibles rendent un contournement (Bypass) possible  
- [ ] Non — aucune CSP n'est présente pour atténuer l'exécution basée sur le DOM  

### L'application utilise-t-elle des frameworks côté client avec des protections intégrées ?
- [ ] Oui — le framework (ex: Angular, React) encode automatiquement les données et aucun contournement n'est possible  
- [ ] Oui — le framework est utilisé mais des "escape hatches" comme `dangerouslySetInnerHTML` sont activés  
- [ ] Non — l'application utilise du JavaScript natif (Vanilla JS) ou des bibliothèques obsolètes (ex: anciennes versions de jQuery) sans auto-échappement (Auto-escaping)  

---