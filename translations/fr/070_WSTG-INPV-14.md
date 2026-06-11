## WSTG-INPV-14 — Testing for Incubated Vulnerabilities

Les vulnérabilités incubées, également appelées vulnérabilités persistantes ou de second ordre, surviennent lorsqu'un Payload malveillant est stocké par l'application dans un état « froid » (cold state), puis exécuté ou traité ultérieurement dans un contexte différent. Ce modèle d'attaque cible généralement les systèmes Back-end, les consoles d'administration ou les outils de reporting automatisés qui récupèrent des données à partir d'une base de données partagée ou d'un système de fichiers sans re-validation adéquate ou Output Encoding. Les Pentesters doivent tracer le cycle de vie de l'entrée utilisateur depuis le point d'entrée (l'« incubateur ») jusqu'à chaque emplacement où ces données sont finalement rendues ou utilisées par d'autres composants ou applications secondaires. Une exploitation réussie entraîne souvent des conséquences à fort impact, telles que le détournement de session administrative via un Stored XSS ou l'exfiltration de données par une Second-order SQL Injection dans des modules de reporting internes.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INPV-14 |
| **CWE** | CWE-20 |
| **État du test** | Non réalisé |
| **Sévérité** | Élevée |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/14-Testing_for_Incubated_Vulnerability  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html#second-order-injection  
* https://portswigger.net/web-security/web-cache-deception  
* https://portswigger.net/web-security/web-cache-poisoning  

**Outils :** `Burp Suite (Collaborator)`, `sqlmap`, `OWASP ZAP`, `KXSs`, `Dalfox`

### Existe-t-il des points de terminaison où les entrées contrôlées par l'utilisateur sont stockées pour une récupération ultérieure par d'autres utilisateurs ou processus ?
- [ ] Non — l'application ne stocke **pas** les entrées utilisateur pour une utilisation ultérieure  
- [ ] Oui — les entrées **sont** stockées dans des champs de base de données, des fichiers journaux ou des paramètres de configuration  

### Une validation des entrées ou une sanitisation est-elle appliquée avant que les données ne soient écrites dans la couche de stockage persistant ?
- [ ] Oui — une validation stricte par Allow-list **est appliquée** au point d'entrée  
- [ ] Oui — une sanitisation générique **est appliquée** mais un contournement **est possible** via l'encodage ou des caractères non standards  
- [ ] Non — l'entrée est stockée sous sa forme brute, non validée  

### Les données stockées sont-elles correctement encodées ou sanitisées lorsqu'elles sont rendues dans des interfaces administratives ou secondaires ?
- [ ] Oui — un Output Encoding sensible au contexte **est appliqué** partout où les données sont rendues  
- [ ] Oui — l'encodage **est appliqué** à certains endroits mais **manquant** à d'autres (par exemple, tableau de bord d'administration interne)  
- [ ] Non — les données sont rendues sans aucun encodage ni sanitisation, permettant l'exécution du Payload  

### Les payloads stockés dans la base de données peuvent-ils affecter les processus batch en Back-end ou les systèmes de surveillance Out-of-band ?
- [ ] Non — les processus Back-end utilisent des méthodes de parsing sûres ou des requêtes paramétrées  
- [ ] Oui — les payloads stockés **peuvent** déclencher des interactions dans la logique Back-end (par exemple, CSV Injection, Command Injection dans les logs)  
- [ ] Oui — les payloads stockés **peuvent** déclencher des requêtes Out-of-band (OOB) vers des serveurs contrôlés par l'attaquant  

---