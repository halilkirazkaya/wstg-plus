## WSTG-INPV-11 — Injection de Code (Code Injection)

L'injection de code se produit lorsqu'une application évalue des fragments de code fournis par l'utilisateur au sein de son environnement d'exécution, généralement via des fonctions spécifiques au langage non sécurisées telles que `eval()`, `exec()` ou `system()`. Cette vulnérabilité diffère de l'injection de commandes car elle cible le contexte d'exécution du langage de programmation (par exemple, PHP, Python, Node.js) plutôt que le shell du système d'exploitation sous-jacent. Les attaquants exploitent ces points pour exécuter une logique arbitraire, exfiltrer des variables d'environnement ou obtenir une exécution de code à distance (Remote Code Execution - RCE) complète sur le serveur. Les Pentesters doivent examiner les fonctionnalités qui traitent des expressions mathématiques, des templates côté serveur (Server-Side Templates) ou des paramètres de configuration dynamiques où l'entrée pourrait être interprétée comme du code exécutable.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INPV-11 |
| **CWE** | CWE-94 |
| **Statut du Test** | Non effectué |
| **Sévérité** | Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11-Testing_for_Code_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/deserialization  
* https://portswigger.net/web-security/llm-attacks  

**Outils :** `Burp Suite (Repeater/Intruder)`, `FFUF`, `PayloadsAllTheThings`, `Commix`

### Est-ce que des fonctionnalités de l'application évaluent des entrées dynamiques via des fonctions d'évaluation spécifiques au langage ?
- [ ] Non — les fonctions d'évaluation dynamique **ne sont pas** présentes dans la base de code  
- [ ] Oui — des fonctions comme `eval()`, `exec()` ou `include()` sont utilisées mais n'acceptent **pas** d'entrées utilisateur  
- [ ] Oui — des fonctions sont utilisées et traitent des entrées fournies par l'utilisateur  

### Des mécanismes de validation ou d'assainissement (Sanitization) des entrées sont-ils appliqués avant l'évaluation ?
- [ ] Oui — l'entrée est strictement validée par rapport à une **liste blanche (Whitelist) codée en dur**  
- [ ] Oui — l'entrée est assainie via une liste noire (Blacklisting), mais un **contournement (Bypass) est possible**  
- [ ] Non — l'entrée est transmise directement à la fonction d'évaluation et **aucun contrôle** n'est appliqué  

### L'environnement d'exécution est-il isolé (Sandboxed) ou restreint pour empêcher l'accès au niveau système ?
- [ ] Oui — l'exécution a lieu dans un bac à sable (Sandbox) hautement restreint où les appels système **ne peuvent pas** être effectués  
- [ ] Oui — certaines restrictions existent, mais une **évasion de bac à sable (Sandbox Escape) est possible**  
- [ ] Non — l'environnement permet un accès complet aux API du langage et aux fonctions de l'OS sous-jacentes *(Critique)*  

### L'injection de code peut-elle être utilisée pour exfiltrer des informations sensibles ?
- [ ] Non — l'exécution est aveugle (Blind) et aucune communication hors bande (Out-of-band) **n'est possible**  
- [ ] Oui — des variables d'environnement ou des fichiers locaux **peuvent** être exfiltrés via des requêtes HTTP/DNS  
- [ ] Oui — les résultats du code exécuté sont **directement reflétés** dans la réponse de l'application  

### L'application permet-elle l'inclusion de fichiers à distance (Remote File Inclusion) ou le chargement dynamique de bibliothèques ?
- [ ] Non — seuls des chemins de code locaux et prédéfinis peuvent être exécutés  
- [ ] Oui — l'application **peut** être forcée à charger et exécuter du code provenant d'une source distante ou contrôlée par l'attaquant  

---