## WSTG-CLNT-08 — Test de Cross Site Flashing

Le Cross-Site Flashing (XSF) est une vulnérabilité côté client qui survient lorsqu'un fichier Flash (SWF) gère de manière incorrecte des entrées contrôlées par l'utilisateur, permettant à un attaquant d'exécuter du code ActionScript malveillant ou de créer un pont vers l'environnement JavaScript du navigateur. En manipulant des paramètres tels que `FlashVars` ou des chaînes de requête URL transmises à des puits (sinks) comme `ExternalInterface.call`, `getURL` ou `loadMovie`, un attaquant peut effectuer des actions dans le contexte du domaine vulnérable, incluant le détournement de session (session hijacking) et l'exfiltration de données. Cette vulnérabilité se retrouve principalement dans les applications d'entreprise héritées (legacy) qui hébergent encore des fichiers SWF, où une validation insuffisante des entrées permet au film Flash d'être détourné comme vecteur de Cross-Site Scripting (XSS) ou pour contourner la Same-Origin Policy (SOP) via des configurations cross-domain permissives.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-CLNT-08 |
| **CWE** | CWE-79 |
| **État du test** | Non effectué |
| **Sévérité** | Moyenne / Élevée |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/08-Testing_for_Cross_Site_Flashing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `JPEXS Free Flash Decompiler`, `FFDec`, `Burp Suite`, `Google Dorks`, `strings`

### Des fichiers Adobe Flash (.swf) hérités sont-ils hébergés sur l'application ?
- [ ] Non — aucun fichier Flash n'est hébergé ou référencé dans le périmètre de l'application  
- [ ] Oui — des fichiers SWF sont présents mais ne servent que du contenu statique  
- [ ] Oui — des fichiers SWF sont présents et acceptent des paramètres dynamiques via `FlashVars` ou des chaînes de requête URL  

### Les entrées transmises aux puits (sinks) ActionScript sont-elles assainies ?
- [ ] Oui — toutes les entrées sont strictement validées et les puits ActionScript **ne peuvent pas** être manipulés  
- [ ] Oui — une validation est appliquée mais un contournement (bypass) **est possible** via des techniques d'encodage spécifiques  
- [ ] Non — l'entrée est transmise directement à des puits sensibles comme `ExternalInterface.call` ou `getURL` *(Critique)*  

### Le fichier Flash peut-il être utilisé pour exécuter du JavaScript arbitraire (XSS) ?
- [ ] Non — `ExternalInterface` est **désactivé** ou le paramètre `allowScriptAccess` est défini sur `never`  
- [ ] Oui — `allowScriptAccess` est défini sur `sameDomain` mais le SWF est hébergé sur le domaine cible  
- [ ] Oui — `allowScriptAccess` est défini sur `always`, permettant un XSS depuis n'importe quel domaine  

### La politique `crossdomain.xml` empêche-t-elle les requêtes cross-origin non autorisées ?
- [ ] Oui — `crossdomain.xml` est restrictif et n'autorise que des origines spécifiques et de confiance  
- [ ] Non — le fichier `crossdomain.xml` **est manquant**  
- [ ] Oui — la politique est excessivement permissive (ex: `<allow-access-from domain="*" />`)  

### Le fichier SWF peut-il être forcé de charger des animations externes contrôlées par un attaquant ?
- [ ] Non — l'application **ne peut pas** être forcée de charger des fichiers SWF externes  
- [ ] Oui — les fonctions `loadMovie` ou `loadMovieNum` acceptent des URL externes non validées  

---