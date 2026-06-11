## WSTG-ATHN-10 — Test d'authentification affaiblie dans les canaux alternatifs

Le test d'authentification affaiblie dans les canaux alternatifs consiste à identifier et à analyser les voies secondaires d'accès aux comptes—telles que les API mobiles, les workflows de récupération de mot de passe, les interfaces du centre d'assistance ou les sous-domaines hérités (Legacy)—qui pourraient ne pas appliquer la même rigueur sécuritaire que le portail web principal. Ces canaux alternatifs manquent souvent d'authentification multi-facteurs (MFA) robuste, de limitation de débit (Rate Limiting) stricte ou d'exigences de complexité de mot de passe, créant ainsi un « maillon faible » qui compromet l'ensemble du système d'authentification. Les attaquants ciblent spécifiquement ces points d'entrée négligés pour effectuer du bourrage d'identifiants (Credential Stuffing) ou contourner la MFA en exploitant les divergences dans la manière dont les différentes interfaces gèrent la vérification d'identité. Garantir la parité sur toutes les surfaces d'authentification est essentiel pour empêcher les accès non autorisés et maintenir l'intégrité de la session et des données de l'utilisateur.

| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-ATHN-10 |
| **CWE** | CWE-287 |
| **Statut du test** | Non effectué |
| **Sévérité** | Élevée |

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/10-Testing_for_Weaker_Authentication_in_Alternative_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Outils :** `Burp Suite`, `Postman`, `cURL`, `MobSF`, `Ffuf`

### Des canaux d'authentification alternatifs sont-ils présents et identifiés ?
- [ ] Non — un seul canal d'authentification existe  
- [ ] Oui — plusieurs canaux identifiés (par exemple : API d'application mobile, client de bureau, portail hérité, services SOAP)  

### Les canaux alternatifs appliquent-ils une parité de sécurité avec le canal principal ?
- [ ] Oui — tous les canaux imposent une complexité de mot de passe et des exigences MFA **identiques**  
- [ ] Oui — les canaux imposent la complexité du mot de passe mais la MFA n'est **pas requise** ou **peut** être contournée  
- [ ] Non — les canaux alternatifs ont des exigences d'authentification **nettement plus faibles**  

### La limitation de débit (Rate Limiting) et le verrouillage de compte sont-ils cohérents sur tous les canaux ?
- [ ] Oui — la limitation de débit et le verrouillage sont **appliqués de manière cohérente** sur tous les points de terminaison  
- [ ] Oui — des contrôles sont présents mais un contournement **est possible** sur des canaux spécifiques (par exemple : API mobile)  
- [ ] Non — la limitation de débit ou le verrouillage n'est **pas appliqué** aux voies d'authentification secondaires  

### La MFA peut-elle être contournée en passant à un canal alternatif ?
- [ ] Non — la MFA est obligatoire et **ne peut pas** être contournée quel que soit le point d'entrée  
- [ ] Oui — la MFA est requise sur le portail web mais **n'est pas appliquée** sur l'API mobile ou l'API héritée  

### Les flux de récupération de mot de passe ou de « mot de passe oublié » sont-ils plus faibles que la connexion standard ?
- [ ] Non — les flux de récupération utilisent une vérification hors bande (out-of-band) forte et ne divulguent pas d'informations  
- [ ] Oui — les flux de récupération utilisent des questions de sécurité faibles qui **peuvent** être facilement devinées ou recherchées  
- [ ] Oui — les flux de récupération sont vulnérables à l'énumération ou **ne requièrent pas** le même niveau de preuve qu'une connexion standard  

---