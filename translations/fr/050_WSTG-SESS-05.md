## WSTG-SESS-05 — Test de Cross-Site Request Forgery (CSRF)

Le Cross-Site Request Forgery (CSRF) est une vulnérabilité par laquelle un attaquant trompe le navigateur d'une victime pour lui faire exécuter une action non souhaitée sur un site web différent sur lequel la victime est actuellement authentifiée. Cette exploitation tire parti du comportement du navigateur qui joint automatiquement des identifiants « ambiants », tels que des cookies de session ou des en-têtes Authorization, aux requêtes sortantes. Les attaquants ciblent généralement des opérations modifiant l'état (state-changing), comme le changement de mots de passe, la mise à jour d'adresses e-mail ou l'exécution de transferts financiers, en hébergeant des scripts malveillants ou des formulaires cachés sur un site tiers. Une exploitation réussie peut mener à une prise de contrôle totale du compte (Account Takeover) ou à une modification non autorisée des données à l'insu de l'utilisateur ou sans son consentement.


| Champ | Valeur |
|---|---|
| **WSTG ID** | WSTG-SESS-05 |
| **CWE** | CWE-352 |
| **Statut du test** | Non effectué |
| **Sévérité** | Moyenne / Élevée* |

> *La sévérité devient Élevée si l'action vulnérable permet la prise de contrôle d'un compte, une élévation de privilèges ou des transactions financières non autorisées.

**Références :**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/05-Testing_for_Cross_Site_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/csrf-cross-site-request-forgery.html  
* https://portswigger.net/web-security/csrf  

**Outils :** `Burp Suite Professional (CSRF PoC Generator)`, `OWASP ZAP`, `CSRFTester`, `python3 (SimpleHTTPServer for PoC hosting)`

### Des jetons anti-CSRF sont-ils implémentés pour les requêtes modifiant l'état ?
- [ ] Oui — des jetons (tokens) uniques et cryptographiquement forts sont requis pour toutes les actions modifiant l'état  
- [ ] Oui — des jetons sont présents mais ne sont **pas** uniques par session ou sont prévisibles  
- [ ] Non — les jetons anti-CSRF ne sont **pas** implémentés  

### La validation côté serveur du jeton anti-CSRF est-elle robuste ?
- [ ] Oui — le serveur valide strictement le jeton et aucun contournement (bypass) n'est **possible**  
- [ ] Oui — la validation est effectuée mais **peut** être contournée en supprimant le paramètre du jeton  
- [ ] Oui — la validation est effectuée mais **peut** être contournée en fournissant un jeton vide ou factice  
- [ ] Oui — la validation est effectuée mais le jeton n'est **pas** lié à la session de l'utilisateur  

### L'application s'appuie-t-elle sur des méthodes de protection CSRF facilement contournables ?
- [ ] Non — l'application ne s'appuie pas uniquement sur des en-têtes faibles ou des vérifications d'origine  
- [ ] Oui — la protection repose uniquement sur l'en-tête `Referer` ou `Origin` qui **peut** être usurpé ou supprimé  
- [ ] Oui — la protection repose sur la vérification de l'en-tête `X-Requested-With` qui **peut** être contournée via des mauvaises configurations CORS  

### Les cookies de session sont-ils configurés avec l'attribut `SameSite` ?
- [ ] Oui — `SameSite` est défini sur `Strict` ou `Lax` pour tous les cookies liés à la session  
- [ ] Non — l'attribut `SameSite` est **manquant**, ce qui laisse le comportement par défaut au navigateur  
- [ ] Non — `SameSite` est explicitement défini sur `None` sans l'attribut `Secure` ou mesures d'atténuation supplémentaires  

### La protection CSRF peut-elle être contournée en changeant la méthode de requête HTTP ?
- [ ] Non — la protection est appliquée quelle que soit la méthode HTTP utilisée  
- [ ] Oui — les jetons ne sont validés que sur les requêtes `POST`, mais l'action **peut** être effectuée via `GET`  
- [ ] Oui — le changement de méthode (par ex. vers `PUT` ou `DELETE`) contourne la vérification du jeton  

---