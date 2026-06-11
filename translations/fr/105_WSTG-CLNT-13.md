## WSTG-CLNT-13 — Test de Cross Site Script Inclusion (XSSI)

L'inclusion de scripts intersites (Cross-Site Script Inclusion - XSSI) se produit lorsqu'une application web exporte des données sensibles au sein d'un fichier JavaScript généré dynamiquement ou d'une réponse JSONP, qu'un site contrôlé par un attaquant peut ensuite inclure via une balise `<script>`. Étant donné que l'inclusion de script contourne la Same-Origin Policy (SOP), un attaquant peut exécuter le script dans son propre contexte et surcharger des objets ou variables globaux pour exfiltrer les informations sensibles. Cette vulnérabilité se manifeste généralement dans des points de terminaison authentifiés qui renvoient des données spécifiques à l'utilisateur telles que des adresses e-mail, des clés API ou des métadonnées de session au format script. Du point de vue de l'attaquant, l'exploitation consiste à concevoir une page malveillante qui référence le script vulnérable et capture les changements de variables globales ou les appels de fonction qui en résultent.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CLNT-13 |
| **CWE** | CWE-200 |
| **Statut du test** | Non effectué |
| **Sévérité** | Moyenne / Haute |

> *La sévérité devient Haute si les données fuitées incluent des jetons CSRF, des identifiants de session ou des PII (Données d'identification personnelle) hautement sensibles.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/13-Testing_for_Cross_Site_Script_Inclusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite`, `JSParser`, `LinkFinder`, `Browser Developer Tools`

### Des points de terminaison authentifiés renvoient-ils du JavaScript dynamique ou du JSONP contenant des informations sensibles ?
- [ ] Non — tous les scripts sont statiques et **ne peuvent pas** contenir de données spécifiques à l'utilisateur  
- [ ] Oui — des scripts dynamiques existent mais **ne contiennent pas** d'informations sensibles  
- [ ] Oui — des scripts dynamiques contiennent des PII ou des jetons et **sont** accessibles entre origines (cross-origin)  

### L'en-tête `X-Content-Type-Options: nosniff` est-il implémenté sur les réponses de scripts sensibles ?
- [ ] Oui — l'en-tête est **appliqué** et empêche le sniffing de type MIME  
- [ ] Non — l'en-tête est **manquant**, permettant potentiellement à du contenu non script d'être exécuté comme un script  

### Les scripts sensibles sont-ils protégés par des mécanismes anti-XSSI tels que des préfixes non exécutables ou des en-têtes personnalisés ?
- [ ] Oui — les scripts nécessitent un en-tête personnalisé ou un jeton qui **ne peut pas** être envoyé via une balise `<script>` standard  
- [ ] Oui — les scripts utilisent des préfixes non exécutables (ex: `)]}'`) qui **ne peuvent pas** être facilement contournés  
- [ ] Non — les scripts sont directement accessibles via des requêtes GET utilisant des identifiants ambiants *(Critique)*  

### L'exfiltration de données sensibles est-elle possible en surchargeant des variables globales ou des prototypes ?
- [ ] Non — les données sensibles ont une portée locale ou sont protégées via des fonctionnalités modernes du navigateur  
- [ ] Oui — les données sensibles sont assignées à des variables globales et **peuvent** être capturées  
- [ ] Oui — les données fuitent via des rappels (callbacks) JSONP qui **peuvent** être interceptés  

---