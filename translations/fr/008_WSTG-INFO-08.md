## WSTG-INFO-08 — Identification de l'empreinte du framework d'application Web (Fingerprint)

L'identification de l'empreinte (Fingerprinting) d'un framework d'application Web consiste à identifier la pile technologique spécifique et les versions logicielles utilisées pour construire et servir l'application. Les attaquants analysent les en-têtes de réponse HTTP, les cookies spécifiques au framework, les structures de fichiers prévisibles et les artefacts JavaScript uniques pour déterminer si la cible utilise des plateformes telles que React, Angular, Django ou Spring. Cette phase de reconnaissance est vitale car elle permet à un testeur de réduire la surface d'attaque aux vulnérabilités connues (CVE) et aux faiblesses de configuration courantes spécifiques à ce framework. En comprenant l'architecture sous-jacente, un attaquant peut passer de tests génériques à des stratégies d'exploitation hautement ciblées.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INFO-08 |
| **CWE** | CWE-200 |
| **Statut du test** | Non effectué |
| **Sévérité** | Informatif |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/08-Fingerprint_Web_Application_Framework  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Wappalyzer`, `BuiltWith`, `WhatWeb`, `nmap`, `Burp Suite`, `curl`

### Les en-têtes de réponse HTTP exposent-ils le nom ou la version du framework ?
- [ ] Non — les en-têtes tels que `X-Powered-By` ou `Server` ne sont **pas** présents ou sont assainis  
- [ ] Oui — le nom du framework est présent mais la version **est masquée**  
- [ ] Oui — le nom du framework et la version **sont tous deux exposés**  

### Les cookies spécifiques au framework ou les structures de répertoires sont-ils identifiables ?
- [ ] Non — les artefacts communs du framework (par ex., chemins `.js`, noms de cookies) sont offusqués ou renommés  
- [ ] Oui — les cookies spécifiques au framework (par ex., `JSESSIONID`, `PHPSESSID`, `csrftoken`) **sont** identifiables  
- [ ] Oui — les structures de répertoires (par ex., `/wp-content/`, `_next/static/`) **sont** visibles et confirment le framework  

### Les messages d'erreur ou les commentaires du code source divulguent-ils des détails sur le framework ?
- [ ] Non — la gestion des erreurs est générique et les commentaires sont supprimés  
- [ ] Oui — les traces de pile (stack traces) spécifiques au framework **sont activées** et visibles dans les réponses  
- [ ] Oui — le code source HTML contient des commentaires ou des balises de métadonnées identifiant le framework  

### Des vulnérabilités connues sont-elles associées à la version du framework identifiée ?
- [ ] Non — le framework identifié est à jour avec **aucune** vulnérabilité publique connue  
- [ ] Oui — la version du framework est obsolète et des CVE spécifiques **peuvent être** identifiés  

---