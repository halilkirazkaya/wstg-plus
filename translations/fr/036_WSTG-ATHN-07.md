## WSTG-ATHN-07 — Test de politique de mots de passe faible

Le test des politiques de mots de passe faibles consiste à évaluer les exigences de complexité, de longueur et d'entropie appliquées par une application lors de la création de compte et de la mise à jour des identifiants. Des politiques insuffisantes compromettent directement la confidentialité des comptes utilisateurs en les rendant vulnérables aux attaques par dictionnaire automatisées, aux tentatives de Brute Force et au Credential Stuffing. Ces vulnérabilités résident généralement dans les points de terminaison d'inscription, les flux de réinitialisation de mot de passe et les panneaux de gestion administrative des utilisateurs. Du point de vue d'un attaquant, une politique permissive réduit considérablement le coût de calcul et le temps nécessaire pour compromettre avec succès les identifiants en utilisant des listes de mots courantes ou du cassage basé sur des modèles.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-ATHN-07 |
| **CWE** | CWE-521 |
| **État du test** | Non effectué |
| **Sévérité** | Moyenne / Faible* |

> *La sévérité devient Élevée (High) si l'application manque de mécanismes de verrouillage de compte (account lockout) ou de Rate Limiting pour empêcher les tentatives de devinette automatisées.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/07-Testing_for_Weak_Authentication_Methods  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**Outils :** `Burp Suite (Intruder)`, `ZAP (Fuzzer)`, `Hydra`, `Hashcat`, `Cewl`

### Une longueur minimale de mot de passe est-elle imposée par l'application ?
- [ ] Oui — la longueur minimale est de 12 caractères ou plus *(Le plus sécurisé)*  
- [ ] Oui — la longueur minimale est comprise entre 8 et 11 caractères  
- [ ] Oui — la longueur minimale est **inférieure à** 8 caractères  
- [ ] Non — aucune longueur minimale n'est imposée  

### Des exigences de complexité (majuscules, minuscules, chiffres, symboles) sont-elles imposées ?
- [ ] Oui — plusieurs classes de caractères sont obligatoires et un contournement n'est **pas possible**  
- [ ] Oui — les classes de caractères sont suggérées mais **non** imposées  
- [ ] Non — aucune exigence de complexité n'est appliquée  

### L'application bloque-t-elle les mots de passe courants/faibles ou les mots de passe contenant des données spécifiques à l'utilisateur ?
- [ ] Oui — les mots de passe faibles connus (ex. : "password123") et les mots de passe basés sur le nom d'utilisateur **ne peuvent pas** être utilisés  
- [ ] Oui — les mots de passe courants sont bloqués, mais les données spécifiques à l'utilisateur (ex. : nom d'utilisateur, email) **peuvent** être utilisées  
- [ ] Non — toute chaîne de caractères répondant aux exigences de longueur/complexité est acceptée  

### Les exigences de complexité ou de longueur peuvent-elles être contournées via une modification côté client ?
- [ ] Non — les politiques sont strictement appliquées côté serveur (**server-side**)  
- [ ] Oui — les politiques sont appliquées via JavaScript et **peuvent** être contournées en interceptant la requête  

### La politique de mot de passe est-elle clairement communiquée à l'utilisateur sans divulguer de détails d'implémentation ?
- [ ] Oui — la politique est visible lors de la saisie et fournit des messages d'erreur génériques  
- [ ] Oui — la politique est visible mais les messages d'erreur révèlent des expressions régulières (Regex) spécifiques ou des failles logiques  
- [ ] Non — la politique n'est **pas** visible, forçant une découverte par tâtonnement (trial-and-error)  

---