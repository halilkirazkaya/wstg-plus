## WSTG-CONF-08 — Test de la politique cross-domain RIA

Les politiques cross-domain des applications Internet riches (RIA) impliquent des fichiers tels que `crossdomain.xml` et `clientaccesspolicy.xml` qui définissent la manière dont les clients web—spécifiquement Adobe Flash et Microsoft Silverlight—gèrent les requêtes cross-origin. Ces fichiers sont critiques pour la sécurité car ils contournent explicitement la Same-Origin Policy (SOP), déterminant quels domaines externes sont autorisés à interagir avec l'application et à lire ses réponses. Des configurations trop permissives, comme l'utilisation de caractères génériques (wildcards) dans l'attribut `domain`, permettent à des attaquants d'héberger des objets RIA malveillants sur un site tiers pouvant exfiltrer des données sensibles ou effectuer des actions au nom d'utilisateurs authentifiés. Les pentesters localisent généralement ces fichiers à la racine du site web afin d'évaluer le niveau de confiance accordé aux origines tierces et d'identifier les fuites potentielles de données cross-origin.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CONF-08 |
| **CWE** | CWE-942 |
| **Statut du test** | Non effectué |
| **Sévérité** | Moyenne / Haute* |

> *La sévérité devient Haute si l'application manipule des données utilisateur sensibles ou des identifiants de session pouvant être exfiltrés via une politique permissive.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/08-Test_RIA_Cross_Domain_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `curl`, `Burp Suite`, `FFUF`, `dirsearch`, `Wget`

### Les fichiers de politique cross-domain (`crossdomain.xml` ou `clientaccesspolicy.xml`) sont-ils présents à la racine du site web ?
- [ ] Non — les fichiers ne peuvent pas être trouvés dans le répertoire racine  
- [ ] Oui — `crossdomain.xml` (Flash) est présent  
- [ ] Oui — `clientaccesspolicy.xml` (Silverlight) est présent  
- [ ] Oui — les deux fichiers de politique RIA sont présents  

### L'attribut de domaine `allow-access-from` est-il restreint à des origines de confiance ?
- [ ] Non — les fichiers existent mais ne contiennent aucune entrée `allow-access-from`  
- [ ] Oui — des domaines spécifiques et de confiance sont explicitement mis en liste blanche et aucun contournement n'est possible  
- [ ] Oui — des domaines sont mis en liste blanche mais incluent des tiers non fiables ou des sous-domaines  
- [ ] Oui — un caractère générique `*` est utilisé, permettant l'accès depuis n'importe quelle origine *(Critique)*  

### La politique autorise-t-elle des communications non sécurisées via HTTP pour des ressources HTTPS ?
- [ ] Non — `secure="true"` est défini (ou par défaut), empêchant les origines HTTP d'accéder aux données HTTPS  
- [ ] Oui — `secure="false"` est explicitement défini, permettant à des attaquants MITM d'exfiltrer des données sécurisées  

### Les en-têtes HTTP sont-ils trop permissifs dans la configuration cross-domain ?
- [ ] Non — `allow-http-request-headers-from` est restreint ou n'est pas activé  
- [ ] Oui — des en-têtes spécifiques sont autorisés pour des domaines de confiance  
- [ ] Oui — le caractère générique `*` est utilisé pour les en-têtes, permettant à des attaquants d'envoyer des en-têtes personnalisés depuis n'importe quel domaine  

### La configuration `site-control` (master-policy) est-elle restrictive ?
- [ ] Non — `permitted-cross-domain-policies` est défini sur `none` ou `master-only`  
- [ ] Oui — la politique autorise d'autres fichiers sur le serveur à définir leurs propres règles (`all`)  

---