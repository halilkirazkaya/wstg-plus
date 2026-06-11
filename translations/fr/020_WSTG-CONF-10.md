## WSTG-CONF-10 — Subdomain Takeover

Le Subdomain Takeover (prise de contrôle de sous-domaine) se produit lorsqu'une entrée DNS (généralement un CNAME, mais occasionnellement des enregistrements A ou MX) pointe vers une ressource déclassée ou inexistante chez un fournisseur cloud tiers ou un service d'hébergement. Les attaquants identifient ces enregistrements DNS "orphelins" (dangling) en recherchant des messages d'erreur spécifiques provenant de fournisseurs tels qu'AWS, GitHub ou Azure, indiquant qu'une ressource n'est plus revendiquée. En enregistrant le nom de la ressource abandonnée sur la plateforme du fournisseur, l'attaquant gagne le contrôle total du sous-domaine, ce qui lui permet d'héberger du contenu malveillant, d'exfiltrer des cookies de session définis au niveau du domaine de base et de contourner les protections Content Security Policy (CSP) ou CORS. Cette vulnérabilité est particulièrement dangereuse car elle exploite la confiance associée à la structure de domaine légitime de l'organisation pour faciliter le phishing et le vol de données à fort impact.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CONF-10 |
| **CWE** | CWE-1329 |
| **Statut du test** | Non effectué |
| **Gravité** | Élevée / Critique* |

> *La gravité devient Critique si le sous-domaine peut être utilisé pour détourner des cookies de session du domaine de base ou contourner les redirections SSO/OAuth.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/10-Test_for_Subdomain_Takeover  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://github.com/EdOverflow/can-i-take-over-xyz  

**Outils :** `Subjack`, `SubOver`, `dnscan`, `Amass`, `Nuclei`, `dig`, `nslookup`

### L'application utilise-t-elle des services d'hébergement tiers ou des services cloud via des sous-domaines ?
- [ ] Non — tous les sous-domaines pointent vers un espace IP contrôlé par l'organisation  
- [ ] Oui — les sous-domaines pointent vers des fournisseurs tiers (S3, GitHub Pages, Heroku, etc.)  

### Existe-t-il des enregistrements DNS "orphelins" (dangling) pointant vers des ressources inexistantes ?
- [ ] Non — tous les enregistrements DNS identifiés se résolvent vers des ressources actives et contrôlées  
- [ ] Oui — des enregistrements CNAME ou A existent pour des ressources qui **n'existent plus** chez le fournisseur  
- [ ] Oui — les enregistrements DNS se résolvent en NXDOMAIN ou en pages d'erreur spécifiques au fournisseur *(ex: "NoSuchBucket")*  

### La ressource orpheline identifiée peut-elle être revendiquée avec succès ?
- [ ] Non — le fournisseur dispose de protections contre la revendication de noms abandonnés ou une vérification de compte est requise  
- [ ] Oui — le nom de la ressource **peut** être enregistré sur la plateforme tierce par n'importe quel utilisateur  

### Le domaine de base utilise-t-il des cookies avec une portée "Domain" qui pourraient être compromis ?
- [ ] Non — les cookies sont strictement limités à des sous-domaines spécifiques ou utilisent des flags `Host-Only`  
- [ ] Oui — des cookies sensibles (ID de session, JWT) sont définis pour le domaine parent et **peuvent** être exfiltrés via un sous-domaine détourné  

### Le sous-domaine peut-il être utilisé pour contourner des en-têtes de sécurité ou des flux d'authentification ?
- [ ] Non — aucune dépendance de sécurité sur les sous-domaines identifiés  
- [ ] Oui — le sous-domaine est en liste blanche dans les directives CSP `script-src` ou `connect-src`  
- [ ] Oui — le sous-domaine est une URI de redirection valide pour les configurations OAuth ou SSO  

---