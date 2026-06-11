## WSTG-ATHN-05 — Test de la fonctionnalité « Se souvenir de moi » vulnérable

La fonctionnalité « Se souvenir de moi » (Remember Me) ou d'authentification persistante est conçue pour maintenir l'état authentifié d'un utilisateur après le redémarrage du navigateur en stockant un jeton (token) ou un identifiant côté client. Si cette fonctionnalité est mal implémentée — par exemple en stockant des mots de passe en clair, des hachages (hashes) faibles ou des jetons à faible entropie dans des cookies — elle expose l'utilisateur à une prise de contrôle de compte (Account Takeover) via Cross-Site Scripting (XSS), un accès physique ou une interception réseau. Les testeurs d'intrusion (pentesters) évaluent l'entropie de ces jetons de persistance, les indicateurs de sécurité (flags) appliqués au mécanisme de stockage et le cycle de vie de la session pour s'assurer que la commodité de l'accès persistant ne contourne pas les contrôles de sécurité critiques tels que l'authentification multi-facteurs (MFA) ou l'invalidation de session. L'exploitation implique souvent la collecte de ces jetons à longue durée de vie pour obtenir un accès non autorisé aux comptes sans nécessiter les identifiants d'origine.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-ATHN-05 |
| **CWE** | CWE-522 |
| **État du test** | Non réalisé |
| **Sévérité** | Moyenne / Haute* |

> *La sévérité devient Haute si des identifiants en clair ou des hachages non salés sont stockés dans le cookie persistant, ou si le jeton permet de contourner l'authentification multi-facteurs (MFA).

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/05-Testing_for_Vulnerable_Remember_Password  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  
* https://portswigger.net/web-security/authentication  

**Outils :** `Burp Suite (Cookie Editor/Sequencer)`, `Browser Developer Tools`, `EditThisCookie`

### L'application implémente-t-elle une fonctionnalité « Se souvenir de moi » ou « Rester connecté » ?
- [ ] Non — la fonctionnalité **n'existe pas** dans l'application  
- [ ] Oui — la fonctionnalité existe et utilise des jetons cryptographiquement forts et imprévisibles  
- [ ] Oui — la fonctionnalité existe mais utilise des jetons prévisibles ou à faible entropie *(Moyenne)*  
- [ ] Oui — la fonctionnalité existe et stocke des identifiants en clair ou des mots de passe encodés en base64 *(Haute)*  

### Les indicateurs de sécurité (flags) sont-ils correctement appliqués au cookie persistant ?
- [ ] Oui — les flags `HttpOnly`, `Secure` et `SameSite` sont **appliqués**  
- [ ] Oui — certains flags sont appliqués mais `HttpOnly` est **manquant**  
- [ ] Oui — certains flags sont appliqués mais `Secure` est **manquant** sur HTTPS  
- [ ] Non — aucun indicateur de sécurité n'est **appliqué** au cookie persistant  

### Le jeton « Se souvenir de moi » reste-t-il valide après une déconnexion manuelle ?
- [ ] Non — le jeton est invalidé côté serveur immédiatement après la déconnexion  
- [ ] Oui — le jeton reste valide mais nécessite un mot de passe pour les actions sensibles  
- [ ] Oui — le jeton reste valide et permet un accès complet au compte **sans** réauthentification *(Moyenne)*  

### La session persistante est-elle interrompue lors d'un changement de mot de passe ?
- [ ] Oui — tous les jetons persistants sont révoqués lorsque l'utilisateur change son mot de passe  
- [ ] Non — le jeton « Se souvenir de moi » reste valide après qu'un changement ou une réinitialisation de mot de passe **a été effectué** *(Haute)*  

### Le jeton persistant contourne-t-il l'authentification multi-facteurs (MFA) lors des visites ultérieures ?
- [ ] Non — la MFA est requise même lorsqu'un jeton « Se souvenir de moi » est présent  
- [ ] Oui — la MFA est contournée, mais le jeton est lié à une empreinte numérique d'appareil (device fingerprint) spécifique  
- [ ] Oui — la MFA est **entièrement contournée** en utilisant uniquement le cookie persistant depuis n'importe quel appareil *(Haute)*  

---