## WSTG-CLNT-11 — Test du Web Messaging

Le Web Messaging, ou `postMessage`, permet une communication cross-origin entre des objets window, tels qu'une page parente et une fenêtre contextuelle (popup) générée ou une iframe intégrée. Ce mécanisme est essentiel pour les fonctionnalités web modernes, mais il introduit un risque significatif si l'application réceptrice ne vérifie pas l'identité de l'expéditeur ou traite de manière incorrecte le payload du message. Les attaquants exploitent ces vulnérabilités en hébergeant un site malveillant qui intègre l'application cible et envoie des messages forgés pour déclencher des actions imprévues, exfiltrer des données sensibles ou exécuter un Cross-Site Scripting (XSS) basé sur le DOM. Une exploitation réussie se produit lorsque les écouteurs de messages (message listeners) ne valident pas strictement la propriété `origin` ou transmettent `message.data` directement dans des puits d'exécution (sinks) dangereux.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CLNT-11 |
| **CWE** | CWE-345 |
| **État du test** | Non réalisé |
| **Sévérité** | Moyen / Élevé |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/11-Testing_Web_Messaging  
* https://hacktricks.wiki/en/pentesting-web/postmessage-vulnerabilities/index.html  

**Outils :** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `PostMessage-Tracker`, `PMHook`

### L'application utilise-t-elle l'API `postMessage` pour la communication entre documents ?
- [ ] Non — les écouteurs `postMessage` ne sont **pas** présents dans le code côté client  
- [ ] Oui — `postMessage` est utilisé pour la communication entre composants internes  
- [ ] Oui — `postMessage` est utilisé pour communiquer avec des domaines ou des widgets tiers  

### L'origine des messages entrants est-elle strictement validée ?
- [ ] Oui — l'origine est vérifiée par rapport à une liste blanche (whitelist) stricte en utilisant `===` ou une comparaison identique *(Le plus sécurisé)*  
- [ ] Oui — l'origine est validée à l'aide d'une regex, mais la logique **est** susceptible d'être contournée (ex: `^https://trusted.com`)  
- [ ] Non — la propriété `origin` n'est **pas** vérifiée, permettant à n'importe quel domaine d'envoyer des messages  
- [ ] Non — un caractère générique `*` (wildcard) est utilisé dans le `targetOrigin` de l'expéditeur, fuyant potentiellement des données vers des écouteurs malveillants  

### Le payload du message est-il assaini avant d'être traité par l'application ?
- [ ] Oui — les données sont traitées comme du texte brut et aucun puits (sink) dangereux n'est utilisé  
- [ ] Oui — les données sont analysées (ex: `JSON.parse`) et validées avant utilisation, et aucun contournement n'est **possible**  
- [ ] Non — les données du message sont transmises directement dans des puits (sinks) dangereux tels que `innerHTML`, `eval()` ou `setTimeout()`  
- [ ] Non — les données du message sont utilisées pour naviguer dans la fenêtre ou modifier le `location.href` sans validation  

### Un attaquant peut-il déclencher des actions non autorisées ou exfiltrer des données via un message malveillant ?
- [ ] Non — même avec un message falsifié, aucune action ou donnée sensible ne peut être accédée  
- [ ] Oui — un message forgé **peut** déclencher des changements d'état sensibles (ex: changement de mot de passe, mise à jour du profil)  
- [ ] Oui — un message forgé **peut** entraîner un XSS basé sur le DOM ou l'exfiltration de jetons CSRF/données de session  

---