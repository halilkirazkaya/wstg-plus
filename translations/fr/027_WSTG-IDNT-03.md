## WSTG-IDNT-03 — Test du processus de provisionnement de compte

Le processus de provisionnement de compte (Account Provisioning) régit la manière dont les nouvelles identités sont créées, vérifiées et dotées de privilèges au sein de l'environnement d'une application. Les attaquants ciblent ce flux de travail pour effectuer des enregistrements de masse automatisés à des fins de spam ou de déni de service (DoS), contourner les étapes de vérification d'identité pour créer des comptes frauduleux, ou manipuler des paramètres pour s'octroyer des permissions élevées lors de la phase d'inscription. Les vulnérabilités se manifestent souvent dans les formulaires d'inscription en libre-service, les systèmes sur invitation uniquement ou les consoles de provisionnement administratif où des défauts de logique métier permettent la création de comptes non autorisés ou une escalade de privilèges. Du point de vue d'un attaquant, compromettre le flux de provisionnement fournit un ancrage légitime qui peut être utilisé comme base pour une exploitation plus profonde, l'exfiltration de données ou des attaques d'ingénierie sociale.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-IDNT-03 |
| **CWE** | CWE-285 |
| **État du test** | Non réalisé |
| **Sévérité** | Moyenne / Élevée* |

> *La sévérité devient Critique si un attaquant peut provisionner des comptes administratifs ou contourner les restrictions d'inscription globales.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/03-Test_Account_Provisioning_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite`, `FFUF`, `Postman`, `Python`, `MailHog`

### L'auto-inscription est-elle strictement contrôlée ou limitée aux utilisateurs autorisés ?
- [ ] Oui — l'inscription est **désactivée** ou nécessite une approbation administrative manuelle  
- [ ] Oui — l'inscription est ouverte mais restreinte à des domaines de messagerie **autorisés** ou à des jetons d'invitation (tokens)  
- [ ] Non — l'inscription est **ouverte** à tout utilisateur sans restriction de domaine ou de jeton  

### Un mécanisme de vérification d'identité robuste (ex : email/SMS) est-il requis avant l'activation du compte ?
- [ ] Oui — la vérification est **requise** et **ne peut pas** être contournée  
- [ ] Oui — la vérification est **requise** mais **peut** être contournée via une manipulation de paramètres ou des jetons prévisibles  
- [ ] Non — les comptes sont **actifs immédiatement** après la soumission sans aucune vérification  

### L'utilisateur peut-il manipuler les paramètres de rôle ou de permission lors de la requête d'inscription ?
- [ ] Non — les rôles sont **assignés côté serveur** et aucun paramètre côté client n'existe  
- [ ] Oui — des paramètres de rôle existent mais la manipulation n'est **pas possible** en raison de la validation côté serveur  
- [ ] Oui — les paramètres de rôle/permission (ex : `role=admin`, `is_staff=true`) **peuvent** être modifiés pour obtenir un accès privilégié *(Critique)*  

### Des limitations de débit (Rate Limiting) ou des contrôles anti-automatisation sont-ils mis en œuvre pour empêcher la création massive de comptes ?
- [ ] Oui — les limitations de débit et les CAPTCHAs sont **actifs** et efficaces  
- [ ] Oui — des contrôles existent mais un contournement **est possible** via l'usurpation d'en-tête (header spoofing) ou des services de résolution de CAPTCHA  
- [ ] Non — aucune limitation de débit ni aucun contrôle anti-automatisation n'est **appliqué**  

### Le processus de provisionnement divulgue-t-il des informations sur les comptes existants (User Enumeration) ?
- [ ] Non — les messages de réponse sont **identiques** pour les utilisateurs existants et non existants  
- [ ] Oui — des réponses différentes ou des variations de timing **permettent** l'énumération des utilisateurs existants  

---