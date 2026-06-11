## WSTG-CLNT-07 — Cross-Origin Resource Sharing (CORS)

Le Cross-Origin Resource Sharing (CORS) est un mécanisme basé sur le navigateur qui permet à un serveur d'autoriser explicitement l'accès à ses ressources à partir de différentes origines, assouplissant ainsi la Same-Origin Policy (SOP). Des mauvaises configurations (misconfigurations) surviennent lorsqu'une application fait une confiance excessive à des origines arbitraires, souvent en reflétant l'en-tête `Origin` dans l'en-tête de réponse `Access-Control-Allow-Origin` ou en utilisant des listes blanches (whitelists) par expressions régulières (regex) mal implémentées. Du point de vue d'un attaquant, une politique CORS permissive peut être exploitée en hébergeant un script malveillant sur un domaine contrôlé qui effectue des requêtes authentifiées vers l'application vulnérable, permettant à l'attaquant d'exfiltrer des données sensibles, telles que des jetons CSRF (CSRF tokens) ou des PII, directement depuis le corps de la réponse (response body). Cette vulnérabilité est particulièrement critique sur les points de terminaison d'API (API endpoints) qui traitent des sessions authentifiées et renvoient des informations non publiques.


| Champ | Valeur |
|---|---|
| **ID WSTG** | WSTG-CLNT-07 |
| **CWE** | CWE-942 |
| **Statut du test** | Non effectué |
| **Sévérité** | Moyenne / Haute |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/07-Testing_Cross_Origin_Resource_Sharing  
* https://hacktricks.wiki/en/pentesting-web/cors-bypass.html  
* https://portswigger.net/web-security/cors  

**Outils :** `Burp Suite (CORS Raider)`, `curl`, `Postman`, `Corsy`

### L'application implémente-t-elle des en-têtes CORS sur les points de terminaison authentifiés ?
- [ ] Non — le CORS n'est **pas activé** ; le navigateur utilise par défaut la Same-Origin Policy stricte  
- [ ] Oui — les en-têtes CORS sont présents et restreints à une **whitelist** stricte  
- [ ] Oui — les en-têtes CORS sont présents mais configurés de manière **permissive**  

### Comment le serveur gère-t-il l'en-tête `Access-Control-Allow-Origin` (ACAO) ?
- [ ] L'ACAO est défini sur un domaine statique spécifique et **de confiance**  
- [ ] L'ACAO est défini sur `*` (wildcard) et les identifiants (credentials) **ne peuvent pas** être envoyés  
- [ ] L'ACAO est défini sur `*` (wildcard) et les identifiants (credentials) **peuvent** être envoyés *(Note : La plupart des navigateurs bloquent cette configuration)*  
- [ ] L'ACAO **reflète dynamiquement** la valeur de l'en-tête `Origin` provenant de la requête  

### L'en-tête `Access-Control-Allow-Credentials` (ACAC) est-il défini sur `true` ?
- [ ] Non — l'ACAC n'est **pas présent** ou est défini sur `false`  
- [ ] Oui — l'ACAC est défini sur `true`, mais l'ACAO est **restreint** à des origines de confiance  
- [ ] Oui — l'ACAC est défini sur `true` et l'ACAO **reflète** des origines arbitraires *(Critique)*  

### La whitelist d'origines peut-elle être contournée via une manipulation de sous-domaine ou de l'origine null ?
- [ ] Non — la whitelist est strictement appliquée et **ne peut pas** être contournée  
- [ ] Oui — la whitelist accepte **n'importe quel** sous-domaine de la cible (ex: `attacker.target.com`)  
- [ ] Oui — la whitelist accepte l'origine `null` via des iframes sandboxées  
- [ ] Oui — la whitelist est vulnérable aux contournements par regex (ex: `target.com.attacker.com` ou `target.com-safe.com`)  

### La configuration CORS permet-elle l'exfiltration de données sensibles ?
- [ ] Non — les points de terminaison exposés ne contiennent **pas** de données sensibles ou spécifiques à la session  
- [ ] Oui — les points de terminaison authentifiés renvoient des PII, des identifiants (credentials) ou des jetons CSRF (CSRF tokens) qui **peuvent** être lus par une origine non autorisée  

---