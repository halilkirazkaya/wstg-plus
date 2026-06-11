## WSTG-INPV-18 — Test d'Injection de Modèles Côté Serveur (SSTI)

L'Injection de modèles côté serveur (SSTI) se produit lorsqu'une application intègre des entrées contrôlées par l'utilisateur dans un moteur de template côté serveur sans assainissement ou échappement suffisant. Cela permet à un attaquant d'injecter des directives de template malveillantes, qui sont ensuite exécutées sur le serveur, pouvant potentiellement mener à un compromis complet du système via une exécution de code à distance (RCE). La SSTI se manifeste généralement dans des fonctionnalités telles que la génération dynamique d'e-mails, les pages de profil et les systèmes de notification utilisant des moteurs tels que Jinja2, Twig ou FreeMarker. Du point de vue d'un attaquant, cette vulnérabilité est souvent exploitée pour divulguer des variables d'environnement sensibles, lire des fichiers internes ou exécuter des commandes système en exploitant les objets et méthodes natifs du moteur de template.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-INPV-18 |
| **CWE** | CWE-1336 |
| **État du test** | Non effectué |
| **Sévérité** | Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/18-Testing_for_Server-side_Template_Injection  
* https://hacktricks.wiki/en/pentesting-web/ssti-server-side-template-injection/index.html  
* https://portswigger.net/web-security/server-side-template-injection  

**Outils :** `tplmap`, `Burp Suite (Intruder/Repeater)`, `ffuf`, `SSTI-Payload-Generator`

### L'application utilise-t-elle un moteur de template côté serveur pour traiter les entrées utilisateur ?
- [ ] Non — l'application n'utilise **pas** de modèles côté serveur pour le contenu dynamique  
- [ ] Oui — des modèles sont utilisés mais l'entrée utilisateur n'est **pas** intégrée dans les directives de template  
- [ ] Oui — l'entrée utilisateur est intégrée dans les modèles **sans** assainissement approprié  

### Des expressions arithmétiques simples sont-elles évaluées lorsqu'elles sont injectées dans les champs de saisie ?
- [ ] Non — les payloads comme `{{7*7}}` ou `${7*7}` sont reflétés comme des chaînes de caractères littérales  
- [ ] Oui — les expressions sont évaluées (par exemple, retournant `49`), confirmant que le moteur de template **est vulnérable**  

### Les objets ou méthodes intégrés du moteur de template peuvent-ils être consultés ?
- [ ] Non — l'accès aux objets, méthodes ou configurations dangereux est **restreint** ou isolé dans un bac à sable  
- [ ] Oui — les objets intégrés (par exemple, `self`, `config`, `__class__`) **peuvent** être consultés pour divulguer des données sensibles  
- [ ] Oui — l'exécution de code à distance (RCE) via des appels système ou des bibliothèques de l'OS **est possible**  

### Existe-t-il un mécanisme de bac à sable (sandbox) ou de liste d'autorisation (allowlist) empêchant l'exécution de directives dangereuses ?
- [ ] Oui — une liste d'autorisation stricte ou un bac à sable sécurisé est en place et aucun contournement n'est **possible**  
- [ ] Oui — un bac à sable est présent mais un contournement **est possible** via des payloads spécifiques dépendants du moteur  
- [ ] Non — aucun bac à sable ni aucune liste d'autorisation n'est **appliqué** au moteur de template  

---