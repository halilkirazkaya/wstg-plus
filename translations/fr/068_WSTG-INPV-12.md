## WSTG-INPV-12 — Command Injection

L'injection de commandes (Command Injection) se produit lorsqu'une application transmet des données non sécurisées fournies par l'utilisateur, telles que des entrées de formulaire, des en-têtes HTTP ou des cookies, à un shell système. Cette vulnérabilité permet à un attaquant d'exécuter des commandes arbitraires du système d'exploitation sur le serveur, généralement avec les privilèges du processus de l'application web. Du point de vue d'un attaquant, cela est exploité en utilisant des métacaractères de shell tels que des points-virgules, des pipes ou des backticks pour enchaîner des commandes malveillantes au flux d'exécution prévu. L'impact est fréquemment catastrophique, menant à une compromission totale du système, à l'exfiltration de données non autorisée ou à un mouvement latéral au sein du réseau interne.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INPV-12 |
| **CWE** | CWE-78 |
| **État du test** | Non effectué |
| **Sévérité** | Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/12-Testing_for_Command_Injection  
* https://hacktricks.wiki/en/pentesting-web/command-injection.html  
* https://portswigger.net/web-security/os-command-injection  

**Outils :** `Commix`, `Burp Suite (Intruder/Repeater)`, `dnsbin`, `Interactsh`, `Collaborator`

### L'application invoque-t-elle des commandes de niveau système ou des exécutables shell ?
- [ ] Non — l'application n'interagit **pas** avec le shell de l'OS  
- [ ] Oui — l'application utilise des API internes ou des bibliothèques de haut niveau pour les tâches système  
- [ ] Oui — l'application invoque directement des commandes shell via des appels système tels que `system()`, `exec()` ou `popen()`  

### Les entrées utilisateur sont-elles validées ou assainies avant d'être transmises aux appels système ?
- [ ] Oui — une liste d'autorisation (allow-listing) stricte et une validation des entrées **sont appliquées**  
- [ ] Oui — l'assainissement (sanitization) ou l'échappement est utilisé mais un contournement (bypass) **est possible**  
- [ ] Non — les entrées utilisateur sont transmises directement au shell **sans** validation  

### Les métacaractères de shell peuvent-ils être utilisés pour manipuler le flux d'exécution des commandes ?
- [ ] Non — les métacaractères sont correctement filtrés ou échappés  
- [ ] Oui — une exécution en bande (in-band) où la sortie de la commande est reflétée dans la réponse **est possible**  
- [ ] Oui — une exécution aveugle (blind) où aucune sortie n'est reflétée **est possible**  

### Une exfiltration hors bande (Out-of-band - OOB) est-elle possible depuis l'hôte ?
- [ ] Non — le serveur n'a pas de sortie (egress) ou les signaux OOB (DNS/HTTP) échouent  
- [ ] Oui — l'exfiltration de données via des signaux OOB basés sur DNS ou HTTP **est possible**  

### Le processus de l'application s'exécute-t-il avec des privilèges système élevés ?
- [ ] Non — l'application s'exécute avec un compte de service dédié à faibles privilèges  
- [ ] Oui — l'application s'exécute en tant que `root`, `Administrator` ou un utilisateur à hauts privilèges *(Critique)*  

---