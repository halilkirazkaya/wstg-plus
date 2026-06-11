## WSTG-BUSL-06 — Test de contournement des flux de travail (Workflow Circumvention)

Le contournement de flux de travail consiste à manipuler la logique d'une application pour outrepasser les séquences ou les prérequis prévus au sein de processus à étapes multiples. Les attaquants identifient des points de terminaison (endpoints) sensibles—tels que les confirmations de paiement, les approbations administratives ou les écrans de validation MFA—et tentent d'y accéder directement, en sautant les étapes intermédiaires obligatoires. Cette vulnérabilité survient lorsque la logique côté serveur ne parvient pas à maintenir une machine d'état (state machine) robuste, s'appuyant plutôt sur l'hypothèse que l'utilisateur suit le chemin guidé par l'interface utilisateur (UI). Une exploitation réussie peut entraîner des défaillances critiques de la logique métier (Business Logic), notamment des transactions non autorisées, une élévation de privilèges (Privilege Escalation) et le contournement des mécanismes de vérification d'identité.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-BUSL-06 |
| **CWE** | CWE-841 |
| **Statut du Test** | Non effectué |
| **Sévérité** | Moyenne / Élevée* |

> *La sévérité devient Élevée si le contournement permet des transactions financières non autorisées, une élévation de privilèges ou un accès administratif.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/06-Testing_for_the_Circumvention_of_Work_Flows  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**Outils :** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Postman`, `Caido`

### L'application contient-elle des processus métier à étapes multiples ?
- [ ] Non — l'application ne consiste qu'en des actions à requête unique  
- [ ] Oui — des processus à étapes multiples (ex : paniers d'achat, formulaires d'assistant, inscription) **sont** présents  

### La séquence des étapes est-elle strictement imposée par le serveur ?
- [ ] Oui — le suivi d'état côté serveur garantit que les étapes **ne peuvent pas** être sautées  
- [ ] Oui — les contrôles côté client suggèrent une séquence, mais le serveur **ne valide pas** la progression  
- [ ] Non — l'application **n'impose aucun** ordre spécifique des opérations  

### Un attaquant peut-il atteindre l'étape finale d'un processus en demandant directement l'URL finale ou le point de terminaison ?
- [ ] Non — l'accès direct aux étapes finales **n'est pas possible** sans complétion préalable  
- [ ] Oui — les étapes finales (ex : `/api/v1/checkout/complete`) **peuvent** être atteintes par requête directe  

### L'application vérifie-t-elle que les données prérequises des étapes précédentes sont présentes et valides ?
- [ ] Oui — le serveur valide toutes les données des étapes précédentes avant de continuer  
- [ ] Non — le serveur **est** vulnérable au "saut" d'étapes où des données sont attendues mais non vérifiées  

### Les contrôles de sécurité tels que le MFA ou la vérification d'identité peuvent-ils être contournés en manipulant le flux de travail ?
- [ ] Non — les contrôles critiques **ne peuvent pas** être contournés via une manipulation de flux  
- [ ] Oui — le contournement du MFA ou des vérifications d'identité **est possible** en sautant directement à l'étape post-vérification  

---