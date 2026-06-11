## WSTG-INFO-02 — Fingerprint Web Server

Le fingerprinting (prise d'empreinte) de serveur Web est le processus d'identification du type de logiciel spécifique, de sa version et du système d'exploitation sous-jacent d'un serveur Web cible. Les attaquants effectuent cette reconnaissance pour identifier des vulnérabilités connues, telles que des CVE non corrigées ou des faiblesses de configuration, propres à certaines versions d'Apache, Nginx, IIS ou d'autres technologies de serveur. Cela est généralement réalisé en analysant les en-têtes de réponse HTTP (par exemple, `Server`, `X-Powered-By`), en examinant les comportements uniques des protocoles ou en observant les informations divulguées via les pages d'erreur et les fichiers par défaut. Un fingerprinting réussi permet à un attaquant d'adapter sa stratégie d'exploitation, passant d'un scan générique à des attaques à haute probabilité de succès contre des failles architecturales connues.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INFO-02 |
| **CWE** | CWE-200 |
| **État du Test** | Non réalisé |
| **Sévérité** | Informationnel / Faible* |

> *La sévérité devient Élevée si le fingerprinting révèle une version de serveur obsolète ou en fin de vie (EOL) présentant des vulnérabilités critiques connues.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/02-Fingerprint_Web_Server  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**Outils :** `curl`, `Nmap`, `WhatWeb`, `Wappalyzer`, `Nikto`, `Netcat`

### Des chaînes d'identification sont-elles présentes dans les en-têtes de réponse HTTP ?
- [ ] Non — les en-têtes `Server` et `X-Powered-By` ne sont **pas présents** ou contiennent des valeurs génériques  
- [ ] Oui — les en-têtes révèlent le logiciel du serveur mais **pas** les numéros de version spécifiques  
- [ ] Oui — les en-têtes révèlent le logiciel spécifique du serveur et les numéros de version **exacts**  

### Les pages d'erreur par défaut ou les réponses générées par le système divulguent-elles des détails sur le serveur ?
- [ ] Non — des pages d'erreur personnalisées sont utilisées et **ne divulguent pas** d'informations sur le serveur  
- [ ] Oui — les pages d'erreur par défaut divulguent le nom du logiciel serveur et/ou sa version  
- [ ] Oui — les pages d'erreur divulguent les détails du serveur, la version et le **système d'exploitation sous-jacent**  

### La version du serveur est-elle divulguée via des fichiers par défaut ou des structures de répertoires ?
- [ ] Non — les fichiers d'installation par défaut, les manuels et les scripts de test ont été **supprimés**  
- [ ] Oui — les fichiers par défaut (par ex. `info.php`, `manual/`, `test.html`) sont **présents** et divulguent des données sur la version  

### Le type de serveur peut-il être déduit avec précision via une analyse de comportement unique ?
- [ ] Non — les réponses du serveur sont normalisées et le fingerprinting basé sur le comportement n'est **pas possible**  
- [ ] Oui — des comportements uniques (par ex. l'ordre des en-têtes, des codes d'erreur spécifiques ou des particularités de la pile TCP/IP) **permettent** l'identification du serveur  

### La version du serveur identifiée est-elle actuellement supportée et corrigée ?
- [ ] Oui — la version identifiée est à jour et **supportée** par l'éditeur  
- [ ] Non — la version identifiée est **obsolète** ou en **fin de vie (EOL)** avec des vulnérabilités connues  

---