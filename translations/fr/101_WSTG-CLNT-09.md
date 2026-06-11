## WSTG-CLNT-09 — Test du Clickjacking

Le Clickjacking (également connu sous le nom de UI redressing) se produit lorsqu'un attaquant utilise des couches transparentes ou opaques pour tromper un utilisateur et l'inciter à cliquer sur un bouton ou un lien sur une autre page, alors que celui-ci pensait cliquer sur la page de premier niveau. Cette vulnérabilité compromet l'intégrité des actions de l'utilisateur, pouvant entraîner une modification non autorisée de données, des changements de compte ou des transferts financiers effectués au nom de la victime. Les attaquants exploitent généralement cela en intégrant le site web cible dans une `<iframe>` sur un site malveillant contrôlé et en le superposant avec un contenu trompeur ou des incitations attrayantes. Cela se produit le plus fréquemment sur les pages effectuant des actions sensibles modifiant l'état, telles que les boutons « Supprimer le compte », les formulaires de réinitialisation de mot de passe ou les panneaux de configuration administrative.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CLNT-09 |
| **CWE** | CWE-1021 |
| **État du test** | Non effectué |
| **Sévérité** | Moyenne / Élevée* |

> *La sévérité devient Élevée si des actions sensibles (ex : transferts de fonds, suppression de compte ou élévation de privilèges) peuvent être déclenchées via clickjacking.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/09-Testing_for_Clickjacking  
* https://hacktricks.wiki/en/pentesting-web/clickjacking.html  
* https://portswigger.net/web-security/clickjacking  

**Outils :** `Burp Suite Clickbandit`, `Browser Developer Tools`, `OWASP ZAP`, `Online Clickjack Test`

### L'application implémente-t-elle des en-têtes côté serveur pour empêcher le cadrage (framing) non autorisé ?
- [ ] Oui — `X-Frame-Options` ou `Content-Security-Policy` **sont appliqués** et empêchent le cadrage *(Le plus sécurisé)*  
- [ ] Oui — les en-têtes sont présents mais **mal configurés** (ex : `X-Frame-Options: ALLOW-FROM` qui manque d'un support large par les navigateurs)  
- [ ] Non — aucun en-tête de protection contre le cadrage **n'est appliqué**  

### La directive `frame-ancestors` de la `Content-Security-Policy` (CSP) est-elle implémentée ?
- [ ] Oui — `frame-ancestors 'none'` ou `'self'` **est appliqué**  
- [ ] Oui — `frame-ancestors` est présent mais **autorise** des origines non fiables ou trop larges  
- [ ] Non — la directive est **manquante** ou la CSP n'est pas implémentée  

### L'en-tête `X-Frame-Options` (XFO) est-il correctement configuré pour le support des navigateurs anciens ?
- [ ] Oui — `DENY` ou `SAMEORIGIN` **est appliqué**  
- [ ] Oui — XFO est présent mais **ignoré** par les navigateurs modernes car une directive CSP `frame-ancestors` existe  
- [ ] Non — l'en-tête XFO est **manquant** ou défini sur une valeur non sécurisée  

### Les protections contre le cadrage peuvent-elles être contournées à l'aide de techniques connues ?
- [ ] Non — contournement **impossible** via les techniques standards (double-framing, etc.)  
- [ ] Oui — la protection **peut** être contournée en raison d'une regex ou d'une logique faible dans la liste blanche (white-list) de `frame-ancestors`  
- [ ] Oui — la protection **peut** être contournée car l'application s'appuie uniquement sur du frame-busting basé sur JavaScript  

### Une action sensible est-elle exploitable via une preuve de concept (PoC) de clickjacking ?
- [ ] Non — le cadrage est bloqué ou aucune action sensible n'est accessible  
- [ ] Oui — le UI redressing **est possible** mais nécessite une interaction utilisateur complexe  
- [ ] Oui — le UI redressing **est possible** et permet des actions immédiates de modification d'état *(Critique)*  

---