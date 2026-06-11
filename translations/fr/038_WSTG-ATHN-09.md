## WSTG-ATHN-09 — Test des fonctionnalités de changement ou de réinitialisation de mot de passe faibles

Les mécanismes de changement et de réinitialisation de mot de passe sont des composants critiques de la gestion de l'authentification qui, s'ils sont mal implémentés, permettent à des attaquants de prendre le contrôle de comptes utilisateurs (Account Takeover). Ces vulnérabilités impliquent généralement des jetons (Tokens) de réinitialisation prévisibles, une absence de verrouillage de compte (Account Lockout) lors de tentatives de Brute Force, un acheminement non sécurisé des jetons, ou la possibilité de changer de mot de passe sans vérifier le mot de passe actuel. Les attaquants exploitent ces failles en interceptant ou en devinant les liens de récupération, en effectuant une Host Header Injection pour rediriger les e-mails de réinitialisation, ou en utilisant la fixation de session (Session Fixation) pour maintenir l'accès après un changement de mot de passe. Garantir que ces processus nécessitent une vérification forte et utilisent un caractère aléatoire cryptographique (cryptographic randomness) est essentiel pour maintenir l'intégrité du compte et prévenir les accès non autorisés.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-ATHN-09 |
| **CWE** | CWE-640 |
| **Statut du test** | Non réalisé |
| **Sévérité** | Élevée / Critique |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/09-Testing_for_Weak_Password_Change_or_Reset_Functionalities  
* https://hacktricks.wiki/en/pentesting-web/reset-password.html  

**Outils :** `Burp Suite (Repeater/Intruder)`, `Python (Custom Scripts)`, `CyberChef`, `OAST (Interactsh/Burp Collaborator)`

### Le mot de passe actuel est-il requis pour définir un nouveau mot de passe lors d'un changement standard ?
- [ ] Oui — le mot de passe actuel **est requis** et vérifié  
- [ ] Oui — le mot de passe actuel **est requis** mais peut être contourné via une Parameter Pollution ou une manipulation de paramètres  
- [ ] Non — le mot de passe actuel **n'est pas** demandé, permettant une prise de contrôle de compte via Session Hijacking *(Élevée)*  

### Les jetons de réinitialisation de mot de passe sont-ils cryptographiquement sécurisés et imprévisibles ?
- [ ] Oui — les jetons ont une entropie élevée, sont aléatoires et **ne sont pas prévisibles**  
- [ ] Oui — les jetons sont générés mais présentent une faible entropie ou des motifs prévisibles (ex : basés sur l'horodatage)  
- [ ] Non — les jetons sont **prévisibles** ou basés sur des données utilisateur statiques comme l'e-mail/ID encodé en Base64 *(Critique)*  

### Le lien de réinitialisation de mot de passe peut-il être redirigé via Host Header Injection ?
- [ ] Non — l'application ignore ou valide l'en-tête `Host` pour la génération du lien  
- [ ] Oui — l'application utilise l'en-tête `Host` ou `X-Forwarded-Host` pour construire les liens, mais une validation **est présente**  
- [ ] Oui — les liens **peuvent** être redirigés vers un domaine contrôlé par l'attaquant via la manipulation d'en-têtes *(Élevée)*  

### Le jeton de réinitialisation a-t-il une durée de vie limitée et une invalidation immédiate ?
- [ ] Oui — les jetons expirent rapidement et sont invalidés **immédiatement** après utilisation ou changement de mot de passe  
- [ ] Oui — les jetons expirent après une longue durée (ex : 24h+) ou permettent **plusieurs utilisations**  
- [ ] Non — les jetons **n'expirent jamais** ou restent valides après que le mot de passe a été modifié avec succès  

### Existe-t-il des limites de débit (Rate Limiting) ou des verrouillages sur les points de terminaison de réinitialisation et de changement de mot de passe ?
- [ ] Oui — un Rate Limiting strict et des CAPTCHAs **sont appliqués** pour empêcher l'énumération et le Brute Force  
- [ ] Oui — des contrôles limités existent mais un contournement **est possible** via la rotation d'IP ou le spoofing d'en-tête  
- [ ] Non — aucun Rate Limiting **n'est appliqué**, permettant l'énumération de comptes en masse ou le Brute Force de jetons  

---