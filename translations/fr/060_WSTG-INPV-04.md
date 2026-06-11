## WSTG-INPV-04 — Test de l'HTTP Parameter Pollution

L'HTTP Parameter Pollution (HPP) se produit lorsqu'une application reçoit plusieurs paramètres HTTP portant le même nom et les traite de manière incohérente ou non sécurisée. En fournissant des paramètres en double, un attaquant peut manipuler la logique interne de l'application, contournant potentiellement les Web Application Firewalls (WAF), les filtres de validation des entrées ou les mécanismes de contrôle d'accès. Cette vulnérabilité dépend fortement du serveur web sous-jacent ou du framework applicatif, qui peut choisir la première, la dernière ou une version concaténée des paramètres répétés. L'exploitation cible généralement les points de terminaison qui transmettent des paramètres à des API back-end, des requêtes de base de données ou des mécanismes de redirection d'URL, entraînant des impacts allant du Cross-Site Scripting (XSS) à l'exfiltration de données non autorisée.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INPV-04 |
| **CWE** | CWE-235 |
| **Statut du Test** | Non réalisé |
| **Sévérité** | Moyenne |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/04-Testing_for_HTTP_Parameter_Pollution  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite (Repeater/Intruder)`, `Arjun`, `HPP Finder`, `Wfuzz`

### Comment l'application gère-t-elle plusieurs paramètres HTTP ayant le même nom ?
- [ ] Non — les paramètres en double sont **rejetés** ou ignorés par le serveur  
- [ ] Oui — le serveur sélectionne systématiquement uniquement la **première** ou la **dernière** occurrence  
- [ ] Oui — le serveur **concatène** les valeurs, permettant potentiellement une injection  

### L'HPP peut-elle être exploitée pour contourner les contrôles de sécurité tels qu'un WAF ou un filtre d'entrée ?
- [ ] Non — les contrôles de sécurité inspectent correctement **toutes** les occurrences d'un paramètre  
- [ ] Oui — un WAF ou un filtre n'inspecte que la **première** occurrence, permettant à une **seconde** occurrence malveillante de passer  
- [ ] Oui — la concaténation permet à un attaquant de **diviser** un Payload sur plusieurs paramètres pour échapper à la détection  

### L'application reflète-t-elle les paramètres pollués dans la réponse sans assainissement approprié ?
- [ ] Non — les paramètres sont assainis ou encodés avant d'être reflétés dans l'UI  
- [ ] Oui — la pollution entraîne un contenu reflété, mais l'assainissement **est appliqué**  
- [ ] Oui — la pollution entraîne un contenu reflété et l'assainissement **n'est pas appliqué**, menant à un XSS ou à des erreurs logiques  

### Les paramètres transmis aux appels API internes ou aux systèmes back-end sont-ils vulnérables à la pollution ?
- [ ] Non — les requêtes back-end utilisent des méthodes de construction sûres et non dynamiques  
- [ ] Oui — l'HPP permet à un attaquant d'**injecter** ou de **réécrire** des paramètres dans la structure de la requête back-end  

### L'application présente-t-elle un comportement différent lorsque les paramètres sont pollués dans les requêtes GET par rapport aux requêtes POST ?
- [ ] Non — le comportement est cohérent pour toutes les méthodes HTTP  
- [ ] Oui — seuls les paramètres GET sont vulnérables à la pollution  
- [ ] Oui — seuls les paramètres POST (body) sont vulnérables à la pollution  

---