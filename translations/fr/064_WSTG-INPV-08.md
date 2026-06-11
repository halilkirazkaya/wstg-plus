## WSTG-INPV-08 — Test d'injection SSI (Server-Side Includes)

L'injection Server-Side Includes (SSI) se produit lorsqu'une application ne parvient pas à assainir les entrées utilisateur avant de les incorporer dans des fichiers HTML traités par le moteur SSI du serveur. Les attaquants exploitent cette vulnérabilité en injectant des directives SSI, telles que `<!--#exec cmd="id" -->`, pour exécuter des commandes shell arbitraires, lire des fichiers locaux ou exfiltrer des variables d'environnement. Cette vulnérabilité se trouve généralement dans les applications servant des extensions de fichiers héritées telles que `.shtml`, `.shtm` ou `.stm`, ou lorsque le serveur web est explicitement configuré pour analyser le SSI dans des fichiers HTML standards. Une exploitation réussie accorde à l'attaquant les mêmes permissions que le processus du serveur web, ce qui conduit souvent à un compromis complet du système ou à l'exposition de données sensibles.


| Champ | Valeur |
|---|---|
| **ID WSTG** | WSTG-INPV-08 |
| **CWE** | CWE-97 |
| **État du test** | Non effectué |
| **Sévérité** | Élevée |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/08-Testing_for_SSI_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite (Intruder/Repeater)`, `ffuf`, `Wfuzz`, `curl`

### Le serveur web prend-il en charge et traite-t-il les directives SSI ?
- [ ] Non — le serveur **ne prend pas** en charge le SSI ou les extensions de fichiers comme `.shtml` sont **désactivées**  
- [ ] Oui — le SSI **est activé** mais les entrées utilisateur ne sont **pas** reflétées dans les pages traitées  
- [ ] Oui — le SSI **est activé** et les entrées utilisateur **sont** reflétées dans les pages traitées  

### Des commandes système arbitraires peuvent-elles être exécutées via la directive `#exec` ?
- [ ] Non — la directive `#exec` est **désactivée** ou restreinte par la configuration du serveur  
- [ ] Oui — l'exécution de commandes **est possible** via des payloads `#exec cmd` ou `#exec cgi`  

### L'exfiltration de fichiers sensibles ou de variables d'environnement est-elle possible ?
- [ ] Non — les directives comme `#include` ou `#config` sont **désactivées**  
- [ ] Oui — la lecture de fichiers locaux (ex. : `/etc/passwd`) **est possible** via `#include virtual`  
- [ ] Oui — les variables d'environnement du serveur **peuvent** être exfiltrées via `#printenv` ou `#echo`  

### Les contrôles de validation des entrées ou du WAF sont-ils efficaces contre les payloads SSI ?
- [ ] Oui — une validation stricte des entrées **est appliquée** et aucun contournement n'est **possible**  
- [ ] Oui — la validation/WAF **est appliqué(e)** mais un contournement **est possible** en utilisant l'encodage de caractères ou une syntaxe SSI différente  
- [ ] Non — aucune validation ou protection WAF n'est présente pour les caractères SSI (`<`, `!`, `#`, `-`, `"`)  

---