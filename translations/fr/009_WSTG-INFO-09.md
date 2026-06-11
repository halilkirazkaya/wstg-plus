## WSTG-INFO-09 — Identification de l'empreinte de l'application Web (Fingerprint)

L'identification de l'empreinte d'une application web (Fingerprinting) est le processus consistant à identifier le framework spécifique, le système de gestion de contenu (CMS) ou la pile technologique sous-jacente ainsi que ses versions associées. Cette phase de reconnaissance est vitale car elle permet à un attaquant de mettre en correspondance les composants identifiés avec des vulnérabilités connues (CVEs) et des exploits publics, réduisant ainsi considérablement la surface d'attaque. Les informations sont généralement recueillies à partir des en-têtes de réponse HTTP, des chemins de fichiers uniques, des noms de cookies, des métadonnées du code source HTML et des hachages cryptographiques des ressources statiques (assets). En déterminant avec précision la pile technologique, un attaquant peut passer d'un balayage automatisé générique à une exploitation hautement ciblée des faiblesses spécifiques à une version.


| Champ | Valeur |
|---|---|
| **ID WSTG** | WSTG-INFO-09 |
| **CWE** | CWE-200 |
| **Statut du Test** | Non effectué |
| **Sévérité** | Faible / Informationnel |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/09-Fingerprint_Web_Application  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Wappalyzer`, `WhatWeb`, `BuiltWith`, `CMSmap`, `nmap`, `Nikto`, `Burp Suite`

### Les en-têtes de réponse HTTP divulguent-ils la pile technologique ou les numéros de version ?
- [ ] Non — les en-têtes sensibles sont supprimés ou contiennent des valeurs génériques  
- [ ] Oui — des en-têtes par défaut comme `X-Powered-By`, `Server` ou `X-AspNet-Version` **sont** présents  
- [ ] Oui — les en-têtes divulguent des versions spécifiques et obsolètes du serveur web ou du framework applicatif  

### Le framework applicatif ou le CMS est-il identifiable via des chemins de fichiers uniques ou des conventions de nommage ?
- [ ] Non — les chemins de fichiers sont obfusqués et ne révèlent **pas** la technologie sous-jacente  
- [ ] Oui — des structures de répertoires courantes (ex. `/wp-content/`, `/_next/`, `/node_modules/`) **sont** visibles  
- [ ] Oui — des fichiers uniques comme `robots.txt`, `humans.txt` ou `sitemap.xml` contiennent des métadonnées spécifiques à la technologie  

### Des numéros de version spécifiques sont-ils exposés dans le code source HTML ou les ressources statiques ?
- [ ] Non — aucune information de version n'est trouvée dans les commentaires ou les paramètres des ressources  
- [ ] Oui — des commentaires HTML ou des balises meta (ex. `<meta name="generator" content="...">`) **sont** présents  
- [ ] Oui — des chaînes de version sont ajoutées aux noms de fichiers CSS/JS (ex. `jquery.js?v=1.12.4`) et ne **peuvent pas** être désactivées  

### Les pages d'erreur par défaut ou les interfaces d'administration révèlent-elles l'environnement logiciel ?
- [ ] Non — l'application utilise des pages d'erreur personnalisées et génériques pour tous les codes d'état  
- [ ] Oui — les pages d'erreur par défaut du serveur web ou du framework **sont** activées et divulguent des données de version  
- [ ] Oui — les portails de connexion administratifs (ex. `/wp-admin`, `/phpmyadmin`) **sont** accessibles et identifiables  

### Les cookies révèlent-ils le framework de gestion de session ?
- [ ] Non — les cookies de session utilisent des noms génériques ou personnalisés  
- [ ] Oui — des noms de cookies spécifiques à la technologie (ex. `PHPSESSID`, `JSESSIONID`, `ASPSESSIONID`) **sont** utilisés  

---