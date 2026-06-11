## WSTG-CONF-12 — Content Security Policy (CSP)

La Content Security Policy (CSP) est un mécanisme de défense en profondeur essentiel, implémenté via les en-têtes de réponse HTTP pour atténuer les risques tels que le Cross-Site Scripting (XSS), le clickjacking et les attaques par injection de données. En définissant quelles ressources dynamiques sont autorisées à être chargées, une CSP bien configurée restreint la capacité d'un attaquant à exécuter des scripts non autorisés ou à exfiltrer des données sensibles vers des domaines externes. Les Pentesters évaluent la politique pour détecter des directives trop permissives, l'utilisation de mots-clés non sécurisés comme `'unsafe-inline'`, et la dépendance à des CDNs non sécurisés qui pourraient faciliter des contournements (bypasses). Une CSP faible ou absente ne crée pas directement une vulnérabilité, mais augmente considérablement l'impact des attaques par injection réussies en ne fournissant pas de couche de protection secondaire.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CONF-12 |
| **CWE** | CWE-693 |
| **Statut du Test** | Non réalisé |
| **Sévérité** | Faible / Moyenne |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/12-Test_for_Content_Security_Policy  
* https://hacktricks.wiki/en/pentesting-web/content-security-policy-csp-bypass/index.html  

**Outils :** `Google CSP Evaluator`, `Burp Suite (CSP Pro)`, `Mozilla Observatory`, `ZAP`, `CSP Mitigator`

### Un en-tête Content Security Policy (CSP) est-il présent dans les réponses de l'application ?
- [ ] Oui — l'en-tête `Content-Security-Policy` est **présent** et **appliqué**  
- [ ] Oui — `Content-Security-Policy-Report-Only` est **présent** pour test  
- [ ] Non — aucun en-tête CSP n'est **présent** dans les réponses  

### Les directives `script-src` ou `default-src` sont-elles correctement restreintes ?
- [ ] Oui — les directives utilisent un whitelisting strict, des nonces ou des hashes et aucun contournement n'est **possible**  
- [ ] Oui — les directives sont **correctement configurées** mais la liste blanche inclut des CDNs connus pour être contournables  
- [ ] Non — les directives utilisent des jokers (`*`) ou des schémas `data:`, rendant le contournement **possible**  

### La politique autorise-t-elle l'exécution de scripts inline non sécurisés ou de eval ?
- [ ] Non — `'unsafe-inline'` et `'unsafe-eval'` ne sont **pas présents**  
- [ ] Oui — `'unsafe-inline'` est **présent** mais protégé par un `nonce` ou un `hash`  
- [ ] Oui — `'unsafe-inline'` ou `'unsafe-eval'` sont **activés** sans protections supplémentaires *(Critique)*  

### L'application est-elle protégée contre le clickjacking via la directive `frame-ancestors` ?
- [ ] Oui — `frame-ancestors` est défini sur `'none'` ou `'self'`  
- [ ] Oui — `frame-ancestors` est **activé** mais autorise des domaines tiers de confiance spécifiques  
- [ ] Non — `frame-ancestors` est **manquant**, s'appuyant sur l'ancien en-tête `X-Frame-Options` ou aucune protection  

### Existe-t-il un mécanisme pour signaler les violations de la CSP ?
- [ ] Oui — `report-uri` ou `report-to` est **configuré** et actif  
- [ ] Non — le signalement des violations est **désactivé** ou **non configuré**  

---