## WSTG-INPV-01 — Reflected Cross Site Scripting (XSS)

Le Reflected Cross Site Scripting (XSS) se produit lorsqu'une application inclut des données non fiables dans une réponse HTTP sans validation ou encodage suffisant, provoquant l'exécution du Payload dans le contexte du navigateur de la victime. Les attaquants délivrent des payloads malveillants via l'ingénierie sociale, généralement par le biais d'URLs ou de formulaires forgés, pour détourner des sessions utilisateur, exfiltrer des cookies sensibles ou effectuer des actions non autorisées au nom de l'utilisateur. Cette vulnérabilité se trouve couramment dans les paramètres de recherche, les messages d'erreur et tout Endpoint qui reflète une entrée directement dans l'interface utilisateur. L'exploitation repose sur l'interaction de la victime avec un lien malveillant, ce qui en fait un vecteur d'attaque non persistant mais très efficace pour cibler des utilisateurs administratifs ou authentifiés spécifiques.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INPV-01 |
| **CWE** | CWE-79 |
| **Statut du Test** | Non réalisé |
| **Sévérité** | Haute |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/01-Testing_for_Reflected_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/cross-site-scripting  

**Outils :** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `XSStrike`, `KXS`, `dalfox`

### Les entrées fournies par l'utilisateur sont-elles réfléchies dans le corps de la réponse ?
- [ ] Non — l'entrée n'est **jamais** réfléchie vers l'utilisateur  
- [ ] Oui — l'entrée est réfléchie mais correctement encodée et un Bypass n'est **pas possible**  
- [ ] Oui — l'entrée est réfléchie **sans** aucun encodage ni assainissement (sanitization)  

### L'encodage de sortie contextuel est-il implémenté ?
- [ ] Oui — un encodage approprié (HTML, Attribut, JavaScript) est **appliqué** en fonction du contexte de réflexion spécifique  
- [ ] Oui — l'encodage est **appliqué** mais est insuffisant pour le contexte spécifique (par exemple, encodage HTML à l'intérieur d'une balise `<script>`)  
- [ ] Non — l'encodage n'est **pas appliqué**  

### Les filtres de validation d'entrée ou du Web Application Firewall (WAF) peuvent-ils être contournés ?
- [ ] Non — les filtres bloquent efficacement tous les payloads XSS communs et obfusqués  
- [ ] Oui — des filtres sont présents mais un Bypass est **possible** en utilisant l'encodage de caractères, des balises non standard ou des polyglottes  
- [ ] Non — aucun filtre ou mécanisme de validation n'est en place  

### Quel est le contexte d'exécution de l'entrée réfléchie ?
- [ ] Sûr — l'entrée est réfléchie dans un emplacement non exécutable (par exemple, à l'intérieur de balises `<div>` ou `<span>` standards)  
- [ ] Risqué — l'entrée est réfléchie à l'intérieur d'attributs HTML ou dans le `src`/`href` de balises  
- [ ] Critique — l'entrée est réfléchie directement à l'intérieur de blocs `<script>`, de gestionnaires d'événements (event handlers) ou de littéraux de gabarits (template literals)  

### Des en-têtes de sécurité de navigateur modernes sont-ils présents pour atténuer l'impact du XSS ?
- [ ] Oui — la `Content-Security-Policy` (CSP) est **activée** avec une directive script-src restrictive  
- [ ] Oui — la `Content-Security-Policy` (CSP) est **activée** mais contient `unsafe-inline` ou des listes blanches faibles  
- [ ] Non — aucun en-tête CSP ou l'ancien `X-XSS-Protection` n'est présent  

---