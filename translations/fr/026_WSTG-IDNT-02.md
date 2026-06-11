## WSTG-IDNT-02 — Test du processus d'enregistrement des utilisateurs

Le test du processus d'enregistrement des utilisateurs consiste à évaluer le flux de travail par lequel de nouvelles identités sont créées afin de s'assurer qu'il ne peut pas être détourné pour un accès non autorisé ou une exploitation automatisée. Les vulnérabilités dans ce processus se manifestent souvent par un manque de vérification d'identité (e-mail/SMS), une vulnérabilité à la création de comptes en masse via des bots, ou une divulgation d'informations via des messages d'erreur verbeux qui révèlent des noms d'utilisateurs existants. Les attaquants exploitent ces failles pour effectuer une User Enumeration, mener des campagnes de spam à grande échelle ou manipuler les paramètres d'enregistrement pour obtenir des privilèges élevés lors de la phase de création de compte. Ce test est essentiel pour protéger l'intégrité de l'application et empêcher les attaquants de peupler le système avec des comptes frauduleux ou malveillants.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-IDNT-02 |
| **CWE** | CWE-836 |
| **Statut du Test** | Non effectué |
| **Sévérité** | Moyenne |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/02-Test_User_Registration_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite`, `OWASP ZAP`, `Python`, `ffuf`, `Cuppa`

### Une vérification d'identité est-elle requise avant qu'un nouveau compte ne devienne actif ?
- [ ] Oui — l'enregistrement nécessite un token unique, limité dans le temps, envoyé par e-mail ou SMS  
- [ ] Oui — la vérification est requise mais les tokens sont **faibles** ou **prévisibles**  
- [ ] Non — les comptes sont activés **immédiatement** sans aucune étape de vérification  

### Le processus d'enregistrement empêche-t-il l'User Enumeration ?
- [ ] Oui — l'application renvoie des réponses **identiques** pour les e-mails/noms d'utilisateur nouveaux et existants  
- [ ] Non — les messages d'erreur (ex: "E-mail déjà utilisé") **permettent** à un attaquant de cartographier les utilisateurs enregistrés  
- [ ] Non — des différences de temps de réponse du serveur (Timing Attacks) **révèlent** si un utilisateur existe  

### Des contrôles anti-automatisation sont-ils mis en œuvre pour empêcher l'enregistrement massif ?
- [ ] Oui — des mécanismes CAPTCHA ou de proof-of-work sont **présents** et **efficaces**  
- [ ] Oui — des contrôles existent mais un bypass **est possible** via les points de terminaison API ou la manipulation de headers  
- [ ] Non — aucun Rate Limiting ou CAPTCHA n'est **appliqué** au point de terminaison d'enregistrement  

### Les paramètres d'enregistrement peuvent-ils être manipulés pour obtenir une escalade de privilèges ?
- [ ] No — les rôles d'utilisateur sont attribués **strictement** par la logique côté serveur  
- [ ] Oui — des paramètres tels que `role`, `admin` ou `group_id` **peuvent** être modifiés dans la requête pour obtenir un accès plus élevé  

### La politique de mot de passe est-elle appliquée lors de la phase d'enregistrement ?
- [ ] Oui — des exigences de mot de passe fort sont **appliquées** côté serveur  
- [ ] Non — les mots de passe faibles (ex: "password123") sont **acceptés** par le système  

---